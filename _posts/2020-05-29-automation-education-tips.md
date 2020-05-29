---
layout: post
title:  "Conducting courses for QA Engineers"
date:   2020-05-29 00:00:49 +0300
author: "Oleksandr Romanov"
description: "Things to consider before starting an internal automation course for QA Engineers"
header-img: "img/20200529/education.jpg"

---

## The need for education

Automated testing or automation in testing has been in the software industry almost from the beginning of the times.  

No matter how you call it - many QA Engineers want to master test automation and use it in their daily work.  
Some of them learn in their spare time. In case of a strong motivation - it can bring results for the engineer (even a career change).  

But in a case, if the whole organization wants to move to automated testing - there is a need for dedicated internal coaching.  

Why am I sharing this?  

I have provided internal test automation courses for 4 groups of QA Engineers during 2018-2019. During the courses, I had an opportunity to try different ways of education: from traditional one to ["flipped classroom"][AEUD].  

My experience is limited compared to a fulltime lectors or course teachers. But if you are thinking about starting internal automation courses in your organization - I hope that my tips below can help you a bit.  

## Before the course: asking questions

![Project Structure]({{ site.baseurl }}/img/20200529/before.jpg)

Before starting any educational activity - gather requirements and expectations from all stakeholders of the process. Here are a few questions that you may want to clarify:  

1. Ask Managers  

    - What is the end goal of educating QA Engineers with automation skills?  
    - What is the expected level of skills after QA Engineer will finish the course?  
    - How the course will affect day-to-day QA activities for the teams? Will the course be conducted during work hours or after work?  
    - What is the acceptable length of the course?  
    - How software development and delivery process will be changed in terms of adding automation as an activity? Do we have planned time for automation?  
    - Do we expect to provide a different career path for QA Engineers after the course or consider it as an additional tool in the skillset?  
    - Are we consider the course as mandatory?  

2. Ask QA Engineers  

   - What is the primary motivation of the QA Engineer? Is it gaining new skills or just to "try out automated testing"?  Do not spend your time on "trying out". Use external videos or courses instead.  
   - What are the required pre-requisite skills for the course? Is it for complete beginners or requires programming knowledge foundation?  
   - Do QA's expect that the educational process will take time and effort?  
   - How does QA Engineer expect to apply the automation knowledge after the course?  

3. Ask Developers  

   - Are developers ready to help their fellow QA Engineers with programming questions during the education and right after it?  
   - Are developers willing to dedicate time for code reviews for QA Engineers' automated test code?

4. Ask Lectors and mentors

    - Do you have lectors that want to share their knowledge?  
    - What will be the format of the course?  
    - Will the course be dedicated only to automation or include programming language also?  
    - Do lectors have enough time to prepare course materials and home tasks?  
    - Do lectors plan to add additional video materials for the course?  
    - Do we have a group of mentors for performing code reviews and answering questions during education?  
    - What will be the format of evaluation (exam, interview, task) after the course?  
    - Do we have a dedicated meeting room for the course and necessary equipment?  

## During the course: tracking progress

![Project Structure]({{ site.baseurl }}/img/20200529/during.jpg)

- Concentrate on internal automation solutions and processes that can be applied just right away.  
- Track if students struggle with some concepts and dedicate additional time for explanation.  
- Be ready to dedicate time to solve technical issues such as library and tools installation, tools usage, etc.  
- Consider record additional videos or write additional wiki pages for the tools and processes used in automation.  
- Encourage students to help each other to solve technical issues during the course.
- Due to the limited course time - do not expect that students will start to produce high-quality automation code from scratch.

## After the course: monitoring results

![Project Structure]({{ site.baseurl }}/img/20200529/after.jpg)

- Ask fellow engineers from the team to conduct automation interviews for the students. It will help to bring QA Engineers in terms of a real-world job interview and reveal potential points for improvements.  
- Implement the way how to track that QA Engineers are contributing to automation in a consistent manner.  In our case, we have a Grafana dashboard that shows all QA contributions to automation-related repositories.  
- Remember that if the knowledge is not practiced after the course - it will be forgotten in one or two months.  
- Recording videos from the course can somehow help - QA Engineer can re-watch videos on-demand and recall how to implement tests.  
- Update course materials from time to time as new tools and processes always appear.  

## Overall outcome

![Project Structure]({{ site.baseurl }}/img/20200529/results.jpg)

Here are clear numbers (2018 - 2019):

- format: internal courses (Java + Automation);
- education: traditional and then flipped classroom;
- conducted: 4 courses;
- length of course: 3-4 months;
- total students: 35;
- results:  
  - 18 QA Engineers are implementing automated tests on per Sprint basis;
  - 6 QA Engineers have been promoted to Software Engineer In Test / Test Automation roles;
  - 1 QA Engineer has switched to a Software Engineer role;
  - newcomer QAs are using course materials in order to get information about the tools and start to implement tests without additional training;  

## Education now

Currently, we are not conducting dedicated automation courses - all QA Engineers either have necessary skills or learn it on their own.  
We have switched to the more in-person format of education:  

- for each QA Engineer, we have built an automation development path;
- the path requires independently getting knowledge from external resources as well as from pre-recorded automation videos about our product-specific solutions;
- each engineer moves through the path in own pace;
- automation knowledge is checked via code reviews of pull requests and interviews;
- additional automation mentor may be requested if needed;
  
## Conclusion

As any tool or process - automation courses for QA Engineers can bring value if it's applied correctly. The more you think and prepare before the course - the better results you will get in the end.  

Gather information, evaluate the risks, and implement it effectively.  

Good luck with your education!

[AEUD]: https://alexromanov.github.io/2019/10/16/automation-education-upside-down/
