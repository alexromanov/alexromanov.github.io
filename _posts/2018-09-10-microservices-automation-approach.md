---
layout: post
title:  "Tuning up test pyramid for microservices"
date:   2018-09-10 08:00:47 +0300
author: "Oleksandr Romanov"
description: "How canonical testing pyramid is changing as more and more applications adopt microservices architecture"
header-img: "img/20180910/pyramid3.jpg"

---

_This blog post is a starting point for a series regarding testing and automating microservices based systems._

### Good old pyramid  

Maybe, this is the most widespread scheme which is used in a lot of talks about testing and specifically, test automation. 

![Project Structure]({{ site.baseurl }}/img/20180910/old_pyramid.png)

Yes - it is a test automation 'pyramid' (or triangle, if you wish :)) proposed by Mike Cohn in his book ['Succeeding with Agile'][agile].  

Since 2009, it is a 'golden standard', when we speak about the right and effective ways of structuring our automated tests.  

According to the pyramid, there should be a lot of unit tests, then fewer amount of integration (service) tests. As for UI tests - their amount should always as minimum as possible.  

For monolithic based applications the pyramid suits well.  

But what about microservices?  

### Microservices - here and now... 

Microservices became really a mainstream type of architecture in a recent years. It is used for building a complex, loosely coupled and reliable backend systems.  

![Project Structure]({{ site.baseurl }}/img/20180910/micros.png)

In a few words: instead of a huge monolithic backend system, which holds all the businees logic and databases, now we have a bunch of independently deployable services, which shares a part of business logic each.  

More about microservices itself and a proper way to build it - you can find in this blog post by Martin Fowler - [Microservices][microservices]. Or in the beautiful reference books written by Sam Newman - [Building Microservices][building]

### Adapting pyramid to new conditions  

As we, as test engineers, are going to test such systems and what is also important - to automate such systems - we need to revise our current approach to structuring test levels.  

Each microservice now can be represent as an independent system, which needs to be tested at various levels: unit, integration, component.  

Also we need to test an integration between services.  

And in the end - we also need to verify that the system as a whole is working as expected. Thus, we need to implement end - to - end tests for API or UI levels.  

As a result, our scheme for testing levels now is slightly different in the realm of microservices.   

![Project Structure]({{ site.baseurl }}/img/20180910/new_pyramid.png)

And yes - this is not a pyramid or even a triangle.  

According to this scheme, we need to test each microservice alone (involving real or mock dependencies).  

We will use contract testing for testing integration between the services.  

Also, we need to remember that our testing approach will not be so effective without any manual testing for our applications, such as exploratory, UX, etc. These types of testing should be performed as well.   

### Conclusions 

In the next posts I will share some practical tips on how to write automated tests for microservices at different levels.
We will concentrate mostly on Spring Boot framework and Java programming language, but also make a step back - to try other languages and approaches.  


[microservices]: https://martinfowler.com/articles/microservices.html
[building]: https://samnewman.io/books/building_microservices/
[agile]: https://www.amazon.com/Succeeding-Agile-Software-Development-Using/dp/0321579364
