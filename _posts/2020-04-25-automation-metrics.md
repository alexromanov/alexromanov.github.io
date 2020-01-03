---
layout: post
title:  "Measuring test automation"
date:   2020-04-25 00:00:47 +0300
author: "Oleksandr Romanov"
description: "How to make test automation effort more goal-oriented via metrics"
header-img: "img/20200425/metrics.jpg"

---

## The road to automation

If the development team of any software product wants to release changes faster, they have only a few choices. One choice is to cut the testing scope. But it may lead to bugs in production.  
The other choice is to use test automation, at least for the regression part.  

Well-written automation tests provide fast feedback in case of regression issues. Also, as authors of the book ["Software Engineering at Google: Lessons Learned from Programming Over Time"][SEAG] state, the tests can decrease the developer's fear of refactoring existing code.  

The Internet is full of tutorials on automation. It is easy to get knowledge about the "test automation pyramid" and how it should be implemented in an "ideal" world.  

It is even easier to learn how to implement basic UI tests using Selenium Web Driver. Then it is only a matter of time to start furiously automate all possible scenarios for an application.  

But when the number of automated tests is about hundreds and more - new questions start to emerge.  

It's time to think about the following questions:  

- How many automated tests do we have now and how many we need to add?

- What is an automated test coverage?

- How much time do we spend on manual testing and how much time do we save by using automation?

- Is test automation effective enough?

In this blog post, I will tell how to answer such questions by using proper automation metrics.  

**Disclaimer:** The metrics, which I'll describe, are not the standard. Consider the context of the project before starting to collect and analyze any metrics.

## Smallest possible coverage

![Project Structure]({{ site.baseurl }}/img/20200425/unitcoverage.jpg)

Unit test coverage is a basic metric and can be collected by multiple tools. One of the well-known tools is a [SonarQube][SQ]. SonarQube provides not only code coverage (lines of code) but also scans the source code in terms of security vulnerabilities.  

The common practice is using SonarQube as a part of automatic verification for the code changes. SonarQube measures unit test coverage for each pull request compares it to the defined threshold and blocks the merge process in case if the coverage is lower than the threshold value.  

Such practice helps to keep code in a healthy state from change to change. The threshold is typically set to 80 - 85% because it is time-consuming to demand 100% code coverage for each code change. Of course, there are some teams, which can hold 100% code coverage, but it completely depends on the circumstances.  

For [Spring Boot][SB] based services SonarQube deals with any type of tests as with unit tests. So, as a result, it is not a trivial task to get more granular information about coverage for different test levels, such as integration, component, contract, etc.  

Unit testing provides information only about which lines of code have been executed. **But what about the behaviors of the system?**  

## Connecting automated and manual test cases

Typically automated and manual test cases are stored in different systems. For example, we use the [Test Rail][TestRail] as a test case management system.  

We have a one-to-one or one-to-many relationships between the automated test and test case. When automated tests finished execution, the results are automatically populated to the particular test run for a given regression cycle in Test Rail.

If the automated test failed - it marked with a "ReTest" status and should be re-checked by the Manual QA. After the checking status of the test can by either "Passed" or "Failed".  

If the test failed in the automatic run but passed manually - it is a false negative result. The overall goal is to keep such tests to a minimum.  

If the test failed automatically and failed after manual re-check - it shows that automated tests reveal a potential bug.  

## What to measure

![Project Structure]({{ site.baseurl }}/img/20200425/measure.jpg)

### Success Rate of automated tests  

   No matter how much is the coverage, the first definition of any automated test is how stable they are. The goal is to set the success rate to some threshold and keep it stable from release to release. Pay attention, that in the case of End-To-End UI tests it can be hard or **[even impossible][flaky]** to keep success rate to 100%.  

   ***SR = (Automated Tests Passed / Total Number Of Automated Tests Executed) * 100%***  

### Automated test coverage

   The basic metric which determines the progress of automation. It shows how much tests out of the all possible are in fact automated.  

   ***Automated Coverage = ( Total Tests with "Automated" status / Total Number Of Tests ) * 100%***
  
### Actual automated test coverage

   Tracking just tests in "Automated" status doesn't say what happens with automation in reality. So it is better to calculate the automated test coverage tied to the success rate.

   ***Actual Automated Coverage = ( Automated Tests Passed / Total Number Of Tests ) * 100%***  

### Time spent on automated tests and manual test in each regression tests

   For tracking time spent on the test, we have a special field in TestRail - "Estimated". It shows, how much time takes by manually executing the test case (with all its preconditions and post-conditions).

   Time spent on manual testing:  

   ***The sum of all "Estimated" field from tests marked as "Manual" and automated tests, which marked as "ReTest"***  

   Time saved by automation testing:  

   ***The sum of all "Estimated" field from tests marked as "Automated"***  

### Defects found by automation

   Finding defects is not a primary goal of automation testing. But in order to have visibility on the prevented defects, we can track the number of automated tests, which falls from "ReTest" to "Failed" status after manual verification.  

## When to measure

All automated test metrics, which I described in the previous section should be collected during each regression cycle.

Then, for better visibility, it is possible to split it by the type of testing (Smoke, Full Regression), by the platform (Web, Mobile, Desktop), by the browser and device type.  

As I am using Google Sheets for collecting data and visualizing graphs, I found it very useful sometimes to apply [Google Sheets Explore][Explore] function provided by the sheets itself. It can analyze your data and then suggest graphs based on the found correlations.  

## Conclusion

Automation metrics provide a clear picture of the value provided by automation and the stability of tests and infrastructure.  
It is possible to set goals (in terms of coverage, execution time, stability) and then track the progress of achieving it.  

Which automation metrics do you use?

[SQ]: https://www.sonarqube.org/
[SEAG]: https://www.amazon.com/Software-Engineering-Google-Lessons-Programming/dp/1492082791
[SB]: https://spring.io/projects/spring-boot
[TestRail]: https://www.gurock.com/testrail/
[Explore]: https://support.google.com/docs/answer/6280499?co=GENIE.Platform%3DDesktop&hl=en
[flaky]: https://testing.googleblog.com/2017/04/where-do-our-flaky-tests-come-from.html
