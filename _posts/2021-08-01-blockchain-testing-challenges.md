---
layout: post
title:  "Challenges in testing blockchain-based applications"
date:   2021-08-01 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210801/bc.jpg"
description: "Why is blockchain testing not an easy thing?"
summary: "Why is blockchain testing not an easy thing?"
header-img: "img/20210801/bc.jpg"

---

Photo by <a href="https://unsplash.com/@blueskin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Terry</a> on <a href="https://unsplash.com/s/photos/blockchain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## Blockchain and applications

[Blockchain](https://blockgeeks.com/guides/what-is-blockchain-technology) is a linked list of records called blocks.  
The blocks are connected using [cryptography](https://en.wikipedia.org/wiki/Cryptography): each block contains the previous block's hash and information about current transactions.  

![Project Structure]({{ site.baseurl }}/img/20210801/blockchain.png) 

The blocks are immutable to change and are replicated across nodes in the blockchain [peer-to-peer](https://en.wikipedia.org/wiki/Peer-to-peer) network.  

The most famous blockchain implementation, for now, is Bitcoin. But blockchain is not limited only to [cryptocurrencies](https://blockgeeks.com/guides/what-is-cryptocurrency/).  

As blockchain provides immutability, integrity, and transparency, it can store any data, not just transactions. Since the beginning of the [Bitcoin](https://bitcoin.org/en/) release, many companies are constantly exploring new ways to apply blockchain to the current processes in the world.  

The creation of [smart contracts](https://blockgeeks.com/guides/smart-contracts) in [Ethereum](https://ethereum.org/en/) allowed simulating many business processes on the blockchain in a code (with either manual or automatic execution). As a result, now it is possible to build Blockchain-Based applications that allow communication with a given blockchain to store data, make transactions, or execute smart contracts.  

All these blockchain applications deal with currency (tokens) or store and keep data on the chain. So we need not only build them but also ensure that the applications work as expected (from functional, performance, and security points of view).  

In this blog post, I will describe the challenges in testing blockchain-based applications based on my experience and also a tremendous research paper of Chhagan Lal and Dusica Marijan - ["Blockchain Testing: Challenges, Techniques, and Research Directions."](https://arxiv.org/pdf/2103.10074.pdf) 

## Architecture and testing

Modern blockchain-based applications do not implement blockchain "from scratch". They typically use blockchain networks as one of the components of the system.  

Here is the sample architecture diagram of the blockchain-based application:

![Project Structure]({{ site.baseurl }}/img/20210801/bc-app.png)


**According to the diagram, the following components need to be tested:**
- unit-testing for business logic
- integration between application and blockchain components (API)
- functional testing of blockchain components (smart contract testing)
- integration between application and data storage layer (API)
- end-to-end testing (UI and API)

**Few notes:**
* Integration between application and blockchain network is typically tested via API (REST, gRPC, command-line)

* If application developers use an existing blockchain network, they may not test its internals (the same with database systems or messaging libraries).

* Smart contracts verification include:
  * functional testing (whether business logic works as expected)
  * performance testing (check execution and time complexity)
  * security testing (data protection and vulnerabilities)

**As blockchain and smart contracts is a reasonably new technology, test engineers face with several challenges during testing blockchain-based applications:**
* lack of practices
* lack of tools
* immutability
* performance
* dependability

## Lack of practices (development and testing)

**Knowledge.** To develop and test blockchain-based applications, engineers need to know about various domains (technical, non-technical, legal). But it is hard to find people with such knowledge on the market. E.g., in our company, test engineers need to know distributed systems, cryptography, blockchain, testing, programming, math, finance.  

**Variety.** For now it is almost [40 different blockchains](https://docs.google.com/spreadsheets/d/1OO06RZ7vw8-Hij8ZxB68FaRYRtQEz3GifnLDNwW8sTs/edit#gid=1051902784) available on the market. You can't just "learn" one blockchain: there is an ecosystem of tools for each particular blockchain. Some blockchains use well-known programming languages like C++, Go, Javascript or Java for implementing smart contracts. Another - use their unique programming languages ([Solidity](https://docs.soliditylang.org/en/v0.8.6/) for Ethereum or [Plutus](https://plutus.readthedocs.io/en/latest/plutus/tutorials/plutus-playground.html) for Cardano) - so you need to learn them too.  

**Standards.** Additionally, there is a lack of guiding procedures, standards, or strategies in designing, developing, and testing blockchain-based applications

## Lack of tools

**Libraries**. As there are many programming languages for smart contracts - there is a lack of mature debugging / testing tools for these languages.  

**Automation**. If you want to ensure that the application works as expected, you need to test the components (in isolation) and system (as a whole). Currently, there is a lack of deployment and automation tools, which allow to deploy and configure blockchain in a "test mode" within one machine's boundaries.  

**Environment configuration** takes a lot of time. So the only thing now is to download the full node locally, synchronize it with other nodes and then test it via command-line or API. As a result, the configuration takes an enormous amount of time. If your application uses some wallet functionality, you also need to download and configure it. 
 
## Immutability

The blockchain is an immutable and open data structure (unless it is not a permissioned one). So as soon as any change is deployed to the network, you can't reverse it back.  
Seriously. All the test data will remain on the chain forever :).  
Moreover, the test data can be seen by any person via transaction explorers.

**Test data** need special care as it is available in a public blockchain. Make sure that you don't use any sensitive information in tests.

**Validate smart contracts** more and more before deploying them to the chain. Any error that slipped to the contract after the deployment can't be fixed easily - in some cases, you need to create another contract to send money back to the failed accounts (or even change the whole network after a hacking attack).

**You can't delete the data from the chain.** So, if your application needs to follow GDPR ([right-to-be-forgotten](https://gdpr.eu/right-to-be-forgotten/) in particular), you need to consider which data to store.

## Performance  

The performance of the blockchain-based application is highly tied with the performance of the blockchain components themselves. Some blockchains have a limited latency and rate of input transactions, so they need to be considered in the overall testing strategy.

**Transaction validation** time depends on a particular chain's consensus protocols - in some cases, it can take minutes or even more for storing one piece of data. 

**Real-world** blockchain workloads are hard to predict due to volatility in blockchain popularity, hacking attacks, and other events.

## Dependability  

**Fault-injections** is a powerful technique in testing highly distributed systems. But due to the dispersed nature of the blockchain systems, it is hard to separate between system level, network level, and user-level fault loads. So there is a lack of tools in this area - mainly because each blockchain has its APIs and components. 

## Conclusions

Blockchain-based applications give a new set of challenges to the testing world. You need a whole lot of knowledge and skills.  

But no worries. Imagine, somewhere like 20 years ago, nobody knows how to implement automated tests for web and mobile. There were times when Selenium Webdriver or Appium does not exist. 

If we, as testers, want to succeed in the blockchain world and bring quality "to the table," - we need to dig into technical aspects of the systems and apply known (and maybe yet unknown) testing techniques and tools. 

We will explore functional, performance, and security challenges and tools in more detail in the following posts. 

