---
layout: post
title:  "How complex and challenging testing can be"
date:   2017-04-06 21:25:47 +0300
author: "Oleksandr Romanov"
description: "In the following post I will talk about my experience of testing huge financial systems"

---

_In this short post I will talk about my experience of testing huge financial systems_ 

**Target audience:** QA Automation engineers / QA Engineers  

### Testing in the realm of a big enterprise company

Last year I had an opportunity to work in large financial enterprise company. It was an incredible experience and I really enjoyed it. I want to say thank you to all of my teammates and fellow testers - you are awesome! :)  

In this short blog post I want to highlight challenges which I have explored during my work. For some of the readers who have experience in top-world big companies these insights may not be interesting. But for testers, who are involved mostly at outsorcing and have small teams - it may be interesting.  

Another thing, that I want to mentioned - is that my previous work experience relates to outsourcing and out - stuffing more or less: projects were small - medium size.  

The challenges in testing system of systems differ from the regular applications in most cases.  

Here are the lessons that I learned:   

1. The testing requires a lot of business knowledge. I really mean -  a lot! In some cases it is crucial. You can't perform any sufficient testing without understanding how exactly and why business user operates with the system in some or other ways.  
It takes us to close cooperation with business analysts or domain specialists, and even real world traders - the users of the systems. If you are testing some website or e-commerce online store - you can have some previous experience. But if you deal with a complex solutions for financial operations - it takes a lot of time and resources to even closely get to the point of view of system's users.  
Remember that the most valuable bugs for user that you can catch - involves business logic.   

2. Applications under test are rarely simple and separate from other systems.
Be prepared to test a huge workflow involving up to 8 - 10 different systems and projects. It adds a complexity not only at manual testing part, but also at integration testing activities.

3. Almost for all applications - performance and stability are the most critical requirements. 
In some cases performance is evaluated not only in milliseconds, but in  microseconds. 

4. Test automation  often relies completely on custom in-house solutions. 
Always remember, that test automation engineering goes far beyond Selenium Webdriver, Appium and some record and playback tools for desktop gui. So be prepared to write any technical solution even from scratch (even to desktop GUI, services, backend, deployments, etc).
It requires from testers and test automation engineers to have solid ground in software development techniques.

### Conclusion
The main point that I learned from working in large company that testing is not simple and not limited to just web or mobile applications. The technology world is definitely huge and there are a lot of things to test and automate.