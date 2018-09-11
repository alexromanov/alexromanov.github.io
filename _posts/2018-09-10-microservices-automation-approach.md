---
layout: post
title:  "Tuning up test pyramid for microservices"
date:   2018-09-10 09:00:47 +0300
author: "Oleksandr Romanov"
description: "How canonical testing pyramid is changing as more and more applications adopt microservices architecture"
header-img: "img/20180910/pyramid3.jpg"

---

_This blog post is the first out of series dedicated to testing and automating microservices-based systems._

### Good old pyramid  

Maybe, this is the most widespread scheme, which is used in a lot of talks about testing and, specifically, test automation. 

![Project Structure]({{ site.baseurl }}/img/20180910/old_pyramid.png)

Yes - it is a test automation 'pyramid' (or triangle, if you wish :)) proposed by Mike Cohn in his book ['Succeeding with Agile'][agile].  

It is a 'golden standard' since 2009 when we are speaking about the right and effective ways of structuring our automated tests.  

According to the pyramid, the number of **unit tests** should be kept as maximum. The number of **integration (service) tests** should be less than unit. As for **UI tests** - their number should always be as few as possible.  

For **monolithic** web based applications this pyramid suits well.  

But what about **microservices**?  

### Microservices - here and now... 

Over the last few years microservices became really a mainstream type of software architecture. It is used for building a complex, loosely coupled and reliable backend systems.  

![Project Structure]({{ site.baseurl }}/img/20180910/micros2.png)

In a few words: instead of a huge monolithic backend system, which holds all the business logic and databases, now we have a bunch of independently deployable services, which shares a part of business logic each.  

More about microservices itself and a proper way to build it - you can find in this blog post by Martin Fowler - [Microservices][microservices]. Or in the beautiful book written by Sam Newman - [Building Microservices][building]

### Adapting pyramid to new conditions  

As we are going to test and, what is also important - to automate, such systems - we need to revise our current approach to structuring test levels.  

Each microservice now can be represent as an independent system, which requires testing at various levels: unit, integration, component.  

Integration between individual microservices also should be covered by integration tests.  

Also our system as a whole is expected to be verified. Thus, we need to implement end - to - end tests for API or UI levels.  

As a result, our scheme for testing levels now is slightly out-of-date in the realm of microservices.   

![Project Structure]({{ site.baseurl }}/img/20180910/new_pyramid.png)

And yes - this is not a pyramid or even a triangle.  

According to this scheme, **each microservice is tested separately** (involving real or mock dependencies).  

**Contract testing** is used for checking integration between the services.  

**UI and API level tests** are also a part of our testing scheme - but their number is kept to minimum and should cover only the critical business flows of the application under test.  

Also, we need to remember that our testing approach will not be solid without any manual testing for our applications. It can be an **exploratory, UX testing**, etc. These types of testing of course should not be neglected by any successfull development team.   

### Conclusions 

In the next posts I am going to share practical tips on how to write automated tests for microservices at different levels.
We will concentrate mostly on Spring Boot framework and Java programming language, but also make a step back - to try other languages and approaches.  


[microservices]: https://martinfowler.com/articles/microservices.html
[building]: https://samnewman.io/books/building_microservices/
[agile]: https://www.amazon.com/Succeeding-Agile-Software-Development-Using/dp/0321579364
