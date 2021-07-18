---
layout: post
title:  "Should You Use Contract Testing?"
date:   2021-07-12 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210712/contract.jpg"
description: "Easy explanation on what is contract testing and why it may be usable"
summary: "Easy explanation on what is contract testing and why it may be usable"
header-img: "img/20210712/contract.jpg"

---

Photo by <a href="https://unsplash.com/@cytonn_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Cytonn Photography</a> on <a href="https://unsplash.com/s/photos/contract?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Almost any tester involved in testing microservices-based systems has heard about the magic contract testing.  
 
But what exactly are contract tests? Should you use it in your project?  

In this blog post, I will talk about the case when contract testing can help. I will also compare contract testing with other types of tests and share my thoughts on using contracts in your application. 

## The story  

Let me introduce two characters of our story: Dave (The Developer) and Tina (The Tester). Both of them work in one Scrum team delivering a critical piece of software for a conference application. The whole backend contains hundreds of microservices.  

Their task is to provide the functionality to get bonuses for a given user. Dave needs to implement a new API call from the User Conference service to the Bonus Conference service to retrieve the bonus data for users.  

![Project Structure]({{ site.baseurl }}/img/20210712/context.png)
 
## The developer way  

As a good developer, Dave wants to cover new functionality with tests. The best possible way to do it - is unit testing.  

So Dave implements many unit tests for checking feature functionality on the User Conference Service. All external calls to Bonus Conference service or any other resource are mocked (so Dave is responsible for defining the response values).  

![Project Structure]({{ site.baseurl }}/img/20210712/unit-tests.png)

Dave creates more and more unit tests; the code coverage tool says that it is covered more than 80% of the new code.  

Dave finishes his work with the confidence that the tests catch-all error cases and bugs. All tests are executed locally within seconds.  

## The tester way  

After Dave's change is deployed to the staging environment, Tina starts the testing. And it turns out that the new functionality does not work at all.  

The root cause of the problem is that another developer changed the Bonus Conference service API, and Dave was not aware of it at all. All unit tests check the service with the older version of the API.  

Tina, as a tester, catches the broken change only after end-to-end testing of the functionality.
After the root cause analysis, Dave and Tina decide to cover the broken case by end-to-end automated test. It will detect the broken API issue when the User Conference service will be deployed to the test environment.

![Project Structure]({{ site.baseurl }}/img/20210712/end-to-end-tests.png)

So from the one hand, Dave has many unit tests that provide good coverage, fast feedback, and runs on a local machine.  

On the other hand, the end-to-end test will provide an additional check of broken integration, but the feedback time will be slow: all services must be deployed to the test environment first.  

Given the number of services (hundreds) and updates per day (thousands or more) - each developer should wait for an entire deployment to find the end-to-end test results.  

**Maybe there is another way of getting information about broken integration? It turns out that it exists.**

## Contract testing to the rescue

Contract testing is another approach to test integration between two counterparties.  

These two sides can be services (communicates via HTTP REST, messaging, or gRPC calls), client and server, various third parties.  

In contract testing terminology, the side that makes the request is called the ***Consumer***; the side that responds is called the ***Producer***.

![Project Structure]({{ site.baseurl }}/img/20210712/contract-tests.png)

The main idea of contract testing is to provide fast information about broken integration without deploying services to the environment. Imagine it like an end-to-end test that delivers the results with a speed of unit test.  

Instead of creating separate unit tests on the User Conference and Bonus conference services, Dave will start with the contract definition.  

The contract defines what the User Conference service will request and what it expects to get in response.  

Here is an example of the contract, using **[Spring Cloud Contract](https://spring.io/projects/spring-cloud-contract)** library:

![Project Structure]({{ site.baseurl }}/img/20210712/contract-sample.png)

Then both services should implement contract tests for each contract. After contract tests are implemented, the most exciting part begins.  

If the developer wants to change to Bonus Conference service API (the Producer), he needs to change a contract. Otherwise, the Bonus Conference service will not be built and compiled locally due to failed contract tests.  

After the developer changes the contract, all consumers will be notified about the change. So they need either approve the change (handle it in their services) or comment that this change breaks the integration.  

## Test types analysis  

Let's compare three types of tests: unit, contract, and end-to-end API.

![Project Structure]({{ site.baseurl }}/img/20210712/comparison.png)

* **Execution.** Unit and contract tests are executed locally on the developer's or build machine, but end-to-end tests require services to be deployed to some dedicated environment.  

* **Feedback time.** Feedback time of failed tests is fast for unit and contract tests. Of course, contract tests require additional seconds to generate tests from contracts, but the overall execution time is almost like for unit tests. As for end-to-end tests, we need to **wait**: wait for service to build and compile; wait for services unit and contract tests execution; wait for the deployment of the services to environment; wait for test execution.  

* **Setup complexity.** For writing unit tests, you need to include a unit-testing library in the project, and you are ready to go. In case of contract tests, you need to set a contract test repository and build an automatic workflow for consumers and producers. For end-to-end tests, you need to develop a deployment pipeline and tests themselves.  

* **Change tolerance.** Unit tests are build only on one developer's assumption on how another service should work in a given moment. If something is changed, unit tests will always be "green." On the contrary, contract and end-to-end tests will fail in case of broken integration changes happen.  

* **Functionality verification.** Both unit and end-to-end tests verify functionality. The main goal of contract tests is to check only the format of communication between two parts.  

* **Team collaboration.** To implement unit tests, the developer does not need to communicate with other developers. Contract and end-to-end tests push the developer to communicate and to think about the impact of changes.  

## Should I use contract testing?

**Contract tests are not a substitution to the unit or end-to-end tests.** It is just one type of test that can be performed.  

![Project Structure]({{ site.baseurl }}/img/20210712/test-layers.png)

If contract testing workflow is implemented correctly - ***it is possible to minimize end-to-end tests and speed up the development process.***

So the crucial question remains: **"Should I use contract testing?"**  

You will not get a value from the contract testing if:  

* the coverage of unit tests is low

* the number of end-to-end tests is close to zero

* there is a lack of skills for setting up the project and writing the contracts

* there is a lack of dedication to change a process of delivery

* the communication between teams is broken (e.g., blaming each other for integration issues)

## Conclusions  

Contract tests are not a "silver bullet" which provides only the benefits.  

It requires many efforts for configuration; it requires even more efforts to change the developer "culture." With contract tests, you can't make hidden changes, silently push it and close the Jira as soon as possible.  

And most important: contract testing forces you to think more about the consequences of each change that goes to production.  

**Have you already tried contract testing for your projects? Did you get any benefits?**  
