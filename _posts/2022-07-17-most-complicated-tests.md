---
layout: post
title:  "The most complicated automated tests"
date:   2022-07-17 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220717/challenge.jpg"
description: "Which tests are the most complicated ones?"
summary: "Which tests are the most complicated ones?"
header-img: "img/20220717/challenge.jpg"

---

Photo by [GR Stocks on Unsplash](https://unsplash.com/photos/Iq9SaJezkOE?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

Are UI tests that easy to write and maintain? Are unit and integration tests really the most complicated tests. 

This blog post will explain my thoughts on which tests are complex and why. If you agree or disagree with these thoughts, let's discuss them in the comments. 

## What started it all?

![Project Structure]({{ site.baseurl }}/img/20210712/test-layers.png)

At the start of my career in test automation, I focused entirely on **UI tests**. Then, after some time, I got to know the zen of the **API level** and started writing such tests wherever possible (wherever that API existed). It was a time of SOAP and REST HTTP testing.

But during the first years, no matter how many automation tests I wrote, I always thought my tests were too easy. 
Real "hardcore" developers write complex and scary unit and integration tests. To write those unit tests, you must know the programming language and frameworks very profoundly. 

These thoughts were in my head until I decided to look at frameworks for unit testing and unit tests in one of the projects at work.

## Unit tests

On the one hand, it seems that unit tests are difficult to write. But it becomes much easier after you understand the very essence of such tests and master at least the basic capabilities of any test framework (such as **JUnit**), mainly when you write features and tests yourself.

![Project Structure]({{ site.baseurl }}/img/20220717/unit.png)

We concentrate on class level (or several closely related classes) in unit testing. The main idea is to test business logic here. Then you can mock all external calls using a mocking framework such as **Mockito**. Simple test data is created in the form of stubs or fakes. 

When you figure out how to write mocks and tests correctly, the speed of creating new tests depends only on the complexity of the business logic of the class itself. As for supporting such tests, it is also not tricky: such tests are closely related to the feature code, so they need to be changed often.

**Implementation complexity of unit tests is easy as well as the maintenance cost.** The only question is how testable the feature code is. 

**The critical thing about unit tests is that the developer fully controls the environment, test data, and behavior of dependencies.**

## Integration tests

Integration testing is one of the most controversial themes among engineers. Every person can understand the integration level on their own. 

![Project Structure]({{ site.baseurl }}/img/20220717/integration.png)

By integration tests, I will understand the testing of individual components in isolation from the entire system. For example, when we have many microservices on the backend, the integration test can be both at the service level (we check the business logic of the service as a whole, and at a more granular level - we verify the operation of the service with the database).

In this case, we mock requests to other services or third-party components using Wiremock. Other dependencies can be run locally as **Docker** containers (**testcontainers**) or in the form of in-memory databases.

Such tests show a more realistic picture of the operation of part of the system and its components.

**The implementation complexity of such tests is average.** The main problem here is the correct configuration of dependencies for the service. 

**The maintenance is also not very hard.** If you have a proper design for your test and feature code, the interfaces between components will be more stable than in unit tests. So the change of database or messaging broker should not have a high impact on the core logic of the system or service. 

**As for the control, the developer has limited control over the dependencies. It is possible to configure the database or message queue.** You can also set some test data as pre- or post-conditions. But you can't be 100% sure that the DB implementation is correct. 

## End-to-End tests

By end-to-end tests, I mean both UI and API tests for the system. The main thing in such tests is that we work with the system as a user. You can call such tests black-box if you want. 

Once you master the Selenium Webdriver's API and know how to use Page Object (and other) patterns, implementing end-to-end tests becomes very easy. 
Of course, there can be exceptions: very complex and tricky logic, exotic browsers, or non-standard APIs. 
Creating new UI tests is trivial for a developer (compared to unit and integration tests). That is why everyone believes that end-to-end tests are the easiest. 

![Project Structure]({{ site.baseurl }}/img/20220717/end-to-end.png)

But the thing is that automated testing is not only about implementing new tests. It is also the maintenance of such tests. It is debugging and fixing. Here lies the biggest pitfall (almost an iceberg) in the perception of end-to-end testing.

A LOT of external and internal factors affect the performance and stability of such tests:
 - **databases** can perform slow replication and return inconsistent data
 - hundreds or thousands of backend **microservices** can stop working at any time (causing cascading problems)
 - critical services may receive data from the **message broker** (e.g., Kafka) with a delay
 - **third-party services** can also work unstable or be completely unavailable
 - the system may be under the influence of an external **DDoS attack**
 - **load balancers** and **caches** can also be unstable
 - the **web or mobile applications** can work differently (depending on the phone model or the browser version)
 - **requests from the browser in the tests** may be processed more slowly  than from the actual user
 - **the test framework** can be erratic (especially if you run many tests in parallel)
 - **connections to containers** in Selenoid can also disappear for some reason
 - **connection to device farm** can also be a point of instability

Considering all these hidden moving parts, the business, management, and even fellow testing team still expect **almost 100% stability of UI tests.** 

But the reality is harsh. When any UI test fails - it needs to be **isolated, analyzed, and carefully fixed**. If you have a dedicated automation team, those few brave engineers need not only understand at which point of the test the issue occurred. They need to isolate the problem and report it to the particular development team for a fix.

Sometimes, the fix will be on the UI test or framework level. Sometimes it is a change in requirements or the urgent need to update the test data. 

But in many cases, the fix also can't be done immediately, especially if the root cause is unclear and need time for investigation. The development team has its priorities and tasks. Ignorance of such failing tests and issues increases the risk of having a bug in production. But anyway - it is a question of how effectively to use end-to-end testing on the project. 

**As a test automation engineer, you don't have control of the system and its dependencies. Here is your browser - and that's it!**

## So, which automation tests are the most complicated?

![Project Structure]({{ site.baseurl }}/img/20220717/compare-levels.png)

I don't say that some tests are redundant and do not bring value. I just want to state that after an initial learning effort, the implementation of unit and integration tests becomes a relatively easy task. That's why we need to have more tests at these levels. 

UI tests can become a nightmare if they are flaky and slow. But it can slow the whole development and release process if the automation team is separate and there is no dedication to end-to-end tests from each development group. 

**End-to-end tests can bring value or be the most complicated automated tests to maintain. It can be a huge source of frustration for automation engineers.**  

Do you expect stable UI tests for your automation team? 