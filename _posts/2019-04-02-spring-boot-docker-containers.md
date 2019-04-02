---
layout: post
title:  "Easy way to test Spring Boot microservices"
date:   2019-04-02 09:00:47 +0300
author: "Oleksandr Romanov"
description: "Easy way how to make integration and component testing for Spring Boot microservices with Docker containers"
header-img: "img/20190402/docks.jpg"

---
**Audience: SDET, Java developers**

### Managing dependencies for microservices  

In one of my [previous blog posts][microservicestesting] I mentioned, that moving an application from monolith to microservices should also lead to transformation of approaches to automated testing.  

![Project Structure]({{ site.baseurl }}/img/20180910/new_pyramid.png)

According to the scheme, the largest part of testing efforts should be concentrated on the level of single microservice. It includes unit, integration, contract and component tests.  

But in the reality, it is rare situation, when microservice is not using any dependencies, such as SQL and NoSQL databases, message brokers (e. g. [Apache Kafka][kafka]) and other internal or external components.  

All of these dependencies can be grouped into two main categories: services, which are somehow configurable for testing purposes and services, which are completely "black - box" for us.  

In case of the "black - boxed" services it is possible to write [mocks][mocks] on our side and use them during testing. It gives us a certain amount of confidence in the quality of the service, but it will limit testing to our **assumptions on how external services are working**.  

Another category of dependencies, which need to be tested - are systems, which are parts of our service and almost completely configurable for development, production and testing needs.  

Such dependencies can be successfully used for component and integration tests of the microservice.  

**So what is our straightforward approach?** 

![Project Structure]({{ site.baseurl }}/img/20190402/oneway.jpg)

One of the ways is install desired databases, message queues, etc - on our local computer, configure them and then implement automated component cases for microservice, which use these dependencies as components.  

**Looks very cool and easy from the first sight. Are there any cons against it?**

1. If service is using SQL, NoSQL databases and message broker simultaneously - it is required to install and configure all software on your local machine. Each developer or tester in your team should also do the same thing for his or her computer. **Every.** **Time.** 

2. The previous point can be overcomed by using a separate cloud-based storage, where we can deploy and set up required software for testing. But the problem is that installation should be done for each component of each microservice. What if amount of microservices at your application is close to 100? To 1000?  

3. No matter which way you choose - external dependencies require updates from time to time.  

4. If your microservices are truly language agnostic - there will be a need to configure dependencies which come from completely different technology stacks, which potentially cause compatibility issues.

**Maybe, there is another way to do it more flexible ...?**  
 
![Project Structure]({{ site.baseurl }}/img/20190402/alternative.jpg)

**Perhaps, we do not need to install and configure all this software...?**  

### Docker to the rescue

Containers and [Docker][docker] itself are not a buzzwords in the modern software - engineering world. Since their launch in 2013, they have become the "gold" standard of deploying applications in production environments. In addition to Docker, many useful tools for Docker container's orchestration also appeared, such as [Kubernetes][kubernetes], [Docker Swarm][dockerswarm] and many others.  

But Docker containers are used not only development services or deploying them to production.  

They can be successfully used for test automation. As a prime example, if you want to run UI automated tests written with Selenium Webdriver and keep each browser independent and controllable - you should check out the tool called [Selenoid][selenoid].  

***But what are the main benefits of using Docker for integration and component testing of Spring Boot based microservices?*** 

It turns out that there are a lot:  

 - there is no need to install many additional external databases, brokers or other systems on your local machine  

 - dependency versions and updates are automatically resolved by using the latest versions of container images

 - it is possible to configure each dependency before usage in test
  
### Test containers for Spring Boot

In order to make work with Docker in Spring Boot based microservices even more easier, [Playtika][playtika] has been created small open - source library called [testcontainers-spring-boot][testcontainers].

All you need to do is to include a couple of dependencies into pom.xml:  

{% highlight xml %}
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter</artifactId>
        <scope>test</scope>
    </dependency>
{% endhighlight %}

For example, if you want container with [Apache Kafka][kafka] set up and ready **before each test**, you can add the following:  

{% highlight xml %}
    <dependency>
        <groupId>com.playtika.testcontainers</groupId>
        <artifactId>embedded-kafka</artifactId>
        <scope>test</scope>
    </dependency>
{% endhighlight %}

**How it works?**:  

 - target dependency with the test scope is included into pom.xml  

 - before each test (and before application context is loaded), the library searches for an image of the chosen system, downloads it, runs it on the local machine, configures it and provides properties for connection to microservice  

 - container with dependency is automatically shut down after the test  
 
 It is possible to include a set of dependencies at the same time: for example you can run microservice with [MariaDB (SQL)][maria], [Couchbase (NoSQL)][couch], [Kafka (message broker)][kafka] and [Zookeper][zoo]. Just include Maven dependencies to pom.xml file and.  

 ![Project Structure]({{ site.baseurl }}/img/20190402/component.jpg)

 For each system it is also possible to configure additional parameters (such as topics to create for Kafka or even custom docker image to download) by adding properties into bootstrap.properties file:

{% highlight xml %}
    embedded.kafka.topicsToCreate=my_custom_topic1,my_custom_topic2
    embedded.kafka.enabled=true
{% endhighlight %}

### Conclusions 

Modern microservices includes a lot of different internal components for storing data and transfer messages across application.  

In order to launch and test single microservice in isolation we need a simple and controlled way to manipulate with such components.  

[Testcontainers-spring-boot][testcontainers] library makes such testing a lot easier. As a result, it is possible to concentrate more on **what to test** rather than on **how to configure application under test**.  

[microservicestesting]: https://alexromanov.github.io/2018/09/10/microservices-automation-approach/
[testcontainers]: https://github.com/Playtika/testcontainers-spring-boot
[docker]: https://www.docker.com/
[kubernetes]: https://kubernetes.io/
[dockerswarm]: https://docs.docker.com/engine/swarm/
[selenoid]: https://aerokube.com/selenoid/latest/
[springboot]: https://spring.io/projects/spring-boot
[kafka]: https://kafka.apache.org/
[mocks]: https://martinfowler.com/articles/mocksArentStubs.html
[maria]: https://mariadb.org/
[couch]: https://www.couchbase.com/
[zoo]: https://zookeeper.apache.org/
[playtika]: https://www.playtika.com/