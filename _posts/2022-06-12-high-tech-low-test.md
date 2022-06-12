---
layout: post
title:  "High Tech - Low Test or Problems with Modern Testing"
date:   2022-06-12 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220612/hightech.jpg"
description: "My thoughts on problems with modern testing"
summary: "My thoughts on problems with modern testing"
header-img: "img/20220612/hightech.jpg"

---

Photo by [Leyre on Unsplash](https://unsplash.com/photos/71SHXwBLp5w?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

I have been working in test automation and development for more than ten years. I've seen good projects, and I've seen awful ones.  

In this blog post, I want to highlight the problems I see in the testing industry and how we all can fix it. (You may have a different experience - so let's share it in the comments!). This is a text version of **[my talk that I gave at Abu Dhabi MoT meetup at the beginning of 2022](https://youtu.be/jigPyy6wSfk)**. 

## What has changed in the industry in 10 years

Over the past ten years, we have witnessed a significant technological boost - driverless cars, artificial intelligence, AR / VR, blockchain, drones, and robots. Much testing and test automation moved from the desktop to the web and mobile devices.

From Waterfall models, many businesses went to Agile and Scrum.

From big and costly tools for writing automation tests, we have moved to small, flexible, and, most importantly - free libraries and tools. Now we have intelligent reporting systems and tests in Docker containers. Running thousands of tests in parallel in the clouds now is not so expensive. 

But at the same time, many things and statements have remained unchanged.

Here are some of the endless topics that are constantly raging among testers:
- *"Manual testing is dead!"*
- "Let's automate everything! Through UI - that's the best!"*
- *"SDET is the peak of the evolution of a test engineer! I want to be like Google!”*
- *"I can't get a job in development, so I will work for a year or so as a tester and then try again."*
- *"Salaries of testers - at the bottom!"*
- *"Test engineering is a job for low-skilled professionals!"*

But why is there such a situation in the industry? Could the problems be something else? What issues do I see?

## Problems with modern testing

![Project Structure]({{ site.baseurl }}/img/20220612/problems.png)

Photo by [Karla Hernandez on Unsplash](https://unsplash.com/photos/LrlyZzX6Sws?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

I will divide the problems into two large groups.

### The pursuit of technology
- **Concentration only on short-term goals.** Many engineers prefer only quick solutions. Such an attitude to automation tests is prevalent. The engineers quickly wrote "something" without any thoughts of maintainability and ran out to a new project.

- **Resume-driven development.** Many testers only consider jobs and projects as a way to get a fancy new line in CV. There is nothing about deepening the skills and solving business issues with testing and automation. 

- **The green pipelines cult.** In this case, test engineers who have successfully included tests into their CICD pipelines begin to be obsessed with making tests green all the time. Such obsession often results in ignoring or even removing unstable tests (which leads to ignoring the problems behind ignored tests). 

- **Framework-oriented automation.** In some projects, engineers are so eager to write the perfect framework that they forget about the tests themselves. As a result, we have 3-6 months of development without tests!. But for the customer, the absence of tests is a direct indicator of the lack of benefits from automation.

- **Search for silver bullets.** Some engineers create a successful framework for one project (and for one context) and then start pulling it into all the others as a "perfect solution." It is not reusability; it only applies one hammer to all the nails and bolts. 


### Insufficient level of skills and knowledge

The lack of knowledge can take multiple forms.

#### Technical knowledge
In interviews, senior candidates often quickly answer any questions about testing. They can draw a "testing pyramid or any figure you want" for you, tell you all the test design techniques in the world, and write the perfect bug report. But candidates quickly lose confidence when you ask questions about fundamental technical aspects (such as how HTTP or networks work).  

You can tell me that not every project requires specific knowledge. That's true. But test engineers should know (or at least be aware of) the fundamental technical knowledge. Ideally, a little bit deeper than the first link in the Google search.  

90% of modern systems work with the network in one way or another, send messages or requests, and use the database or distributed storage. Multiple layers of abstractions can cover it - but in a nutshell - it all works similarly.  

#### Programming and architecture
Some test engineers still believe that learning programming is complicated and unnecessary. Let the programmers write the code!

The other part is those who have learned to code a bit but concentrate their efforts only on the UI tests. The world outside of these tests does not seem to exist.

Knowledge of system architecture and how they work is a scarce skill among test engineers. All this is considered work for experienced "bearded" architects or only senior developers with dozen years of experience.

But without knowing how the system works inside and with each other, it is easy to miss a lot of critical errors. On the other hand, the lack of technical knowledge will make it hard to describe such issues in a bug report. 

I'm not saying that without knowledge of the components of the system, it is impossible to point out lousy testability. As a result, we continue to write fragile XPath locators because no one has added IDs for elements.  

#### Knowledge of business and product
Of course, it all depends on the project. There are some projects where a testing team works as if on a factory line: get the build, test it and pass it to the next unit (without asking any questions).

Test engineers are significantly lacking in product knowledge. Few engineers work with users or with customer support. 
But those people can provide helpful information about the riskiest parts of the application or give feedback on the usability system.

Even fewer test and automation engineers think about how their work affects business. Do those automation tests save the company money? Or provide feedback faster than the manual testing?  

#### Personal development of engineers
Regarding personal development, there are two significant groups of engineers exist:

- “Why should I learn to program (any other skill)? That will make me a less skilled tester!”
- "I allegedly want to study, but I'm waiting for my one-year feedback. My manager or test lead will come and tell me what I need to learn."

But here lies the most critical point. **ONLY YOU ARE RESPONSIBLE FOR THE PERSONAL DEVELOPMENT OF SKILLS, KNOWLEDGE, AND CAREER!**

Your manager (or anyone else on the Internet) will not be able to create a map of your skills for you. Only you know your strengths and weaknesses. Only you know (or guess) what gaps you have in your knowledge. Only you can understand how to gain that knowledge and reach a new level (and get that dream boost).

The manager can only adjust your plan and indicate the company's resources: the budget for attending conferences or other developments.

## A vicious circle of uncertainty
So what questions does an average test engineer usually ask himself?

- *"I can't write code - I'm not a developer ..."*
- *"Who am I to talk to an Architect about the architecture or testability of system components?"*
- *"The DevOps team have turned off unstable tests in the pipeline - well, they know more about how it should be ..."*
- *"The manager keeps asking me: what exactly are you testing? Why is that? And that?"*
- *"Business is just trying to cut the costs and fire the whole department of unnecessary testers ...."*
- *"How do I even know what problems our users have?"*

As a result, most test engineers fall into a circle of uncertainty.

![Project Structure]({{ site.baseurl }}/img/20220612/cycle.png)

- Testers cannot correctly prove their benefits to the team, other engineers, management, and business. In such cases, it is hard for a tester to build trust and confidence inside the team;
- Demotivation of test engineers is growing. The idea to switch to software development or management grew along with demotivation. 
- Many experienced and senior engineers leave the testing industry. Cases of successful testers are becoming fewer and fewer. Cases when test engineers influence the product and processes and make technical improvements - can be counted on your fingers. Achievements and stories at conferences come down to "well, we got this framework or this tool for test management - now it's all  beautiful." There is a lack of people who can write new libraries and tools. There is no one to test complex and scientific software. There is no one to do research in the field of testing;
- The industry continues to think that a tester is someone who "just pushes buttons and does almost nothing." Young test engineers see the same picture but are even more demotivated;

## How to change the situation for the better

- **Prepare a plan for your professional development.** What skills to acquire, what to learn, and how to use those skills at work. If you can gain knowledge and skills in the company - that's excellent! If not - look for another project or company. There is always a choice - and it is in your hands.

- **Learn to program.** I'm not talking about skills at the level of a senior developer. But the ability to write a script can remove a lot of monotonous work from the deployment or configuration. Reading and understanding someone else's code gives a better understanding of how the system works in general and which cases are not yet covered.

- **Deepen your knowledge of application and systems architecture.** Start with the system you are testing now. Decompose it. Disassemble architectural diagrams. Think about how and where an individual component may "fall" and how the system will respond. Talk to your architect.

- **If you are creating scripts or automation tests, always ask yourself: who and how will use my code (tests)?** Is it written clear enough? Is there a detailed report? How easy will it be for a new engineer to understand the code?

- **Learn how users work with your system.** What is your product business model? What are the riskiest parts? This information will help you test what is vital to the user and save money for the business.