---
layout: post
title:  "Blockchain for Test Engineers: Merkle Trees"
date:   2022-06-19 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220619/roots.jpg"
description: "What are Merkle Trees - from tester's point view"
summary: "What are Merkle Trees - from tester's point view"
header-img: "img/20220619/roots.jpg"

---

Photo by [Camille Brodard on Unsplash](https://unsplash.com/photos/peOp2E3Zukk?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

This blog post is a part of [**"Blockchain for Test Engineers"**](https://alexromanov.github.io/2022/04/24/blockchain-testing-mindmap/) series.  

In previous articles, we have already discussed **[hash functions](https://alexromanov.github.io/2022/05/01/bchain-testing-1-hashing/)** and their usage. Today I propose to learn about an exciting data structure built using hashes - **Merkle Tree**.

## Blockchain and transactions
A **[blockchain](https://alexromanov.github.io/2022/06/05/bchain-test-what-is-blockchain/)** is a chain of data blocks connected cryptographically (each subsequent block points to the previous one).

![Project Structure]({{ site.baseurl }}/img/20220619/blockchain.png)

In addition, each block stores a lot of other interesting information: the hash of the block itself, the number of transactions, the complexity of the mining algorithm, and more. But most importantly - each block contains a set of validated transactions. 

![Project Structure]({{ site.baseurl }}/img/20220619/block.png)

But how can a user verify that a transaction is inside the block? Wasn't it changed during the mining phase?  

In the simplest case, the user must download the entire blockchain to his computer and check the required blocks. Each block takes up, on average, about 1MB. Therefore, the transaction verification process can take a long time (and requires hard disk space too).

But there is a way to check transactions quickly and with a much smaller data set than the whole block? The answer is yes - it is **[Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree)** data structure.

## What is Merkle Tree?
Imagine that we have eight transactions ready to be included in the block:

![Project Structure]({{ site.baseurl }}/img/20220619/trxs.png)

Apply hash functions to each transaction:

![Project Structure]({{ site.baseurl }}/img/20220619/trxhashes.png)

Let's concatentate each pair of hashes and then hash the result:

![Project Structure]({{ site.baseurl }}/img/20220619/round2.png)

We will continue the pairing and hashing processes until we get a single value:

![Project Structure]({{ site.baseurl }}/img/20220619/tree.png)

The resulting binary tree is called a **Merkel Tree**. The last hash obtained is the tree's root (Merkle Root). This root is also a part of the header of each block in the Bitcoin blockchain.  

Other parts of the Merkle Tree are **non-leaf nodes** and **leaf nodes.** 

## What is the meaning of the Merkle Tree?
The advantage of organizing transactions in the form of Merkle Tree manifests when we check the integrity of the transaction.   

In a simple case you can download all eight transactions, then generate a leaf nodes and recreate the process of creating a tree from scratch. And then - compare the resulting root of the tree with the one stored inside the block.

We can do it for a dozen transactions. But if there are **thousands** of transactions? Or millions, or maybe more? For example, some projects can contain **billions** of Merkle Tree leaves. So to re-create the tree, you need to compute a vast amount of hashes with the **O(n) complexity**. 

**Merkle Proofs** allows you to mathematically check the integrity of the transaction without the need to download and recreate all transactions. You need to perform much fewer operations. 

For example, we need to check **transaction 2**. In this case, we need to have Merkle Tree root and Merkle Proof for this transaction:

![Project Structure]({{ site.baseurl }}/img/20220619/proof.png)

In our example, the proof contains only 3 hashes and another 4 hashes we need to calculate to get a Merke Root. Otherwise, we will need to download 8 transactions and calculate 15 hashes. 

Instead of donwloading n transactions, we need to download **O(log n)**-sized Merkle Proof and also - Merkle Root.

In the Bitcoin blockchain, if we need to verify that transaction is indeed in the block, we need to download the block header (80 bytes instead of 1MB block) and then request Merkle Proof for a specific transaction. As a result, we can unambiguously check the integrity of the transaction with much lower resource costs.  

Merkle Tree is used in Bitcoin's "light" clients (Simple Payment Verification nodes) to quickly validate if transaction belongs to a particular block. 

## Where you can find Merkle Trees?
It turns out, that Merkle Trees are used in many modern systems:

- **[Git](https://en.wikipedia.org/wiki/Git_(software))** and **[Mercurial](https://en.wikipedia.org/wiki/Mercurial)** version control systems - for storing versions of data)
- **[Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra)**, **[DynamoDB](https://en.wikipedia.org/wiki/Dynamo_(storage_system))** - for detecting inconsistencies between replicas and minimize the amount of data transferred
- **[Bitcoin](https://en.wikipedia.org/wiki/Bitcoin)**, **[Ethereum](https://en.wikipedia.org/wiki/Ethereum)** blockchains - for validating integrity of transactions
- **[IPFS](https://en.wikipedia.org/wiki/InterPlanetary_File_System)** file system