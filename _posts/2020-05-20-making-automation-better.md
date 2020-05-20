---
layout: post
title:  "Making automation better"
date:   2020-05-20 00:00:49 +0300
author: "Oleksandr Romanov"
description: "In this blog post I share a few tips on how to make automation better"
header-img: "img/20200520/better.jpg"

---

## The question

The first automated test is eventually written. The script opens a product's page, makes a few clicks, and do some assertions. It looks easy to write. It looks like “magic”!  

But the main question is how to make it further: how to move from one automated test to a scalable automation solution, which provides value and is **reliable, usable, and fast**?  

Here are a few things which I might say to myself in a past which can make automation efforts better.  

## Determine goals and expectations

![Project Structure]({{ site.baseurl }}/img/20200520/goals.jpg)

Gather all visions and requirements about automation from all stakeholders: management, developers, fellow testers, and other people involved in your product.  

- Why do they want to introduce automation? It is only a "hype" or a well-established plan of transformation?  
- What do they want to optimize? What do they want to save (time, money)?
- How do they see the usage of automated tests on a daily basis?  

**Tip:** Test automation is not a “write once, execute forever” solution. Maintenance should be taken into account as well.  

## Research technology stack and architecture of the product

![Project Structure]({{ site.baseurl }}/img/20200520/research.jpg)

Learn about product architecture - it will help to plan which automated tests bring more value.  

If you are the first person to start with automation - choose the stack which is close to your developers: they can help you in the future.  

Try few tools from the market: from codeless solutions to automation libraries and compare the pros and cons. Keep in mind the goals during comparison: the tool may be helpful at the start - but it will be hard to maintain a number of tests will grow.  

**Tip:** stable and fast integration or API tests can bring more value than thousands of flaky UI tests.  

## Get necessary skills

![Project Structure]({{ site.baseurl }}/img/20200520/skills.jpg)

Learn programming language fundamentals and go through a few automation-related courses.  
Do not hesitate to ask for help and advice in testing communities - we all start from the beginning.  
Ask fellow developers for mentorship and code reviews. They can also teach your local coding standards and approaches.  

**Tip:** Consider comments in a code review as points for improvement.  

## Move small

![Project Structure]({{ site.baseurl }}/img/20200520/movesmall.jpg)

Start with a small smoke suite of the most critical cases. Make them stable and usable.  
Automate 100% of tests that can be automated, no more.  
The best way to think about testing as a mix of automated and manual exploratory testing.  

**Tip:** Five cases, which are executed on each developer's code change is a way better than a hundred of failed tests that are executed from time to time.  

## Always show value and visibility

![Project Structure]({{ site.baseurl }}/img/20200520/visibility.jpg)

Execute automated tests from day one. Integrate it into the development process.
Continuously show the value from automated tests: how much time do they save, how stable or fast are they, what is the automated test coverage?  

**Tip:** Think outside of tests - where automation can help and speed up the delivery. It can be the scripts for preparing test environments, generating test data, setting up complex preconditions.  

## Engage the community of users

![Project Structure]({{ site.baseurl }}/img/20200520/community.jpg)

If you already show value and visibility of automated tests - try to find users and supporters within your team and other engineers. Their feedbacks are the point for improvements.  

**Tip:** Automation does not live in a vacuum. Do not use it alone - make other people use it and get benefits from it.  

## As a conclusion

**Be ready to maintain and fix** the tests in a continuous manner.  
**Be ready to resolve complex errors** especially in a distributed environment (e.g.during parallel test runs).  
**Be ready to optimize test code** in terms of complexity, readability, and speed.  
**Be ready for software engineering in automation.**  

And of course - be ready to **help** the team to test the product **faster and reliable.**  
