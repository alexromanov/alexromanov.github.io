---
layout: post
title:  "Modern Problems With Test Automation"
date:   2021-07-05 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210705/problem.jpg"
description: "My thoughts on modern test automation problems and how to solve them"
summary: "My thoughts on modern test automation problems and how to solve them"
header-img: "img/20210705/problem.jpg"

---

Photo by <a href="https://unsplash.com/@kxvn_lx?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">kevin laminto</a> on <a href="https://unsplash.com/s/photos/cyberpunk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## Brave new world  

Test automation is everywhere. Almost every job description in the software testing industry requires knowledge of programming and automation at some point.  
If you don't have these skills - please go to numerous courses and learn the skills. Now, as soon as possible.  
Otherwise, you will not get a good job. Or you will not be promoted. Or you will not be an SDET (Software Developer In Test).  

But do we get the value from test automation? Do we correctly implement automated tests for our projects? Are there any problems with that?  

In this blog post, I want to highlight a few problems with test automation. Some of the issues I faced personally, other - I know by talking to other engineers.  
 
## Don't want to see the trees for the forest  

**Concentrating only on the UI/API end-to-end level.**

>"If they want test automation, we will create a bunch of UI end-to-end tests and get immediate results! Everybody will be happy! Bugs will not slip into the release!"  

>"Let's use this new fancy record-and-playback tool, and we will be able to create automated tests even without programming knowledge."  

>"Let's use BDD (Behaviour Driven Development) approach, and the team will create end-to-end UI tests which business people and developers will use! I have not asked them yet, but they will use these tests for sure!"  

>"Unit, component, integration tests... Oh, boy! It is too technical, and I am not a developer! I want to fix UI tests day by day."  

>"I heard at the conference that this tool is popular, so I purchased a license for a year! Now we as a team need to find out how we can use it."  

>"Our UI automated tests are green - why do we still have bugs?"
 
## Thinking fast and small  

**Implementing quick and destructive solutions, and then moving to the next project.**  

>"Now I am going to implement a test framework with this fancy new library X and move to another project ASAP!"  

>"If I use this new library that just been released a month ago - I will be able to put it into my CV and request more salary!"  

>"Who cares that for test execution, you need first to install a hundred dependencies, update 39 different files, and re-compile half of the product?"  

>"Why should I write README for the test project? Anyone can understand the solution and quickly start implement the tests!"  

## Scratching a technology surface  

**Lack of focus on deep technology and architecture knowledge.** 

>"I don't know how the system work - for me, it is only a set of web pages or mobile app which is somehow works (by magic)."  

>"Subsystems, abstractions, load-balancers, HTTP calls, messaging, replication? Wow - it is too much for me. I will write the "Open Google page" test one more time using a bright and shiny new framework."  

>"Wow, I just clicked something, got some scary error, make a screenshot and throw it to the developer."  

>"Why has a bug happened? How can we prevent it? - Oh, I don't know, the developer did a fix, and now it works. Somehow. I don't care."  

## So what?  

For junior test automation engineers, it is entirely normal to ask all of such questions. It is normal to concentrate on the UI tests only, do not understand bug stack traces, and do not dig into the technology.  

But if you are a senior engineer now and still see many such questions asked, here is what we can do to make the situation a little better.  

Here what you can do:
1. **Learn the programming language and technology stack of the system you are working with.** Which framework do developers use, which database? How is the system deployed? How is your data replicated across the system? What are third parties to an application under test? The more you know how the system works, the faster you can investigate and fix the failures. And the better tests you can create.  

2. **Investigate more about different levels of testing: unit, component, integration, contract.** What are their pros and cons? Why can't you use such tests in your project? What blocks you from adding such tests?  

3. **Discover the root causes of the issues together with developers and DevOps engineers.** Is it possible to find the bug before the UI tests caught it? Which actions can you do now to prevent such issues in the future? Where do you have "holes" in code coverage? 

4. **Do not consider your job as a "fast fire-and-forget consultancy. (Unless you are a consultant)"** Always ask stakeholders why they need to have test automation. Which value do they want to get from such tests? How can you help them to release faster and more reliable? What is the product vision for upcoming years, and how automation fits in?  

5. **Explore how other companies solve issues with test flakiness, stability, scalability.** In most cases, your application is not a unique - so you can find out that your current struggles have already been solved by some open-source tool that you can use.  

6. **Do not try to use every fancy tool on the market.** Start with careful evaluation and compare different tools in the segment. Do not believe that automated tests can be recorded once and be executed forever. Maintenance always matters!  

7. **Always think about which impact do your engineering efforts make on the product.** Is it make the life of other developers easier? Does it help to investigate issues faster?   

## Conclusions

Every team member can be a source of automation problems. Maybe it is a test manager who wants to change a process and tools. Perhaps it is a developer who doesn't like to share information on how the product works internally. Or it can be a test automation engineer who wants quick results without any effort.  

Anyway, it is up to you to identify and fix such problems. Good luck in fixing it!  

**Have you ever faced any of these problems?**
