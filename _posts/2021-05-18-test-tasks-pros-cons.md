---
layout: post
title:  "Test tasks for automation engineers: do we need them? "
date:   2021-05-18 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210518/test-task.jpg"
description: "Pros and cons of test tasks for automation engineers"
summary: "Pros and cons of test tasks for automation engineers"
header-img: "img/20210518/test-task.jpg"

---

## Interviews  
  
Job interviews are hard. It is hard and stressful when you apply to the company as a candidate.  
But it is also hard if you are on the other side - as an interviewer. The hard thing is to assess the hard and soft skills of the candidate within a very limited period of time.  
  
Some people say that the main thing is deep technical knowledge. Without it - the candidate is completely useless. The others say that the better person communicates - the better teammate he/she will be at work. The hard skills can be easily taught during the first month or so).  
  
But the biggest problem still remains the same.  

You only have one or two hours. You are not able to disclose any code under NDA.  You are also not able to "waste" time of interviewers on the wrong or "weak" candidate. 
How can you ensure that the candidate can actually write a code at the appropriate level and can bring "value to the table"? 

In some cases, the answer is - **let's give a candidate a test task to implement!**. Then you can perform a code review, see the actual code and prepare additional questions for the technical interview. 

Seems like a good plan. But is it really good? Is it possible to make an "ideal" task for a test automation engineer? Let's find out together in this blog post. 

Throughout my career, I was involved in interviewing QA and test automation engineers for my team as well as for other teams within multiple companies. Also, as a part of my consultancy job, I was involved in technical assessment for some automation engineers in other organizations.  
  
Additionally, one of my tasks was to define job descriptions for multiple test-oriented positions.

Please note that I am talking about hiring a Senior / Lead / Principal engineers in test automation.

## Define a test task
Let's imagine - what is an "ideal" form of the task for a test automation engineer (or SDET or QA Engineer with automation skills).  

Here is the caveat - the answer as always is - **"it depends"**.  
It depends on the testing strategy at the company level.  
It depends on the technology stack.   
It depends on the tools and processes within a particular Scrum team.  
It depends on the product and company vision. 

### Task content
What the content of the test task should look like? 

* Should it use the public API of your product? (Imagine that the official API docs is not so up-to-date so the candidate needs to find out a lot of things during the implementation). 
* Should it be an abstract test for an abstract site? 
* Should it be a test for some test application? Do you have an engineering capacity to implement a test application and maintain it? 
* Should it be only UI test automation? Or only API? What about mobile? 
* What if you need to have a person to maintain an old legacy system written at Java 6? 
* What if then in 6 months the project is planned to be migrated to another modern stack? Should a test task check Java Core knowledge? Or Selenium Webdriver API? 
* Or it should check the knowledge of modern Javascript testing tools and frameworks? 

### Task size
* How much time should a candidate spend on the test task? Is it one hour, two? Is it a couple of days?
* Do you want to compensate all candidates for time spent on the task? 

### Skills to assess
* Which skills do you want to evaluate in the test task? 
* Is knowledge of core programming language? Some specific tools? 
* Is it APIs of third-party systems? Is it continuous integration tools and Bash scripting?
* Can your current teammates pass the test task or interview for this position? :)

Whenever you want, please remember: **it is almost impossible to find the exact person that fulfills 100% of the requirements from the job description**. 

## Pros and cons of test tasks
Pros:
 - You can see the real code that engineers can write: which patterns are used, what is the structure of tests and solutions, is it understandable, maintainable, extensible, it is applicable to your internal code standards?
 - You can come up with additional questions which can be asked during the technical interview

Cons:
 - Test task may be implemented by the other person (even for some compensation)
 - Test task may be implemented with the help and guidance of another person
 - Other companies offer an interview process without any test tasks - so the candidate will consider them in the first place
 - Candidates are not compensated for the time spent on the test task. For Junior positions, it can be considered a learning opportunity, but not for Seniors. Specifically, if you do not provide any feedback on the test task

## When test tasks ~~should~~ may work

Test tasks may work in the following cases:
 1. In case if your company has a **solid** technical brand (so everyone wants to work on that well-known product. So does the candidate)
 2. In case if your company or team **has an engineer/scientist well-known in the community** and the candidate wants to work with that specific person and learn from him/her
 3. In case if your company is **well-known by its high salaries** (way above the market) and/or incredible other **perks**
 4. In case if you are developing **great product and the candidate loves** it and wants to "improve the world"
 5. In case of actively **contributes to Open Source** and the candidate wants to do the same

## When test tasks will not work

Test tasks will work in the following cases:
 - In case if there is high demand for the candidates so you need to compete for the talent
 - In case if your company is one of the various companies with the same profile (e.g. outsourcing company with a big legacy project portfolio)
 - In case if your company is small and can't compete on the salaries
 - In case if your salaries are within the market and perks are the same as at your competitors
 - In case if your product is not so legal :)

## So should the test tasks exist or not?
The simple anwser is: it is up to you to decide. 

In case if you can and want to provide the test task - please make it open-ended. Provide a few points that should be achieved for sure, but for the other improvements - it is up to the candidate to decide what will be an "ideal" implementation.  

IMHO, if you have a brand and good salaries you can try to offer a test task as a pilot project. But track the efficiency of the process. If you lose good candidates due to the test task - maybe something should be changed in the hiring process.

In case if you stick to "no test tasks, only technical interviews" - ask the candidate to review existing test code, find bugs or issues in it, pattern violations. 

Good thing is to ask the candidate about the Github profile and contributions to OSS. Here you can review an actual code and see whether it was approved by the community. 

Generally - it is better to seek a person with solid engineering knowledge even without matching your automation stack. In test automation, particularly in UI, it is not so hard for an engineer to learn a new language (of course if it is not Javascript:) ) and libraries if he/she knows the basics well. 

You can always ask a candidate to implement some piece of test during the technical interview. 

What about you? Do you use test tasks before technical interview? 
  