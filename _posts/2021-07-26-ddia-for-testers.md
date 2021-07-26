---
layout: post
title:  "Book Review: Designing Data-Intensive Applications"
date:   2021-07-26 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210726/ddia.png"
description: "Why every test engineer should read DDIA book"
summary: "Why every test engineer should read DDIA book"
header-img: "img/20210726/ddia.png"

---

Being a test engineer does not mean blindly execute test cases for each regression cycle. To design more valuable test scenarios, you need to get more knowledge about the product.  

Business cases are the first source of information. But if you develop something a bit more complex than a simple single-page application, you may need to consider how the system under test works internally.  
 
[***"Designing Data-Intensive Applications"***](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/) (latter DDIA) by **Martin Klepmann** - is a book that explains how modern applications can work and fail.

## World of distributed systems

Here are a few questions that you can answer after reading the book:

### Modern applications:
  ![Project Structure]({{ site.baseurl }}/img/20210726/sys_architecture.png)  

  - Why do modern systems care about **reliability**, **scalability**, and **maintainability**?
  - How to describe the **performance characteristics** of the systems?
  - Why is **maintainability** also worth considering in the design phase? 


### Data storage and delivery:
  ![Project Structure]({{ site.baseurl }}/img/20210726/protobuffs.png)
  - How do storage systems work **internally**? 
  - When do you need to choose **SQL**, **NoSQL**, or **GraphQL** databases? 
  - What are the different **data distribution models** (other than JSON or XML)?  

### Distributed systems:
  ![Project Structure]({{ site.baseurl }}/img/20210726/error.png)  

  - What is data **replication**? Which problems can occur during replication (at multi-leader and leaderless setups)?
  - What is data **partitioning**, and how may it fail? 
  - Do you need to care about **serializability** when working with transactions? What are different ways how **transactions** can cause errors?
  - Which **errors can occur to distributed systems**? What is the difference between **network failures**, **unsynchronized clocks**, and **consensus violations**? 
  - What are the pros and cons of **batch** versus **stream processing**, and which errors may occur during data processing?

## Conclusions  

DDIA book is not an easy read. It requires a lot of prerequisite knowledge or an extensive additional "googling" of concepts on the way.

And it is not for beginner tester for sure. 

But it is 100% worth reading. 
It gives you a more understanding of how complex systems may and can fail.  

The author provides a very practical examples of failure scenarios.
With this knowledge, you, as a tester, can find more "hard to find" bugs and prevent critical issues with your applications. 
