---
layout: post
title:  "Course review: Java and Spring courses at Pluralsight"
date:   2020-04-26 00:00:49 +0300
author: "Oleksandr Romanov"
description: "Review of Java courses which I've recently completed at Pluralsight"
header-img: "img/20200426/psfree.jpg"

---

## The Why

In April 2020, a [Pluralsight][Pluralsight], an online learning platform, has offered free access to more than 7000+ courses during the whole month.  
So I thought it worth trying to use the platform and get new skills in my toolset.  

Below is my review on some Pluralsight courses on Java and Spring framework as well as a brief overview of the platform functionality.  
I hope that it will help you to choose the next courses for education.  

## Courses

![Project Structure]({{ site.baseurl }}/img/20200426/list.jpg)

My primary goal was to get more knowledge in Java development and the Spring framework.  

Here is the list of courses which I completed with their pros and cons.

### JPA and Spring

["Java Persistence API 2.2"][JPA1] by Antonio Goncalves

- pros:
  - Nice course for getting initial knowledge about Java Persistence API (as a part of Java EE)
  - The author clearly explains why JPA has been created and which benefits it provides
  - Each concept is explained with code samples
- cons
  - Not found

["Spring Framework: Spring Data JPA"][JPA2] by Dan Bunker

- pros:
  - The course suits well as an extension to the JPA course from A.Goncalves
  - Dan Bunker briefly explains the history of Spring Data JPA and goes straight to the main functionality
  - JPA functionality explained on the code samples as a form of refactoring from traditional JPA to Spring Data JPA (beware that a lot of boilerplate code will be removed)
- cons
  - Not found

### Java Virtual Machine

["Understanding the Java Virtual Machine: Security"][JVM1] and
["Understanding the Java Virtual Machine: Memory Management"][JVM2] by Kevin Jones

- pros:
  - Explanation of Garbage Collection process in Java, types of Garbage Collectors and algorithms of their work (including visualizations)
  - Samples on how to switch between collectors and how to use Java tools for JVM monitoring: JStat, jvisualvm (with VisualGC plugin)
  - Theory of different types of references in Java: Strong, Soft, Weak, Phantom and also use cases for each one
  - Nice explanation on Java code security, permissions, policy files (including custom ones)
- cons
  - For Memory Management - not found
  - Security concepts may not be applicable for an average project

### Concurrency in Java

["Applying Concurrency and Multi-threading to Common Java Patterns"][Concurrency1] and ["Advanced Java Concurrent Patterns"][Concurrency2] by Jose Paumard

- pros:
  - Great intro to concurrency in Java, including topics like threading, thread states, synchronization, concurrency issues, 'happens-before' linking
  - Examples of basic concurrency patterns, such as Consumer/Producer and also how to implement Singleton pattern in a multithreaded environment
  - Great overview of ExecutorService, different types of locks (Reentrant, ReadWrite), as well as Semaphores, CyclicBarriers
  - Explanation of "Compare and Swap" problem and how Atomic variables help to solve it
  - Overview of basic concurrent data structures in Java: ArrayBlockingQueue, ConcurrentLinkedQueue, ConcurrentHashMap, ConcurrentSkipList
  - Wonderful code samples for each concept explained
- cons
  - Not found

### Apache Kafka

["Getting Started with Apache Kafka"][Kafka] by Ryan Plant

- pros:
  - One of the best introductory course I've ever seen - in terms of the material structure
  - The content includes: distributed messaging concepts, Kafka architecture, consumer/producers purpose, offsets, consumer groups
  - Real code examples of Kafka producer/consumer implementation in Java using Kafka API
- cons
  - Not found

### Software engineering

["SOLID Software Design Principles in Java"][SOLID] by Dan Geabunea

- pros:
  - Great course for a beginner level developers
  - A clear explanation of each principle (in an easy to understand terms)
  - Code examples on how to apply principles and also what happens if principles are violated
- cons
  - The limited number of code examples

["Java Refactoring: Best Practices"][Refactoring] by Andrejs Doronins

- pros:
  - A good overview of common refactoring process and code smells
  - Nice way of explanation for each code smell: definition and which refactoring technique can fix it
  - Code examples for each code smell
  - Few hints on using IDE and plugins for refactoring
- cons
  - Not found

### Testing

My attention caught the course ["Mutation Testing in Java with Pitest"][PITEST] by Esteban Herrera

- pros:
  - A structured overview of the concept of mutation testing, as well as the history of mutation testing tools in Java
  - Clear examples on how to add PiTest to the project and execute it
  - An overview of mutators, operators groups and how to configure them using Maven plugins
- cons:
  - The course seems a little bit fast-paced
  - Code examples can be more explanatory
  - PiTest with the multi-module project was not covered

## The platform

![Project Structure]({{ site.baseurl }}/img/20200426/psprofile.png)

**Pluralsight** offers not only a single separate course on topics like Software Development, but IT also OPS, Cyber Security, Design, Architecture, and Business.  

### Functionality

The user additionally can:

- choose a certain "Path" (collection of courses on a given topic, like Spring or Java EE)
- test knowledge on a given topic and get course recommendations based on a skill level
- set personal goals on how much learning needs to be accomplished in a week
- prepare for the certification (mostly security or cloud) and pass the exam

**Basic courses are definitely lack of practice:** by default it looks like you are watching a conference talk with samples. But as I understood, there is a separate type of courses called **"Interactive"** where you can really build real projects led by an instructor.  **"Interactive"** courses have not been freely accessible, so I had not tried it for now.

### Content

The courses on most widespread programming languages can be easily found: from introductory to advanced level.  
C# developers fill to get the most of it, because C# and ASP.NET course are the majority of the content (295), comparing to others (Java - 153, Python - 180, Node.js - 85).
Another major topic, which is covered by a various number of courses - is a Cyber Security.  

## Conclusion

Pluralsight is a good platform for getting online training on certain topics of software development, security, and other. One of the most widespread use cases for it is a corporate subscription for developer's education.

The basic courses will provide a good "starting ground" for the subject, but more in-depth knowledge or getting the practical experience you should find other resources or books.  

Have you already used Pluralsight?

[Pluralsight]: https://www.pluralsight.com/
[JPA2]: https://app.pluralsight.com/library/courses/spring-data-jpa-getting-started
[JPA1]: https://app.pluralsight.com/library/courses/java-persistence-api-21
[PITEST]: https://app.pluralsight.com/library/courses/mutation-testing-java-pitest
[Kafka]: https://app.pluralsight.com/library/courses/apache-kafka-getting-started
[JVM1]: https://app.pluralsight.com/library/courses/understanding-java-vm-memory-management
[JVM2]: https://app.pluralsight.com/library/courses/understanding-java-vm-security
[Refactoring]: https://app.pluralsight.com/library/courses/java-refactoring-best-practices
[SOLID]: https://app.pluralsight.com/library/courses/solid-software-design-principles-java
[Concurrency1]: https://app.pluralsight.com/library/courses/java-patterns-concurrency-multi-threading
[Concurrency2]: https://app.pluralsight.com/library/courses/java-concurrent-patterns-advanced
