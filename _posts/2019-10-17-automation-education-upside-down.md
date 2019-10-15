---
layout: post
title:  "Turning automation education upside-down"
date:   2019-10-17 00:00:47 +0300
author: "Oleksandr Romanov"
description: "Usage of flipped classroom approach in test automation education"
header-img: "img/20191015/upsidedown.jpg"

---
**Audience: QA Leads, QA Automation Engineers, QA Managers**

### The story  
I joined [Playtika][playtika] two years ago. The first thing that I noticed here was a really tight release schedule. It was more than a 40 releases per week (web, mobile platforms and also separate services).  

QA Engineers inside Scrum teams tested not only newly created functionality, but also - they stuck at regression testing. A lot of regression testing. This leads us a point when test automation is not desired - it is inevitable. 

Test automation can't be done effectively by separate team in our context. That's why, we decided to teach our QA Engineers to use internal solutions and create / fix  automated tests.  

### First attempts  

First of all, I defined the roles in our upcoming education course. The roles seemed to be very obvious - the students and lector / mentor. The lector provides lectures parts, but also should be responsible for the code reviews of the homework and giving feedbacks to the students.  

![Project Structure]({{ site.baseurl }}/img/20191015/traditionalEd.png)

I came to the most traditional way of education: lectures at the classroom and homework at home. As a result a new course has been created with the name - QA Academy.  

![Project Structure]({{ site.baseurl }}/img/20191015/qaacademy.png)

The format of the course was: 3 times per week by 1.5 hour for 3 months. The size of the group was chosen to be no more than 10 people. The course has been divided into two parts - Java and Automation.  

Why start with programming language first?  
Without knowledge of programming language it is impossible to contribute any  valuable test automation (if we are not talking about "record-playback" tools - but that's another story).  

Programming language part was mostly dedicated to Java core knowledge - from data types and loops to classes, objects and exceptions. Automation part was a combination of general knowledge about Git, Maven and our internal automation solutions for API and UI testing.  

What was the measurement of course success  / failure? I chose a definitive metric - automation contributions (during course and right after it). Automation contributions means amount of pull requests merged into master branch of any automation solution either it's a bug fix or creating a completely new automated test from scratch. 

### Results and reflections  

![Project Structure]({{ site.baseurl }}/img/20191015/firstRes.png)

After the first and second groups, we got the following results:
 - first group - 3 out of 10 
 - second group - 2 out of 10  

The results were not so successful, so I started to think and analyze - what was done bad and how it can be improved.  

What were the pitfalls:  

1. The course was completely optional. That's why there was no dedication to complete it.  

2. Too much focus on theory - and not enough practice.  

3. The recordings of the lectures were not organized. I did only a full lecture recording. As a result - I got a collection of 1.5 hour videos. It was not useful for students to watch all of the videos.

4. The total lack of time. Students did not have enough time to practice writing automation tests during the course and even just right after the course completion.  

### In a search of new way  

I started to seek for a better ways of education.  
One of the approach that I have discovered was a "flipped classroom".
The main idea of this approach is literally "turning education upside-down": do theory at home and practice at the lesson.  

![Project Structure]({{ site.baseurl }}/img/20191015/flippedEd.png)

More precisely, the approach is: watch prerecorded theory videos at home - do homework assignments at classroom. More than that - it turns out that it's not only a "threoretical approach".  
Some of the public schools in California, US has been applied this approach (also with using Khan Academy educational platform) for teaching mathematics to kids. 
And results were successful.  

So I had a thought - "Why not? How can I apply flipped classroom approach to automation part of the course? How should I update the QA Academy course?"  

![Project Structure]({{ site.baseurl }}/img/20191015/qaAcademyProcess.png)

Improvements:  

1. I split lector's and mentor's responsibilities on two different set of people. Lectors can concentrate on teaching, mentors - on code-reviews and feedbacks.   

2. I divided automation part of the course onto set of small parts - up to 10 - 15 minutes long - and record them in a form of short video lessons.  

3. The format of education was the following: students watch and practice a couple of lessons at home and after that that - ask questions and write automated tests on practice.  

4. After practice lessons students can ask additional questions to their mentors and a code reviews from them 

The general timeline of the course was the same - 1.5 month for Java lesson and 1.5 month for automation. Java part was done using traditional approach to education (without any change).

### New results  

![Project Structure]({{ site.baseurl }}/img/20191015/secondRes.png)

After the third (revised) group of QA Academy, we tracked automation contributions  again. It turns out, that all students have contributed one or more tests during education and more than one test just right after the course. The involvement of students during the course was also increased. 

### Conclusions

In conclusion, I want to say that our experiments with "flipped classroom" approach is not finished right now - we are planning to make more for the upcoming groups of QA Academy.  

Stay tuned ad code wisely.  

[playtika]: https://www.playtika.com/