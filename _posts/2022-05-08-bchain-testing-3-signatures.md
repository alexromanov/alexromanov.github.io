---
layout: post
title:  "Blockchain for Test Engineers: Digital Signatures"
date:   2022-05-15 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220515/signature.jpg"
description: "Digital signatures from tester's point of view"
summary: "Digital signatures from tester's point of view"
header-img: "img/20220515/signature.jpg"

---

Photo by [David Nitschke on Unsplash](https://unsplash.com/photos/pegxjW_1YOU?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

This is the third blog post in the [**"Blockchain for Test Engineers"**](https://alexromanov.github.io/2022/04/24/blockchain-testing-mindmap/) series.  

In the previous posts we talked about **[hashing](https://alexromanov.github.io/2022/05/01/bchain-testing-1-hashing/)** and **[encryption](https://alexromanov.github.io/2022/05/08/bchain-testing-2-encryption/)**. Today I will show you what is digital signature and how it is used in the blockchain world.

### The purpose of digital signatures
Last week we learned some methods for hiding the transmitted information from third parties. It is - encryption (symmetric or asymmetric).

But even if the message is encrypted, other problems need to be solved:
1. How can the recipient be sure that the sender sent the message?
2. How can the recipient and sender ensure that the message has not been modified during transmission?  

Both of these tasks are solved using the mechanism of digital signatures (digital signatures).

### What is digital signature
In a real world, if you make a contract deal - both parties need to make a signature on the documents. It will prove, that you have read the documents and agreed about the content. In the the digital world it is a bit different. 

A **[digital signature](https://en.wikipedia.org/wiki/Digital_signature)** - is a special mathematical algorithm that allows checking the authenticity and integrity of messages.  

Digital signatures are built based on symmetric and asymmetric encryption. Asymmetric one is used more widely.  

The digital signature scheme was first described by the creators of asymmetric encryption - Whitfield Diffie and Martin Hellman, in 1976. RSA algorithm can be used for creating a primitive version of digital signatures.  

Digital signatures (as a scheme) can be a part of electronic signatures. Electronic signatures are equivalent to the real ones and are valid in many countries in the world. Electronic signatures allow you to reliably sign documents in case if can't be physically present during the deal. 
There are a lot of services, that provide electronic signatures - e.g. DocuSign, HelloSign, etc.

### How does a digital signature work?

![Project Structure]({{ site.baseurl }}/img/20220515/howsignworks.png)

In the simplest form, the digital signature of a message is a combination of encryption and hashing:
 1. The sender calculates the hash of the message;
 2. The document's hash is encrypted using the sender's private key;
 3. The document + encrypted hash is sent to the recipient;
 4. The recipient calculates the hash of the message using the same hash function;
 5. The recipient decrypts the signature using the sender's public key and receives the hash from the sender.
 6. The recipient compares the sender's hash and the hash calculated from the message. If they are the same - signature is correct. 

There are a lot of different algorithms for digital signatures.  
Some are - **[RSA-PSS](https://en.wikipedia.org/wiki/RSA_(algorithm))**, **[DSA](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm)**, **[ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)**, **[Rabin signature](https://en.wikipedia.org/wiki/Rabin_signature_algorithm)**, and **[Schnorr signature](https://en.wikipedia.org/wiki/Schnorr_signature)**. Each algorithm has its own strengths and weaknesses. 

### Usage in blockchain

![Project Structure]({{ site.baseurl }}/img/20220515/signblockchain.png)

Every time you make a new transaction in the wallet, "under the hood," the transaction is signed with your key. Then, in the blockchain, you can check who created one or the other transaction.  
Additionally, each new block is signed by a miner of this block.  

So everybody in the blockchain can check and prove that particular transaction was sent by a user with the correct private key.

But what about well-known blockchains?

**Bitcoin** initially used the **[ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)** algorithm for signing transactions. A couple of years ago, this algorithm was replaced, and now transactions can be signed using the **[Schnorr signature](https://en.wikipedia.org/wiki/Schnorr_signature)**. The algorithm itself is fascinating - I will tell you more about it in the following notes.  

**Ethereum** currently uses **[ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)** and **Cardano** uses **[Ed25519](https://en.wikipedia.org/wiki/Curve25519)**.

### Attacks on digital signatures
How can digital signatures be compomised:
 - **Forgery of a document** (collision of the first kind) - an attempt to select a document for a specific hash. It is doubtful - because documents are usually large in size.
 - **Receiving two documents with the same digital signature** (collision of the second kind).
 - **Social attacks**. Based on the manipulation of the keys, stealing the private key, forcing the sender to sign the wrong document, and replacing the sender's public key.

### Tools
It is good thing to discover a new concept. But completely other thing - to try it practically.  

As a part of this blog post, I want to tell about tools, that I use personally for digital signatures in my day-to-day work.

#### gpg

The open encryption standard [OpenPGP](https://www.openpgp.org/) exists in two implementations [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) and [GPG (GnuPG)](https://en.wikipedia.org/wiki/GNU_Privacy_Guard). If the first one is closed, then any user can install the second one on his computer (both in Unix and in Windows).
Installation of the utility is described *[here](https://gpgtools.org/)*.

Here is the cheat sheet of basic commands:
 - generate keys (public and private): `gpg --gen-key`
 - see which keys are already imported: `gpg --list-keys` (also - `gpg --list-public-keys` and `gpg --list-secret-keys`)
 - import your keys from external sources: `gpg --import public_key.txt`
 - export your keys (to a file): `gpg --export -a "email" > pub.asc` (or `gpg --export-secret-key -a "email" > priv.asc`). Without redirection to a file - you can simply output the text of the key to the console.
 - upload your keys to public key servers for distribution: `gpg --keyserver pgp.mit.edu --send-keys KEY_ID`
 - encrypt and sign a message for the given recipient: `gpg --encrypt --sign --armor -r samplerecipient@mail.com message.txt`
 - decrypt data: `gpg --decrypt message.txt.gpg`
 - put a digital signature on the message: `gpg --detach-sign message.txt`
 - verify the authenticity of the signature: `gpg --verify message.txt.sig`


#### Mailvelope

![Project Structure]({{ site.baseurl }}/img/20220515/mailvelope.png)

If you don't like the command line, other more user-friendly tools are available. One example is [Mailvelope](https://mailvelope.com/en).  

You can install the Mailvelope plugin for Google Chrome (also available for Firefox and Edge). Using plugin, you can do the same thing as with gpg - but even more easy. You can read about how to install and configure this plugin [here](https://mailvelope.com/en/help#configuration).

I personally can say that this plugin is very comfortable. Its paid version integrates with **Google Gmail** - and allows you to encrypt and sign emails (and automatically decrypt them) without switching between third-party tabs and windows.

### Conclusions
Digital signatures is a very useful and powerful concept. It helps to sign documents anywhere in the world. It helps to check messages and provide secure communication. It also helps to check the validity of transactions and blocks in the blockchains. 

And another interesting thing.  
Did you notice, how the new knowledge is built on top of the previous concepts? Without knowing about **hashing** and **encryption** it is hard to understand how **digital signatures** works.  
That's why it is good thing to learn new things and connect it to already known parts. You can build **your own knowledge maps**. 





