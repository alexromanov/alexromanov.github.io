---
layout: post
title:  "How to test randomness?"
date:   2021-12-10 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20211210/random.jpg"
description: "How to test random number generators?"
summary: "How to test random number generators?"
header-img: "img/20211210/random.jpg"

---

Photo by <a href="https://unsplash.com/@dylan_nolte?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">dylan nolte</a> on <a href="https://unsplash.com/s/photos/lottery?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## Situation
Imagine: you come to an interview for the position of the test engineer. Then you are given an application that generates random numbers (e. g., Random.org). 

The interviewer asks a question: "how do you check that the application generates truly random numbers?"

## What is random number generation?
In general, random number generation is a process that uses a device (or algorithm) to generate a sequence of numbers or symbols. The sequence is random if it is impossible to predict values ​​until generation.
A classic example is a coin toss or a shuffle of playing cards.

## How do random number generators get?
In addition to random number generators, there are also **pseudo-random number generators** (in which numbers only look random but are generated according to a strictly defined algorithm). Such an algorithm can be "hacked" - so someone can predict the numbers in the sequence.
You can get quite random values if you add a hardware **source of randomnes** to the pseudo-random number generator.

The source of randomness can be any physical process that is difficult to model at the level of knowledge.
Examples of such sources are clock status or equipment serial number, different noises from video or audio inputs, chaotic air turbulence when searching at HDD disks, mouse movements.

## So how do you properly test your random number generator?
The easiest way to test is to generate N numbers in a row and ensure they do not repeat (provided that the range of numbers is extensive enough).
But is such a test enough? Will test design techniques help you in this case?

In reality, it is not enough. Because even pseudo-random number generators can have a tremendous repetition rate and "seem" random at first glance.

Therefore, the following options exist:

* **Visual**. Based on the numbers, we make a large bitmap image and see any repetitions and patterns. E.g. here is the visual comparison of Randoom.org algorithm and PHP rand() function.  

![Project Structure]({{ site.baseurl }}/img/20211210/randomVisual.png)

* **Statistical tests of randomness** (used by [NIST](https://www.nist.gov/) - US National Institute of Standards and Technology). There are [fifteen such tests](https://csrc.nist.gov/projects/random-bit-generation/documentation-and-software/guide-to-the-statistical-tests). Among them are frequency tests, discrete Fourier transform, aperiodic tests, linear complexity tests. 

Statistical tests include:
- Frequency (Monobits) Test
- Test For Frequency Within A Block
- Runs Test
- Test For The Longest Run Of Ones In A Block
- Random Binary Matrix Rank Test
- Discrete Fourier Transform (Spectral) Test
- Non-Overlapping (Aperiodic) Template Matching Test
- Overlapping (Periodic) Template Matching Test
- Maurer's Universal Statistical Test
- Linear Complexity Test
- Serial Test
- Approximate Entropy Test
- Cumulative Sum (Cusum) Test
- Random Excursions Test
- Random Excursions Variant Test

Almost all the tests help find repeating patterns in a sequence of numbers or symbols.

How do **you** test randomness?

