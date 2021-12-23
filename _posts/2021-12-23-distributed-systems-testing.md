---
layout: post
title:  "Why does the tester need to bother about distributed systems?"
date:   2021-12-23 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20211223/distributed.jpg"
description: "Distributed systems, fallacies and testing"
summary: "Distributed systems, fallacies and testing"
header-img: "img/20211223/distributed.jpg"

---

Photo by <a href="https://unsplash.com/@alinnnaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Alina Grubnyak</a> on <a href="https://unsplash.com/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## What is a Distributed System?
In simple words, a **distributed system** is a system that **consists of many nodes** (for example, computers, processes, devices) that **communicate over a network** and **perform some form of a task together.**

[Leslie Lamport](https://en.wikipedia.org/wiki/Leslie_Lamport), a renowned researcher and author of scientific papers, gave this description:
*". . . a system in which the failure of a computer you didn’t even know existed can render your own computer unusable."*

## Why do we need distributed systems at all?
* **Resiliency.** If all calculations are carried out on only one machine, then the failure of such machine will mean that the entire system will be inaccessible to the user.
* **Performance.** Even the most powerful one, no single computer can handle simultaneous requests from millions and billions of users.
* **Handle significant computational problems.** Many tasks will be either impossible or highly time-consuming to compute on a single machine.

## What are the misconceptions about distributed systems?
Developers who write code for distributed systems are far from ideal. Especially if they are new to the industry or have never dealt with really huge systems. Such developers can form many false assumptions about distributed systems in their heads.

[Peter Deutsch](https://en.wikipedia.org/wiki/L._Peter_Deutsch) (and several other engineers) from Sun Microsystems back in the 90s formed a list of these assumptions. It can be found as "8 fallacies of distributed computing" on the Internet.

**Eight misconceptions about distributed systems:**
* **The network is reliable.** In reality, packets in the network can be lost; services can stop and endlessly wait for a response.
* **Network latency is zero.** There will always be network latency - at least, it's limited by the speed of light. It is impossible to instantly transmit a packet over the network from one point on the Earth to another.
* **The bandwidth is infinite.** The bandwidth is limited, but it can also be different for different nodes.
* **The network is secure.** In fact, any messages sent over the network can be intercepted by attackers. Or hosts can be attacked or blocked.
* **The network topology does not change.**
* **There is always one administrator on the network.** Nodes and subnets can be managed by many various companies with different security policies.
* **Transport costs are zero.**
* **The network is homogeneous.** In the real world, nodes on a network can be completely different, with different characteristics.

## Why would I need to know about distributed systems - I'm just a tester?
If you are testing an application that is little more than a couple of simple scripts, there is a non-zero chance that you are developing (or using) distributed systems in one way or another.

- Does your application have a **database**? It may be distributed.
- Do you have **microservices** on the backend? It is also, in essence, a distributed system.
Do load balancers regulate the incoming traffic? Balancers can also be distributed.
- Components exchange messages via **messaging** (for example, Apache Kafka)? And that can be distributed too.
Do you host **several data centers** in different parts of the world to give users faster answers? This is also a distributed system.

In fact, **almost any large modern application is a distributed system. And consists of several smaller systems.**

In my opinion, test engineers who deal with distributed systems in one way or another, should at least know these three things:
- how do these systems work? (easy level)
- how can these systems fail? (medium level)
- how can the risk of failure be reduced? (hard level)

## Conclusion
Distributed systems are almost everywhere in a modern technology world. If you know how such systems can fail - you will be able to prevent a lot of such problems in your products.

