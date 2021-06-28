---
layout: post
title:  "Getting Started With Testing Microservices"
date:   2021-06-28 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210628/ms.jpg"
description: "Resources for engineers starting microservices testing"
summary: "Resources for engineers starting microservices testing"
header-img: "img/20210628/ms.jpg"

---

Photo by <a href="https://unsplash.com/@ryoji__iwata?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Ryoji Iwata</a> on <a href="https://unsplash.com/s/photos/pieces?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## Microservices testing: from the theory to practice  

In this blog post, I will share resources that help you to be more prepared to testing microservices-based systems.  

My background is Java and Spring Boot. Many tools and libraries that I will share are from the Java world. But you can find analogies in other technology stacks as well. 

I will split resources into the following sections - from the theory to practice:

* [Learn the basics](#learn-the-basics)
* [Explore technology stack](#explore-technology-stack)
* [Build test strategy](#build-test-strategy)
* [Learn tools](#learn-tools)
* [Investigate contract testing](#investigate-contract-testing)

## Learn the basics  

![Project Structure]({{ site.baseurl }}/img/20210628/micro.png)

The first thing is to understand microservices architecture and how it differs from more traditional monolith systems.

The following resources will help you to build initial information:
- **[Building Microservices](https://www.amazon.com/Building-Microservices-Designing-Fine-Grained-Systems/dp/1491950358)** book by Sam Newman
- **[What are microservices](https://youtu.be/j3XufmvEMiM)** video by freeCodeCamp
- **[Microservices overview](https://martinfowler.com/articles/microservices.html)** by Martin Fowler
- **[Microservices](https://www.ibm.com/cloud/learn/microservices)** at IBM Cloud
- **[The What, Why, and How of a Microservices Architecture](https://medium.com/hashmapinc/the-what-why-and-how-of-a-microservices-architecture-4179579423a9)** by Jetinder Singh

## Explore technology stack  

Resources (Spring Boot):
* **[Spring Boot](https://spring.io/projects/spring-boot)** - project documentation
* **[Learn Spring Boot - Rapid Spring Application Development](https://www.udemy.com/course/spring-boot-intro/)** course on Udemy
* **[Spring Test](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html)** library overview
* **[Testing WEB APIs](https://spring.io/guides/gs/testing-web/)** at Spring Boot

Of you extensively use virtualization tools, like Docker - learn it.  
Pay attention to how to start a single microservice in Docker container on your local machine (together with its dependencies: SQL, NoSQL databases, messaging systems, etc.):
- **[Advanced Functional Testing in Spring Boot Using Docker in Tests](https://dzone.com/articles/advanced-functional-testing-in-spring-boot-by-usin)**
- **[Easy Integration Testing With Testcontainers](https://mydeveloperplanet.com/2020/05/05/easy-integration-testing-with-testcontainers/)**

If you are not from the Java world - ask your fellow developers about technologies and frameworks they use for building microservices in your organization.  
Do not hesitate to ask which testing tools, practices, and guidelines they already use.  

## Build test strategy  

![Project Structure]({{ site.baseurl }}/img/20210628/pyramid.png)

Classic approach with unit, integration, and end-to-end testing can't be applied to microservices "as is." That's why approaches to testing need to be reworked and adapted.

Resources:
- **[Testing Strategies in a Microservice Architecture](https://martinfowler.com/articles/microservice-testing/)** by Toby Clemson
- **[The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)** by Ham Vocke
- **[Testing Microservices: an Overview of 12 Useful Techniques](https://www.infoq.com/articles/twelve-testing-techniques-microservices-intro/)** by Wojciech Bulaty and Liam Williams
- **[Testing Microservices, the sane way](https://copyconstruct.medium.com/testing-microservices-the-sane-way-9bb31d158c16)** - very big and highly informative article by Cindy Sridharan
- **[Descending the Testing Pyramid: Effective Testing Strategies for Microservices](https://www.slideshare.net/chris.e.richardson/oracle-codeone-2019-descending-the-testing-pyramid-effective-testing-strategies-for-microservices)** slides by Chris Richardson  

## Learn tools

Tools and libraries can greatly simplify testing and automation and boost developer productivity.  

Resources:  

- **[Wiremock](http://wiremock.org/)** for mocking external API (API virtualization)
- **[Swagger](https://swagger.io/)** for API visualization, research, and testing. Ideally, each microservices should provide an API for public and admin usage
- **[In-memory databases](https://www.baeldung.com/java-in-memory-databases)** will help you to prepare service dependencies faster and make them more reliable for unit testing
- **[Testcontainers](https://www.testcontainers.org/)**. The incredible library automatically runs service dependencies (databases, messaging, other services) in a separate docker containers. As a result, you will have a "real-world" setup for service component tests - but with much more control over dependencies.
- **[Rest Assured](https://rest-assured.io/)** for API test automation. It is just one of the possible tools: do not hesitate to try something else and find a tool suitable for particular purposes

## Investigate contract testing  

![Project Structure]({{ site.baseurl }}/img/20210628/contracts.png)

When the number of microservices is increasing to hundreds or thousands, the problems of integration arises:
- if I deploy this API change - which microservices will I break?
- which services depend on each other?

Contract testing allows building another layer of fast and reliable tests, preventing integration issues as soon as possible in the development workflow.

Depending on the technologies used on your project, you may prefer:
- **[Spring Cloud Contract](https://spring.io/projects/spring-cloud-contract)**. It works best with Java and Spring ecosystem (but not limited to it)
- **[PACT tool](https://pactflow.io/)**. It supports multiple programming languages out-of-the-box 

If services use gRPC protocol for communication - **[you can use PACT for contract testing as well](https://medium.com/@ivangsa/consumer-driven-contract-testing-for-grpc-pact-io-d60155d21c4c).**  

Resources:  

- **[What is contract testing](https://pactflow.io/blog/what-is-contract-testing/)** by PACT developers
- **[An introduction to contract testing](https://www.ontestautomation.com/an-introduction-to-contract-testing-part-1-meet-the-players/)** blog post series by Bas Dijkstra
- **[Introduction to contract testing with Pactflow](https://www.youtube.com/watch?v=U05q0zJsKsU&list=PLwy9Bnco-IpfZ72VQ7hce8GicVZs7nm0i)** video series by PACT developers
- **[Practical Contract Testing with Spring Cloud Contract](https://youtu.be/_AYfxXJ7o20)** - my talk at TestCon Europe
- **[Spring Cloud Contract course](https://www.pluralsight.com/courses/spring-cloud-contract-introduction)**
- **[gRPC course](https://www.youtube.com/watch?v=XRXTsQwyZSU&list=PLt1SIbA8guusAJIBS8JgbSFKfQdVkWDbl)**

## Conclusions
Microservices architecture is not a "silver" bullet in software development. The benefits also come with drawbacks.  

For tester's point of view - **[testing microservices is like a testing system of systems](https://alexromanov.github.io/2018/09/10/microservices-automation-approach/)**: you start with single service verification in isolation, then move to integrate it with dependencies and other services, then you move to end-to-end API and UI tests as for any application.  

The biggest challenge here is that the previously described testing process needs to be applied to many different services. It is not fast and scalable to do it manually - so high investment in automation is a prerequisite to any successful microservices testing effort. 

This list of resources is not complete - it is just the best ones that helped me a lot in the past.  

What is your favorite book or blog post about microservices?
