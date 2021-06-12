---
layout: post
title:  "Automating gRPC services with Scala and akka-grpc"
date:   2021-06-12 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210612/grpc-pic.jpg"
description: "How to automate gRPC based services in Scala"
summary: "How to automate gRPC based services in Scala"
header-img: "img/20210612/grpc-pic.jpg"

---

Photo by <a href="https://unsplash.com/@jannerboy62?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Nick Fewings</a> on <a href="https://unsplash.com/s/photos/communication?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## What is gRPC?

[gRPC][grpc] - is a modern Remote Procedure Call framework created and popularized by Google but now is under [CNCF][cncf] license.  

The main idea of the framework is to provide for clients, written in various languages, a possibility to execute remote calls to the server as if it is a local call.  

![Project Structure]({{ site.baseurl }}/img/20210612/grpc.png) 
    Photo from <a href="https://grpc.io/docs/what-is-grpc/introduction/">grpc.io docs</a>

The core concept of gRPC is that all communication between server and client is based on [Protocol Buffers][protocol-buffers] defined in proto files. The server will implement and support this interface, and the client will generate and use stub code. 

More about gRPC and concepts - [here][grpc-docs].
  
## Introducing: Greeter service

gRPC service example is taken from the official [akka-grpc guide][grpc-guide].  

[server-grpc-example][server-repo] repository contains a proto file with a service definition and its implementation. You can execute it on a local machine by running: 

```console
$ sbt runMain io.grpc.examples.helloworld.GreeterClient
```

By default, the server will start at **localhost:8080**. 

Protobuf definition for the service is the following:

```protobuf
syntax = "proto3";
import "google/protobuf/timestamp.proto";

option java_multiple_files = true;
option java_package = "example.myapp.example.myapp.helloworld.grpc";
option java_outer_classname = "HelloWorldProto";

service GreeterService {
  rpc SayHello(HelloRequest) returns (HelloReply) {}

  rpc ItKeepsTalking(stream HelloRequest) returns (HelloReply) {}

  rpc ItKeepsReplying(HelloRequest) returns (stream HelloReply) {}

  rpc StreamHellos(stream HelloRequest) returns (stream HelloReply) {}
}

message HelloRequest {
  string name = 1;
}

message HelloReply {
  string message = 1;
  google.protobuf.Timestamp timestamp = 2;
}
```

As you can see, Greeter Service has four methods:  

- **SayHello()** method for unary calls: a client sends a single request to the server and gets a single response back  
- **ItKeepsTalking()** method for client streaming: the client will send a bunch of messages to the server and after that will wait for a single reply back
- **ItKeepsReplying()** method for server streaming: the client will read a stream of messages from a server until there are no more new messages.  
- **StreamHellos()** method for bi-directional communication: the client and the server send a stream of messages to each other. 

## Automate by consuming proto files
Test automation process for gRPC based service consists of the following steps:  

1. Add sbt plugin for [Akka gRPC][akka-grpc] - create **plugins.sbt** file with the following content in the **repo/project** folder.  

    ```scala
    addSbtPlugin("com.lightbend.akka.grpc" % "sbt-akka-grpc" % "2.0.0")
    ```
2. Add dependencies to **build.sbt** and update the project.  

    ```scala
    enablePlugins(AkkaGrpcPlugin)

    libraryDependencies += "org.scalatest" %% "scalatest" % "3.3.0-SNAP2" % Test
    ```

3. Get all proto files from the [server][server-repo] repository. In our case, it will be just copying [hello_world.proto][proto] file from server repo and pasting it to src/main/protobuf directory. 
    
    As an improvement, proto files can(and should!) be automatically downloaded from the server or central proto file repository to all client repositories. It prevents the client from synchronization errors as the development of the server functionality evolves.  

4. Generate gRPC clients from proto files. As our build system in sbt, you need to execute the command  

    ```console
    $ sbt compile
    ``` 
    in Terminal or choose it from sbt command list in IDE. 
    As a result all clients generated from proto file can be found at */target/scala-2.13/akka-grpc/main* folder.   

5. Add [Matchers][matchers] and [ScalaFutures][scala-futures] traits to test class definition.  

    ```scala 
    class GreeterServiceApiTest extends AnyFlatSpec with Matchers with ScalaFutures {
    ``` 

6. Configure default waiting timeout for responses.   

    ```scala
    implicit override val patienceConfig: PatienceConfig =
        PatienceConfig(timeout = Span(3, Minutes), interval = Span(10, Millis))
    ```

7. Add configuration for gRPC client.  

    ```scala
    val host = "127.0.0.1"
    val port = 8080

    implicit protected val system: ActorSystem = ActorSystem("api-test")

    val clientSettings: GrpcClientSettings = GrpcClientSettings.connectToServiceAt(host, port).withTls(false)

    val client: GreeterService = GreeterServiceClient(clientSettings)
    ```

8. Implement the tests  

    ```scala
    "Greeter Service" should "handle unary requests and response" in {
        val name = getRandomValue
        val response = client.sayHello(HelloRequest(name)).futureValue

        response.getTimestamp should not be null
        response.message should be(s"Hello, $name")
    }

    "Greeter Service" should "handle client streaming" in {
        val data = getRandomList(3)
        val response = client.itKeepsTalking(Source(data.map(HelloRequest(_)))).futureValue

        response.timestamp should not be null
        response.message should be(s"Hello, ${data.mkString(", ")}")
    }

    "Greeter Service" should "handle server streaming" in {
        val name = getRandomValue
        val responses = client.itKeepsReplying(HelloRequest(name)).runWith(Sink.seq).futureValue

        responses should not be empty
        val messages = responses.map(_.message).toList
        all(messages) should not be ""
    }

    "Greeter Service" should "handle bi-directional streaming" in {
        val data = getRandomList(3)
        val responses = client.streamHellos(Source(data.map(HelloRequest(_)))).runWith(Sink.seq).futureValue

        responses should not be empty
        val messages = responses.map(_.message).toList
        all(messages) should not be ""
    }
    ```
    Pay attention, that we can't get reponse straight away - we need to wait until Future completes and than get the data by executing **.futureValue**.  

    Matchers library provides a fluent api for writing assertions - for single values as well as for collections of elements.

9. Execute the tests either from IDE or from Terminal  

    ```console 
    $ sbt test
    ```

## Conclusions
Automation of gRPC-based services is a little bit similar to automating [SOAP-based][soap] services. In both cases, you have some utilities for generating clients as a code (**ws-consume** for SOAP and **akka-grpc** or **scalapb** for gRPC).  

After you generate a client - an overall approach is the same as for any API tests - firing a request with test data and asserting responses.  

gRPC brings a bit more complexity with client, server, and bi-directional streaming - but you can handle it without any problems using futures and collecting all responses in Lists.  

As always, all code is available in [github-repo][client-repo]

[grpc]: https://grpc.io/
[cncf]: https://www.cncf.io/
[protocol-buffers]: https://developers.google.com/protocol-buffers/docs/overview
[grpc-docs]: https://grpc.io/docs/
[grpc-guide]: https://doc.akka.io/docs/akka-grpc/current/server/index.html
[akka-grpc]: https://doc.akka.io/docs/akka-grpc/current/index.html
[proto]: https://github.com/alexromanov/server-grpc-sample/blob/main/src/main/protobuf/hello_world.proto
[matchers]: https://www.scalatest.org/user_guide/using_matchers
[scala-futures]: https://docs.scala-lang.org/overviews/core/futures.html
[soap]: https://ru.wikipedia.org/wiki/SOAP
[server-repo]: https://github.com/alexromanov/server-grpc-sample
[client-repo]: https://github.com/alexromanov/client-grpc-sample