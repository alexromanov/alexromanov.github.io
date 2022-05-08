---
layout: post
title:  "Blockchain for Test Engineers: Encryption"
date:   2022-05-08 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220508/encryption.jpg"
description: "Encryption from tester's point of view"
summary: "Encryption from tester's point of view"
header-img: "img/20220508/encryption.jpg"

---

Photo by [Mauro Sbicego on Unsplash](https://unsplash.com/photos/4hfpVsi-gSg?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

This is the second blog post in the [**"Blockchain for Test Engineers"**](https://alexromanov.github.io/2022/04/24/blockchain-testing-mindmap/) series.  

We will dive into a fascinating world of ciphers and encryption in this post. 

### The message to a friend

So you want to send a message to your friend. 

**Attempt # 1 - send a letter via post service**  

![Project Structure]({{ site.baseurl }}/img/20220508/attempt1.png)

If you write it in a letter and send it using your local post office - there is some chance that someone can steal the letter and read the message. The reason is that message is written in plain text. 
But you need to keep the message **private** - so only you and your friend can read it. 

**Attempt # 2 - send a box with a lock**  

You need a piece of paper, a box and a lock with two keys. You write a message and put it in the box. Then, you lock it with your key and pass it to the carrier. The carrier will bring the box to the friend. 
Your friend will use their key, open the lock and read your message. Hooray! Looks like the puzzle is solved. 

![Project Structure]({{ site.baseurl }}/img/20220508/attempt2.png)

But we have some weaknesses:
- we need to make sure that there are **only two keys** to the lock **that exist**
- carrier (or someone else) can steal the box and try to break the lock (using a picklock or a hammer) and get the message
- before sending a message - **we need to give a key to the friend** (so meet him physically in some place)

But in our global world, your friend can be in another city or even on another continent. Also, if you will understand that the lock is broken, you need to exchange a new portion of keys with your friend. 

### Cryptography

Maybe there is another way to transform your message into a non-readable way, send it to a friend and return it to the original form?  
Even in ancient times, the military forces tried to invent ways to communicate reliably and securely. 

Thus a new science has emerged - **cryptography.** [**Cryptography**](https://en.wikipedia.org/wiki/Cryptography) - is a study of techniques for secure communication in the presence of adversarial behavior. The science consists of two parts: one that studies ways of hiding information (**encryption**) using ciphers, and the second (**cryptoanalysis**) - is the study of breaking those ciphers. 

Modern cryptography divides encryption algorithms (ciphers) into two main categories: **symmetric** and **asymmetric** encryption (also known as public-key cryptography). Let's talk about them in detail. 

### Symmetric encryption

Before we move to the ciphers, let's define a few terms:
- **plaintext** - the message that we want to send to other counterparties in a secure way
- **a key** - a secret piece of text that is used for encryption and decryption
- **ciphertext** - non-readable text that is produced from applying cipher and secret key to the plaintext (original)
- **a cipher** - encryption algorithm for transforming the text to non-readable form (and back to readable)

![Project Structure]({{ site.baseurl }}/img/20220508/sym.png)

The essence of [symmetric encryption](https://en.wikipedia.org/wiki/Symmetric-key_algorithm) is that the same key is used for encrypting plaintext and decrypting it back. You can quickly get the initial message if you have a key, ciphertext, and you know the cipher.  

For example, one of the well-known symmetric ciphers is [**Caesar's cipher**](https://en.wikipedia.org/wiki/Caesar_cipher). The cipher was invented by the ancient Roman emperor - Julius Caesar. The essence of the algorithm is to use an alphabet with a slight offset from the start. The algorithm was so popular that it was considered "unbreakable" for a few centuries.  

![Project Structure]({{ site.baseurl }}/img/20220508/caesar.png)

In the modern world, of course, the letters in the alphabet do not move. Usually, they use the bitwise XOR operation (exclusive or) and a massive number of bit conversions, shuffles, and substitutions over many rounds.

**What is XOR (^)?**  

Briefly: if bits A and B are different, then A^B = 0. Otherwise, A^B = 1.
**Why XOR?**  

For example - 123 ^ 45 = 86. (where let 123 be the source text and 45 be the key). Then 56 is the ciphertext. And if you do this: 86 ^ 45 = 123 (original text!).  

Symmetric ciphers also can be divided into two groups:

- **stream ciphers** (encrypt each digit or letter independently as they appear in the text)
- **block ciphers** (take several bits and encrypt them as one unit)

Examples of symmetric ciphers are: AES, 3DES, RC2, Blowfish, RC4, etc.

### Perfect symmetric encription

The main drawback of symmetric encryption is that the same key is used for encrypting and decrypting the message. If an attacker collects many ciphertexts encrypted by the same key, the chances of breaking the cipher - will increase.  

As a countermeasure to guessing the key and breaking the code, the WWII military used a particular thing - **a one-time pad.** 

![Project Structure]({{ site.baseurl }}/img/20220508/otp.png)

It is a technique that allows encrypting each message with a new key (for a relatively long time). Before exchanging the messages, you need to prepare a big pad with pseudo-randomly generated letters or digits. It might be a person who gets a random letter from a pile of letters. Then those big "randomly" generated pads should be securely distributed between the army divisions. If any of such pads are lost - it can compromise communication.  

This approach is not easy to break. But the main drawback is that you need to prepare and distribute a lot of such pads. And you need to do it constantly - because the communication during the war is intense. 

So the approach is reasonable but not practical enough.

### Asymmetric encryption

Let's return to the **"message and box" analogy** from the beginning of the post. 

You have the message, the box, and the lock with keys. But this time, the keys are a little bit different. One key can be used only for locking the box. The second one - is only for opening the lock.

So to send a secure message, you put it in the box, lock it using locking key and send it to the friend. Your friend then opens the lock using the second key and reads the message.

This is the essence of **[asymmetric encryption](https://en.wikipedia.org/wiki/Public-key_cryptography)**: rather than using one key on both sides of the communication channel, we use one key (**public** key) for **encrypting** and the other (**private** key) for **decrypting** the message.  

![Project Structure]({{ site.baseurl }}/img/20220508/asym.png)

The idea of ​​asymmetric encryption is based on [**one-way functions**](https://en.wikipedia.org/wiki/One-way_function). These functions are easy to apply to a number, but it is complicated to calculate the original number from the result. This idea came to the mind of two scientists, Diffie and Hellman, in the 1970s. Together, they came up with the first asymmetric encryption algorithm.

For example, taking two large prime numbers and multiplying one number by another is a straightforward task. And now try knowing the result - to calculate (or instead pick up) the original two numbers? This is already a difficult challenge.  

### What are the pros and cons of asymmetric ciphers?

**Pros:** The key exchange can be carried out even over an open communication channel. An attacker, knowing the public key, will not be able to decrypt the message.

**Cons:** With asymmetric encryption for communication, both parties need to exchange public keys. Plus, the speed of encryption and decryption is more significant than in symmetric ciphers.

### What are the examples of asymmetric algorithms?

- [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)
- [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) - based on the difficulty of factoring large prime numbers
- [Digital Signature Algorithm (DSA)](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm) - is based on the complexity of computing discrete logarithms.
- [Elliptic Curve Digital Signature Algorithm (ECDSA)](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) - is also an algorithm for calculating discrete logarithms, but already in a group of points on an elliptic curve. ECDSA is used for the electronic signature of transactions in blockchain systems.

### Where encryption is used in the modern world?

- [**HTTPS (SSL/TLS)**](https://en.wikipedia.org/wiki/Transport_Layer_Security) - with asymmetric encryption, two parties construct and exchange a key, which is then used to symmetrically encrypt messages.
- **Digital Signatures**- checking the integrity and authenticity of data.
- [**PGP**](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) - a set of utilities for encryption
- [**S/MIME**](https://en.wikipedia.org/wiki/S/MIME) - standard for encryption and signature in e-mail
- **Blockchain**. When you create an account in almost any wallet for any blockchain, you generate a pair of private and public keys. Public addresses are subsequently generated (to which cryptocurrency or tokens are sent to you) based on public keys.

### How to test the encryption algorithms?

Symmetric encryption algorithms are compared in terms of the number of rounds, the length of the key and block, and the complexity of transformation and implementation. Their avalanche effect is also compared.
The primary indicator here is the cryptographic strength of the algorithm (how hackable it is). A cipher can be substantial if it takes so much time and resources to break it that the information becomes irrelevant after breaking it. 

The whole protection lies in the fact that it will take an attacker a lot of time and computing resources to brute-force iterate through all possible values of the key.

Therefore, the longer the key, the less vulnerable the encryption is to hacking. Even the RSA algorithm with a key of 1024 bits is no longer considered reliable enough in modern systems. It is better to take 4096 bits.

Another way how to compare encryption algorithms - is by their speed.  

### Conclusions
Encryption and cryptography is an incredibly vast topic to cover in one blog poost and even in one book. If you really interested in diving into the story of the ciphers in human history - I can recommend a great book by [Simon Singh - "The Code Book".](https://www.amazon.com/Code-Book-Science-Secrecy-Cryptography/dp/0385495323)

In the next posts, we will learn how public key cryptography is used in digital signatures and blockchain. Together we will see how private and public keys are generated in cryptowallets.
Also - we will see how to use commandline tools for generating private and public keys, sign messages and manage multiple encryption keys. 





