---
layout: post
title:  "Book Review: Software Engineering At Google"
date:   2020-04-21 00:00:47 +0300
author: "Oleksandr Romanov"
description: "My insights after reading the latest book about engineering at Google"
header-img: "img/20200421/scale.jpg"

---

### Another book from Google

Eight years ago, [How Google Tests Software][HGTS] book was published. Since then, it became one of the most popular software testing books among a new generation of testers.  

In this book, a group of engineers, along with a well-known James A. Whittaker, shared their experiences and practices in testing applied at Google.  

Some people liked the book, some people don't - but anyway - the book was well written and contained plenty of useful information.

I liked that book, but in my opinion, it missed information about other topics of engineering - how to introduce new tools and processes, share knowledge, write unit tests, maintain the health of software and deal with flakiness.  

In March 2020, O`REILLY Media has published a new book - ["Software Engineering at Google: Lessons Learned from Programming Over Time"][SEAG]. The book was written by Titus Winters, Tom Manshreck and Hyrum Wright.  

I discovered this book by accident. The first thing that I did - I started to search for reviews. Reviews were a little bit controversial - the people hoped that book reveal magic things which can be applied anywhere, but in fact, the book was not about universally applicable tools or patterns.

Maybe because my current work is dedicated more to software engineering now and less to testing, I liked this new book a lot more, than the previous one.  

Below I will summarize my insights after reading the book (with a spirit of automation and quality of course).

### The structure

The book is divided into three main parts: culture, processes, and tools.

All articles, processes, and tools, described in the book, are written only as an experience of one particular company. It may be applicable to your situation, but in most cases - not. Not so many companies on the market, which operates on the same scale as Google. But anyway, just by reading about the challenges and how these challenges are analyzed and overcome, it helped me to think over challenges in our product and what I can do in order to solve it.  

### Culture

![Project Structure]({{ site.baseurl }}/img/20200421/culture.jpg)

1. When designing and implementing a system of any size, it is better to consider the following principles:  
   1. Time and Change. How long the code will be in production and how often it will be changed over time. This is the first thing to think about when starting any project: e.g. if the script for automation is needed only in one particular place and only once - there is no need to think about extensibility. But in case, if automation is used by many teams - each new change should applicable for a large amount of users.  

   2. Scale and Growth. How are you prepared to the growth of users of your system: how well an API and processes described, is it understandable by the end-users, etc. Is it operates efficiently under load?

   3. Trades and Costs. It is a completely golden rule for introduction of tools or processes for any team or organization. The costs can take various forms: time, money, etc.  

2. All new systems are developed not by lonely genius in a cave, but by the team of engineers. So the life or death of any product completely depends on how well you can deal with other people in order to achieve a certain goal. Always think about humility, trust, and respect in social interactions.

3. Psychological safety is a key thing to engage in learning something new.

4. Knowledge sharing can take many different forms, but whatever you choose - please track its effectiveness and usefulness.

5. Being a leader of engineers can be hard, especially for someone who used to be an individual contributor. But the best advice here is to stop measuring yourself by lines of code that you produce. Measure it by your team's overall success towards goals.

6. GSM (Goals-Signals-Metrics) framework is worth to consider when introducing any new metric to track.

### Processes

![Project Structure]({{ site.baseurl }}/img/20200421/process.jpg)

1. Apply style guides if you want to have consistency in your code: feature or automation one.

2. Think about the correctness, comprehension, and readability of the code during the code reviews.

3. Remember about the target audience while creating any type of documentation.

4. The sooner you get agreed about the test terminology and test sizes - the better.

5. Consider unit tests as a tool for eliminating the fear of refactoring.

6. Introducing the testing culture into the organization always takes time and effort.

7. Mocks are useful in unit tests but beware of overusing it. Think about fakes instead.

8. The flakiness of end-to-end UI tests is a pain in Google too :). It is a universal problem without a one-to-all solution.

### Tools

![Project Structure]({{ site.baseurl }}/img/20200421/tools.jpg)

In this section, the authors describe a set of tools, which used in many teams in the company. For each tool, there is a description of why there is a need for this tool and how it fulfills the need.

Tools vary from version control and build systems, to a large-scale CI system, from code review tool to static code analysis. Everyone will find something interesting here.  

Some concepts can applicable even without introducing something technical - it is more to a mindset of people, rather than to a particular tool.

### In the end

Every project and product is unique, but they have a lot in common: we all deal with people, we all deal with computers.

Overall the book provides the story of experiences of one particular company. The only difference is the scale. But for any software engineer, it can provide a lot of insights on day-to-day problems and how to at least start to move towards solving them. Nobody can predict - maybe your particular product will burst into the user base and will be massively scaled in a world. So it is better to be prepared for such challenges in advance.

[HGTS]: https://www.amazon.com/Google-Tests-Software-James-Whittaker-ebook/dp/B007MQLMF2/ref=sr_1_1?crid=22FTSHV1XPB61&dchild=1&keywords=how+google+tests+software&qid=1587469752&s=books&sprefix=how+google+test%2Cstripbooks-intl-ship%2C291&sr=1-1

[SEAG]: https://www.amazon.com/Software-Engineering-Google-Lessons-Programming/dp/1492082791
