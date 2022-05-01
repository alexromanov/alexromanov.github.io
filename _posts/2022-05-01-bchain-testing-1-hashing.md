---
layout: post
title:  "Blockchain for Test Engineers: Hashing"
date:   2022-05-01 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220501/hashing.jpg"
description: "Hashing from tester's point of view"
summary: "Hashing from tester's point of view"
header-img: "img/20220501/hashing.jpg"

---

Photo by [Pan Yunbo on Unsplash](https://unsplash.com/photos/EgL0EtzL0Wc?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink). 

This is the first blog post in the "Blockchain for Test Engineers" series.  

When you start to study blockchain, the odds are pretty high that you will hear about **hashing** and **hash functions**.  
But what is hashing? Why is it a critical concept in cryptography? Which hashing algorithms are used for different blockchains? And even more interesting - how can we test a cryptographic hash function (and should we do it at all)?  
 
### What is hashing?
Hashing can be imagined as a "box" with one input and one output. You can provide any data as an input and get a result of the fixed length.  

![Project Structure]({{ site.baseurl }}/img/20220501/hash-function.png)

In the case of the SHA-256 hash function - the result will always be a 256-bit sequence. The most exciting thing in hashing is - that **even the minimal change to the input changes the output - significantly.**  

As an example, let's get the SHA-256 hash of the text "Test Engineering Notes".  
The result will be - **"dc3101f57c983aa68499306de0e60cb7266e3b1ae2235a84aa0d727f0f07b18f"**.  

But **"Test Engineering note"** will have completely different hash: **"16d164f46c6a29518420d0cdda6394969692cc82145b46eb89fe32af4a8b996e"**.  

### Properties of hash functions
The two main properties of the hash function are **ease of computation** and **determinism** (*the same input data should always result in the same hash*).

Besides that, a good hashing function should withstand cryptographic attacks.  
So it should have the following properties:  
- **Pre-image resistance**. The hash function should be a "one-way": Given a hash value h, it should be difficult to find any message m such that h = hash(m). 
- **Second pre-image resistance**. Given an input m1, it should be difficult to find a different input m2 such that hash(m1) = hash(m2).
- **Collision resistance**. It should be difficult to find two different messages m1 and m2, such that hash(m1) = hash(m2). Such two messages are called a hash collision. 

### Why does hashing exist? 
There are multiple applications of the hashing in the world of computers:  

- With hashing, you can check the integrity of the data;
- Instead of storing user passwords as plain text in a database - it is better to keep its hashes;
- You can use hashing in the digital signatures;
- Datatypes, such as Hash Tables and Hash Maps;
- Blockchains :);

### Different hash functions

The most famous are [MD5](https://en.wikipedia.org/wiki/MD5), [SHA-1](https://en.wikipedia.org/wiki/SHA-1), [RIPEMD-160](https://en.wikipedia.org/wiki/RIPEMD-160), [BLAKE](https://en.wikipedia.org/wiki/BLAKE2), [Whirlpool](https://en.wikipedia.org/wiki/Whirlpool_(cryptography)), [SHA-2](https://en.wikipedia.org/wiki/SHA-2), and [SHA-3](https://en.wikipedia.org/wiki/SHA-3). 
E.g., **Bitcoin** (and its forks) uses [SHA-256](https://en.wikipedia.org/wiki/SHA-2) cryptographic function, **Ethereum** - [SHA-3 (Keccak)](https://en.wikipedia.org/wiki/SHA-3), **Cardano** - [BLAKE2b-224](https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE2b_algorithm).

Those algorithms are based on some sequence of rounds with bit operations: shifts, AND, XOR operations, and others).  

![Project Structure]({{ site.baseurl }}/img/20220501/sha-2.png)

[Lane Wagner's blog post](https://blog.boot.dev/cryptography/how-sha-2-works-step-by-step-sha-256/) provides an excellent step-by-step explanation of the SHA-2 hash function. You can also try to **[hash something online](https://emn178.github.io/online-tools/sha256.html)**.

It is a vast mathematical task to create new hash functions and prove their properties. [National Institute of Standards and Technology](https://en.wikipedia.org/wiki/National_Institute_of_Standards_and_Technology)is reponsible for testing hash functions. The chosen one are added to the family of **[secure hash algorithms](https://en.wikipedia.org/wiki/Secure_Hash_Algorithms)** and recommeded by NIST. The last known standard algorithm (**Keccak)** was chosen at the **[NIST hash functions competition](https://en.wikipedia.org/wiki/NIST_hash_function_competition)**. Hash functions were evaluated by their performance, security, analysis, and diversity. 

### How to test hash functions?
One of the leading indicators of the quality of hash functions is the probability of getting hash collisions. So one test is to check collisions at the massive input data. 

Usually, the distribution of hash values ​​is uniform and is tested using the **[Chi-square test](https://en.wikipedia.org/wiki/Chi-squared_test)**. The actual distribution of elements is compared with the expected (uniform) distribution. The ratio within the confidence interval should be in the range of 0.95 - 1.05.

Also, there is an additional test for uniformity of distribution of hashes. It is based on **[strict avalanche criteria](https://en.wikipedia.org/wiki/Avalanche_effect)** - when each input bit changes with a probability of 50% in the output sequence. 

### Will you test hashing function as a blockchain test engineer? 
As always - it depends. Many blockchains use well-known standards and rarely implement hash functions from scratch.  

Suppose you will be a part of the core engineering team responsible for developing the whole blockchain from scratch. In that case, you may need to verify that hashing algorithm works as expected within the context of the system. 

If you test only blockchain-based applications based on smart contracts - you don't need to verify the hashing function in isolation. You should trust other security specialists and mathematicians who tested these functions for you. But you definitely need to know how such functions are used in your system. 




 