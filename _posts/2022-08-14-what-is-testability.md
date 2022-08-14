---
layout: post
title:  "Testability and how to improve it"
date:   2022-08-14 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220814/testability.jpg"
description: "How can a regular test engineer affect testability"
summary: "How can a regular test engineer affect testability"
header-img: "img/20220814/testability.jpg"

---

Photo by [Jeswin Thomas on Unsplash](https://unsplash.com/photos/-Cm7hnp4WOg?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

## Why is it testability?
At first glance, the typical test engineer already has enough things to do:
- Work out the requirements.
- Create plans and strategies.
- Write test cases.
- Report and re-check bugs.
- TEST!

Somewhere between these things, you still have to find time for automation. And you still need to make a demo for the customer.  

Why even bother with that **testability**?  

Isn't that the task of whoever writes this code? Is it possible for a test engineer to influence the team (especially the architects and developers) - and make the software a little more open and easy for testing?  

Let's try to figure this out a little together.

## Testability: officially and not so much
What is the definition of testability given by industry standards?

According to [ISO/IEC 25010:2011](https://www.iso.org/obp/ui/#iso:std:iso-iec:25010:ed-1:v1:en):
*Testability - degree of effectiveness and efficiency with which test criteria can be established for a system, product, or component, and tests can be performed to determine whether those criteria have been met.*

According to [ISTQB terminology](https://glossary.istqb.org/en/term/testability-3):
*Testability - The capability of the software product to enable modified software to be tested.*

In simple terms, **testability is how convenient and easy it is to test an individual component and the entire system.**

**Testability** depends on multiple factors:  

* How easy is it to control the internal state of the components during testing?
* Is it hard to observe the system and its components during tests?
* Is it possible to test components in isolation?
* How fully documented is the component?
* Is it doable to write automated tests for it?

## Testability at the code level
To efficiently test classes at the module level, you need to be able to create Mock objects for all class dependencies.

If the developer applies the [**Dependency Inversion**](https://en.wikipedia.org/wiki/Dependency_inversion_principle) principle, there will be significantly fewer testing problems. Ideally, you need to pass all dependencies through the class constructor.

Also, don't forget about [**observability**](https://en.wikipedia.org/wiki/Observability) and [**controllability**](https://en.wikipedia.org/wiki/Controllability), even at the class level. The developer (or test engineer) must be able to see the state of critical parts of the object at any time. It would be much better to add different methods to get this information directly to the test class.  

But still - this aspect is entirely under the responsibility of the developer.  

## Testability at the architecture level
A practical tip at the architecture level would be to separate **infrastructure** and **domain** code. By domain code, we understand all the code responsible for business logic. Infrastructure code works with external dependencies - databases, message queues, third-party services, etc.  

To separate one type of code from another, you can design components using the **[Ports and Adapters (Hexagonal)](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))** pattern. **Alistair Cockburn** proposed this model in 2005.

![Project Structure]({{ site.baseurl }}/img/20220814/portadapters.png)

Maurício Aniche gives a more practical example of such a model in the book **[Effective Software Testing](https://www.manning.com/books/effective-software-testing)**:

![Project Structure]({{ site.baseurl }}/img/20220814/portsexplained.png)

## Testability through the eyes of testers
Okay, but how can a particular test engineer affect testability?

### Unique properties of elements
If you've written more than one UI test, you can tell a lot about the pain of "long and fragile" locators. Changing the elements on the page is one of the main reasons for the instability of such tests.

As a test engineer, you can do two things in this case to improve testability:

- Agree with the team (or architects) to add unique properties for each new critical element;
- Agree with the developers that you will add unique properties to elements whenever it's needed (for legacy components);

Having an special property like **"automation-id"** or **"data-id"** for a web element - **saves a time and nerves** for a test engineer. 

### Components in isolation
Please keep track of how convenient it is to test the component in isolation. Ideally, a developer or tester should isolate a single part (e.g., a single microservice) and test only the business logic.

* Is it easy to replace the actual database with an in-memory one?
* Is it convenient to fill the database with the necessary test data for the correct component operation?
* Is it easy to block external requests to third-party services?
* Is it hard to send or read messages in the Message Queue for a single service?

You can always work together with a developer to build such tools for the components of your system. In many cases these tools can be find as libraries. If not - it is a good point to create it and spread as inner source in the organization.  

### API for system management
It is good to analyze how you test (from time to time). Some components in the system may work using probabilities or specific third-party data. The tests for such components will be unstable. But such systems should be tested and, ideally - automated as well. 

In search of the answers, we can think of it as a system of saves in a computer game. 
Imagine that you need to test how the "boss" behavior at the end of the level. You can verify it by playing the whole level from the start till the end. If we talk about one or two minutes - it is ok. But if each level in the game takes even ten minutes or more - such tests quickly become unrealistically long.  

A better way will be to use magic save, reach the end of the level and test exact behavior.  

The solution is to **make a separate test API that will allow you to bring the system to one or another state.** Using such API will make tests faster and increase the stability and predictability of the system under test (at a certain level).  

That is the reason why many games have a long list of cheat codes. 

You can think about such a "cheat" system in the context of your application. These can be special commands you need to enter or separate ways of creating data that the system will process as a test (even in production). Such data will then be easier to filter and analyze.

## And how do YOU improve testability on your project?