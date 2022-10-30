---
layout: post
title:  "On choosing and changing languages for test automation"
date:   2022-10-30 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20221030/choice.jpg"
description: "How to choose language for automation? Does it worth to change between langauges and tech stack?"
summary: "How to choose language for automation? Does it worth to change between langauges and tech stack?"
header-img: "img/20221030/choice.jpg"

---

Photo by [Javier Allegue Barros on Unsplash](https://unsplash.com/photos/C7B-ExXpOIE?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

**You are at the beginning of your testing career...** You see that in many job descriptions, they ask about test automation skills. Or you heard that by working in automation, you can earn more money (which depends). 
**Which programming language to choose? Which language and tech stack is best of the best? Is there a single choice for it?**

It can happen to anybody. You can be frustrated in your current job. You can feel that you are doing the simple things, and your progression is stopped. You also desire more salary or more significant responsibilities. 
So the call or message notification on Linkedin about a **new and fancy job opportunity is coming** just in time. **But the issue here is that this new job is in a completely different tech stack or programming language. What can you do with it?** 

Today I want to share my thoughts on choosing the first programming language. Also - we will think together whether it is worth changing between language and tech stack - and how to make the such process beneficial for you and your manager. 

# My story
I had various jobs and titles in my 10+ years of IT industry journey. I was a Test Automation Specialist, a QA & Testing Analyst, an SDET, a QA Automation Engineer, and even a Software Engineer.

In fact, **it is not about how fancy is your position's name.** **It is all about your responsibilities and your influence on the team** or department level. 

Over the years, I've changed automation languages ​​several times:
* at my first job, I wrote UI autotests in C# + Specflow;
* then - wrote API tests on Groovy DSL (SoapUI) and simply on "pure" Java;
* then it was the Ui web test automation with C# again;
* then it was a big project, and tests on Scala.
* Then it was Java, Spring Boot and software engineering job.
* Right now, I am writing my tests and tools mainly in Scala. 

# Questions to consider before choosing a language

![Project Structure]({{ site.baseurl }}/img/20221030/skill.png)

Image from [Reddit](https://www.reddit.com/r/ProgrammerHumor/comments/v1lx1u/this_is_a_meme/)

Before choosing the programming language for automation, you need to ask yourself a set of questions. 

## Project-related:
- Technological stack and architecture.
- Testing processes. Are there testing processes on the project? How do you plan to use automation testing? Who will be responsible for such tests - testers, developers, or a separate team of engineers?
- Test automation strategy. Is there an ultimate goal of automation? What types of automation tests do you plan to write? When will such tests be executed? How do you use the test reports?
- Programming and automation experience. Do you have programming experience? Did you write automation tests at all?

## Language-related:
- How many vacancies are currently on the market for this language
- How deeply do you plan to learn this language? What is your technical background?
- Is the language and the infrastructure around it mature enough? Do stable frameworks and tools exist that will allow you to quickly write the necessary solutions?
- How do you plan to improve your career by learning a language?

# On environment

Your work environment matters. Depending on the team, its skills, and you personally - the choices for language can be different. Let me show you a few cases and possible solutions for dealing with automation. 

## One tester. A small team. No prior automation experience.

Options: 

- Invite a short-term automation consultant. They will help develop an automation strategy and initial Proof-of-Concept for automation tests.
- Choose the same programming language for automation that the developers in your team use. They will be able to advise and help you with the tests. The downside of this approach is that developers can write complex programming languages ​​that are not very efficient for writing simple end-to-end tests. (Or course, you also can write Selenium tests in Haskell.)
- Find a mentor (inside your company or elsewhere) to help and guide you in learning automation.

## One tester. A small team. Some automation experience.

Options: 

- Choose the language you know best. Also, consider how many automation tools and libraries exist for the language. It may happen that many tools will have to be written: "from scratch."
- Alternatively, you can choose the language and stack identical to what the developers use. Learning a new programming language to create several UI autotests is relatively easy (if you already have some background).

## A team of testers. No automation experience. You're the lead.

Options: 

- Select an individual tester from the team and train him or her in test automation.
- Hire a dedicated automation engineer who will later train all testers in automation.
- Train developers to implement automation tests by themselves. (Difficulty level: HARD).

# On choosing languages

![Project Structure]({{ site.baseurl }}/img/20221030/language.png)

Image from [Reddit](https://www.reddit.com/r/ProgrammerHumor/comments/7x96ts/learning_a_new_programming_language/)

There are many programming languages. Some languages are harder to learn. But such languages can give a boost to writing tools.

- If you like the world of front-end - then your choice is **Javascript or Typescript** and its army of frameworks. This world of tools and libraries is a very dynamic one. You will always have a job!
- If you're more into network stuff, hacking, data science, or other back-end stuff (or just like simple and friendly code) - look towards **Python**.
- For those who want enterprise-level stability, consider **Java** or **C#**.
- Those who want to work with blockchain - can learn smart-contract languages like **Solidity**, **Plutus**, and other platform-specific ones. You need to learn **Haskell**, **Rust**, and **C++** for more low-level things in the blockchain world. 

## About new languages

If you choose a programming language that is new or not a mainstream, then be prepared to learn it deeply and then look for a job for a longer time. As a side effect, the salaries may be slightly higher. But it may NOT be. 

Additionally, if the language is new, you will need to write a lot of the functionality of the frameworks of other languages ​​by yourself. On the one hand, it's an opportunity to improve your technical skills. On the other hand, it requires time and effort (which may not be available to you).

# On changing languages and tech stacks

![Project Structure]({{ site.baseurl }}/img/20221030/change.png)

Image from [Reddit](https://www.reddit.com/r/ProgrammerHumor/comments/8vrugj/programming_languages_problems_and_html/)

- **If you are a Junior or Middle automation engineer.** It is relatively simple to change the programming language and stacks at this level. If the new project has an established framework and/or senior engineer to mentor you - you can switch without fear. You will need to learn a lot - but a good mentor can make this learning process as quick and painless as possible. 

- **If you are a Senior engineer.** If you join the new company as the first engineer in the team, you will need time to learn new things and write proofs-of-concept that might only work on the first try. On the other hand - if you join a well-established team, your onboarding process will be more straightforward. 

- **Switching between statically typed languages** ​​(Java, C#, and C-like languages ​​in general) is quite simple. It will be a little more challenging to transition from statically typed to dynamically typed languages, such as JS, Python, Groovy, Ruby, etc. (or vice versa). Plan more time for learning new concepts. 

- **Few words about functional programming languages** ​​(Scala, Closure, Haskell, and others). It is possible to write in OOP style on some of them (in particular on Scala). For tests, in principle, it will be enough. But functional programming will still need to be mastered to get the most out of the language. And it takes even more time than the languages ​​from the previous point. Plus, you can only write in a functional style in languages like Haskell or Closure.

- **About working at FAANG and other western startups.** They all hire engineers - the people who can master one or another language in a short period (not instantly, of course) and write production or test code with it. But do not worry. In most cases, such companies will have a lot of experienced engineers who will help with code reviews and feedback. So what matters here - is your basic technical knowledge and problem-solving experience. Programming language - is only one thing among your tools.

# On skills

**Programming language is only a tiny part of what a test engineer or SDET should be able to do in automated testing.**

In addition to the language, there are also basic approaches to testing and automation. There is technical knowledge of various systems' architecture and how to test them. 

There are many additional tools for writing and running tests - build tools, automation frameworks, various libraries for logging, parallel runs, reporting, and CICD tools. And also - design patterns and the ability to write clean and understandable code.

**Most of the automation skills do not depend on the programming language.** You can find analogs in the selected technology stack for most of the tools. If such tools do not exist in the chosen technology, why not build it yourself?

**The main job of an engineer is to solve a business problem most efficiently. Effectiveness here lies primarily in how fast, scalable, and understandable the tool or approach is to other engineers and managers.**

# Conclusion

![Project Structure]({{ site.baseurl }}/img/20221030/automation.png)

Image from [Reddit](https://www.reddit.com/r/ProgrammerHumor/comments/gvvq3t/if_u_didnt_spend_6_hours_automating_a_task_that/)

Choosing a programming language for automation - is a hard thing. Especially if you are a single tester in a small team. Each language has its pros and cons. Each language brings an ecosystem with it. But **asking good questions BEFORE making such a choice** - **can decrease the amount of "headache"** in the future. 

Changing languages and stacks later in your career also have their own implications. You can be a **generalist** and scratch the surface of many languages. Or you can be a **specialist** in one language - but do technically complex things and optimizations. **It's up to you to decide which engineer you wanna be.** 

In the end, I want to state a few general things that you should never forget when you work with test automation:  

- **A few stable and fast tests are better than a "super framework."** If you plan to create only a Smoke suite from a pair of end-to-end tests "for every day, " you can do it with almost any tool. Just make sure that the tests are fast and stable. Such tests will bring benefits to the whole team.

- **Automation can take many forms.** It can be a UI or API test for the most critical features, deployment scripts, or database migration. Small script that automate daily routine tasks can bring more value to the team than a lot of simple UI tests.

- **Automation strategy is the first step.** If you plan to create automation tests at different levels, think about test coverage, launch on various platforms, then first create and communicate the automation strategy to management. Without a clear understanding of the "end" point, you will not be able to tell whether you are moving in the right direction.

- **Learn programming and the technology stack.** Writing a large project for automation requires the same knowledge and programming skills as writing feature code. Therefore, constantly improve your level of expertise.

