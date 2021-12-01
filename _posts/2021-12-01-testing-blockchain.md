---
layout: post
title:  "What does it mean 'to test a blockchain' and which knowledge do you need for it"
date:   2021-12-01 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20211201/blockchain.jpg"
description: "Different forms of testing blockchain"
summary: "Different forms of testing blockchain"
header-img: "img/20211201/blockchain.jpg"

---

Photo by <a href="https://unsplash.com/@a_uem?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Akinori UEMURA</a> on <a href="https://unsplash.com/s/photos/chain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

Blockchain is a hype topic nowadays. It seems that every other person is making their own cryptocurrency or opening a cryptocurrency exchange at the corner of the street :).

To better understand what blockchain is and how does it work, I highly recommend these introductory videos: [1](https://youtu.be/SSo_EIwHSd4) and [2](https://youtu.be/bBC-nXj3Ng4)

But what blockchain brings to the testing world? How deeply do you need to know about blockchain to test with such systems?

In this blog post, I will describe a few different cases of testing blockchains. Also, I will share my list of things you need to know to bring quality to such systems successfully.

## Blockchain networks

It is the most "hardcore" option. In this case, the entire blockchain network is tested - as a holistic solution. For example - verification of Bitcoin, Ethereum networks.

What we test:
- storing transactions in blocks and communication between them
- cryptographic encryption and key management protocols
- data replication between nodes in the network
- consensus algorithms
- system performance
- system security
- fault tolerance of the system

## Blockchain-based applications

Here we are dealing with ordinary Web and Mobile applications, in which one of the functions is either to read something from the blockchain network or to write some data to it.

Communication with the blockchain on the backend side takes place through API requests. Often this is not an HTTP REST API but something more specific like gRPC.

The API can be either a third-party system or just a node of the blockchain network located inside the system.

What we test:
- integration with blockchain
- functional and non-functional application properties (as at any other application)
- security and performance

Pay attention that the application's performance can depend on how fast blockchain works at the backend.

## Cryptowallets

Wallets are applications that store information about your passwords and enable you to buy and sell cryptocurrency. They can be both in the form of web and mobile applications as well as desktop.

The main aspect when testing is the use of test blockchain networks (TESTNET) for transactions. The currency is technically the same as the real one in such networks, but it costs nothing.

In addition, there may also be specific types of wallets in individual electronic devices. And this is a separate exciting world of embedded devices.

## Smart contracts

Smart contracts are software code (uploaded to the blockchain) that implements some business logic. Most often, smart contracts mean the code in the Solidity language in the Ethereum blockchain. But there are other programming languages available in different blockchains.

What we test:
- unit tests for contracts
- functional tests contracts
- security testing

The unusual thing about testing is that it starts working immediately after the code gets to the blockchain (“deployed to production”). It is challenging to "stop" it. And it’s impossible to delete or revert the deployment.

## Cryptocurrency exchanges

Exchanges are usually web (or mobile) applications that allow you to create an account and trade (convert) different cryptocurrencies.

They are very similar to ordinary sites for banking or currency exchanges.

The main difficulty of testing here is integration with APIs of many hundreds (or even thousands) of blockchains. So get ready for integration bugs and quick fixes.

## What do you need to know and understand to test the blockchain?

1. Hashing: how does it work, and what are the different algorithms for it
2. Cryptography: symmetric and asymmetric, electronic signatures
3. Consensus protocols and their types
4. Distributed systems and methods of communication between a node in a network
5. Transaction models: UTxO (Bitcoin) or account-based (Ethereum)
6. What are the transactions, and how are they stored (hello Merkle trees :))
7. Types of blockchains (Mainnet vs. Testnet, open and permissioned)
8. Types of crypto wallets and principles of their work
9. Smart contracts (why are they needed)
10. Programming languages ​​of smart contracts - Solidity, Plutus, etc
11. Tools for local deployment of blockchains for testing
12. Smart contract testing tools
13. Vulnerabilities and problems of blockchains and smart contracts in particular
14. API testing (HTTP REST, gRPC, etc.)

## Conclusions

The world of blockchain is vast and continues to grow. It is not easy to get an initial knowledge about the topic because it is a mix of different areas: distributed systems, cryptography, finance, and others.  

As there are not so many tools for testing blockchains, you may need to dig through a lot of whitepapers to get specific knowledge. And here is the beauty of blockchain (IMHO) - a short way from academic papers to the real-world implementation of the new concepts.  

If you, like myself, are interested in blockchain testing - I prepared [a list of resources](https://github.com/alexromanov/awesome-blockchain-testing) about this topic. It is open-source - so feel free to use it and extend it with new blog posts and papers.

