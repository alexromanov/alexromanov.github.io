---
layout: post
title:  "Load testing for gRPC services with Gatling"
date:   2021-08-23 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210823/load.jpg"
description: "Examples of load tests for grpc services"
summary: "Examples of load tests for grpc services"
header-img: "img/20210823/load.jpg"

---

Photo by <a href="https://unsplash.com/@ricrawfo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Riley Crawford</a> on <a href="https://unsplash.com/s/photos/load?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## Load testing for gRPC - the case

I recently faced the task of doing simple load testing for a set of gRPC based services.
There are a lot of tools on the market, but not all of them support communication via gRPC.  

After searching the Internet for some time, I found a few options:

- [Gatling](https://gatling.io/) with [gRPC plugin](https://github.com/phiSgr/gatling-grpc).

    The plugin allows to test gRPC services by making single calls and client, server, and bi-directional streaming; Scripts are written in Scala language.

- [k6.io](https://k6.io/).

    k6 is a great tool for writing performance tests in Javascript. It supports gRPC unary calls by default. But for now, streaming is not supported at all. And it is not easy to add this functionality easily to the current implementation.

- [ghz](https://ghz.sh/).

    ghz is a simple command-line tool written in Go. You can write load tests by passing various command-line parameters to the tool. It also has nice reporting capabilities. But the project is completely open-source, so it is a bit worse than Gatling in terms of functionality. But for simple benchmarking, is a good tool to try. 

After investigating the options, I selected to try **Gatling** for my load tests.  

In this blog post, I want to show a few basic examples load tests for gRPC service. 

## The service under test

For our example, we will use simple gRPC service implementation from one of the [previous blog post](https://alexromanov.github.io/2021/06/12/scala-grpc-api-tests/). 

**Sample service API:**
- **SayHello()** method for unary calls: a client sends a single request to the server and gets a single response back  
- **ItKeepsTalking()** method for client streaming: the client will send a bunch of messages to the server and, after that, will wait for a single reply back
- **ItKeepsReplying()** method for server streaming: the client will read a stream of messages from a server until there are no more new messages.  
- **StreamHellos()** method for bi-directional communication: the client and the server send a stream of messages to each other. 

You can find the code of the server on [Github](https://github.com/alexromanov/server-grpc-sample). 

## Setting up the project

For load tests, we need to create a new [sbt](https://www.scala-sbt.org/) based project and add a few dependencies and plugins:

Gatling dependencies.

```scala
  val gatlingDependencies = Seq(
    "io.gatling.highcharts" % "gatling-charts-highcharts" % gatlingVersion % Test,
    "io.gatling" % "gatling-test-framework" % gatlingVersion % Test,
    "com.github.phisgr" % "gatling-grpc" % "0.11.1" % Test,
    "com.github.phisgr" % "gatling-ext" % "0.2.0" % Test
  )
```

gRPC dependencies.
```scala
  val grpcDependencies = Seq(
    "io.grpc" % "grpc-netty" % "1.36.0",
    "com.thesamet.scalapb" %% "scalapb-runtime-grpc" % scalapb.compiler.Version.scalapbVersion,
    "com.thesamet.scalapb" %% "scalapb-runtime" % scalapb.compiler.Version.scalapbVersion % "protobuf",
    "com.typesafe.akka" %% "akka-actor-typed" % "2.6.10",
    "com.typesafe.akka" %% "akka-protobuf-v3" % "2.6.10",
    "com.typesafe.akka" %% "akka-stream" % "2.6.10"
  )
```

Plugins.

```scala
// assembly
addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.9")

// gatling
addSbtPlugin("io.gatling" % "gatling-sbt" % "3.2.1")

// akka
addSbtPlugin("com.lightbend.akka.grpc" % "sbt-akka-grpc" % "1.1.1")

// protoc
addSbtPlugin("com.thesamet" % "sbt-protoc" % "0.99.33")
libraryDependencies += "com.thesamet.scalapb" %% "compilerplugin" % "0.10.11"
```

And as a final step - add plugin and compiler configuration to the **build.sbt**:
```scala
enablePlugins(GatlingPlugin)

PB.targets in Compile := Seq(
  scalapb.gen() -> (Compile / sourceManaged).value / "scalapb"
)
```

Do not forget to copy [service proto file](https://github.com/alexromanov/server-grpc-sample/blob/main/src/main/protobuf/hello_world.proto), put it in the src/main/protobuf folder and compile it in the console:
```scala
sbt compile
```

## The tests

### Here is the test for gRPC single call:
```scala
class GrpcSimulation extends Simulation {

  val scn = scenario("Make Hello Request and Get Response")
    .exec(grpc("Hello Request")
      .rpc(GreeterServiceGrpc.METHOD_SAY_HELLO)
      .payload(HelloRequest("Gatling Load Test"))
      .extract(_.message.some)(_ saveAs "message")
      .check(statusCode is Status.Code.OK)
    )
    .exec(grpc("Hello Request with parameter from session")
      .rpc(GreeterServiceGrpc.METHOD_SAY_HELLO)
      .payload(session => HelloRequest(session.attributes("message").asInstanceOf[String]))
      .check(statusCode is Status.Code.OK)
    )

  setUp(scn.inject(rampUsersPerSec(1) to (2) during (20 seconds)).protocols(grpcPsgConf.shareChannel))
}
```

In this example, we call SayHello() method, get a response message, save it to the internal scenario session, and then use it in the subsequent request. 
Here is how you can get a value from session:
```scala
// saving data to session
.extract(_.message.some)(_ saveAs "message")

// get data from session
.payload(session => HelloRequest(session.attributes("message").asInstanceOf[String]))
```

### Test for server streaming will look like this:

```scala
class ServerStreamingSimulation extends Simulation {
  val serverCall = grpc("Replying").serverStream("replier")

  val scn = scenario("Server Streaming Flow")
    .exec(serverCall.start(GreeterServiceGrpc.METHOD_IT_KEEPS_REPLYING)
    (HelloRequest("Gatling Load Test"))
      .extract(_.message.some)(_ saveAs "ServerReply")
      .sessionCombiner(SessionCombiner.pick("ServerReply"))
      .endCheck(statusCode is Status.Code.OK)
    )

  setUp(scn.inject(rampUsersPerSec(1) to (2) during (20 seconds)).protocols(grpcPsgConf.shareChannel))
}
```
Pay attention, that in order to save data to session object in the streaming simulation, you need not only extract data and save it, but also call sessionCombiner method to share session data between all messages in a stream.  


### gRPC bi-directional streaming:
```scala
class BiDiStreamingSimulation extends Simulation {
  val bidiCall = grpc("BiDi call").bidiStream("bidi")

  val scn = scenario("BiDi streaming")
    .exec(bidiCall.connect(GreeterServiceGrpc.METHOD_STREAM_HELLOS)
      .endCheck(statusCode is Status.Code.OK))
    .exec(bidiCall.send(HelloRequest("Gatling Load Test First")))
    .exec(bidiCall.send(HelloRequest("Gatling Load Test Second")))
    .exec(bidiCall.complete)

  setUp(scn.inject(rampUsersPerSec(1) to (2) during (20 seconds)).protocols(grpcPsgConf.shareChannel))
}
```
## Test execution and results

You can execute the tests by running commands in the console:
```scala
sbt "gatling:testOnly *.GrpcSimulation"
sbt "gatling:testOnly *.ServerStreamingSimulation"
sbt "gatling:testOnly *.BiDiStreamingSimulation"
```

You can find Gatling test reports in the /target/gatling folder.

![Project Structure]({{ site.baseurl }}/img/20210823/grpcGatling.png)

![Project Structure]({{ site.baseurl }}/img/20210823/serverGatling.png)

## Conclusions

Gatling with gRPC plugin - is a good choice for testing performance capabilities of gRPC services. 
But if you want to use this tool effectively - you need to know Scala and can read and understand Scala code well. 

As always, you can find all examples in the [Github repository.](https://github.com/alexromanov/gatling-grpc-tests-sample) 

