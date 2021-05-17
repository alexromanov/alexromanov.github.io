---
layout: post
title:  "Automation In Scala: Allure Reporting"
date:   2021-05-17 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210517/allure-report.jpg"
description: "How to add Allure reports to Scala tests"
summary: "How to add Allure reports to Scala tests"
header-img: "img/20210517/allure-report.jpg"

---

Photo by <a href="https://unsplash.com/@isaacmsmith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Isaac Smith</a> on <a href="https://unsplash.com/s/photos/report?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  
## Reporting in automation
 
In the [previous blog post][ui-tests], I shared the basic examples of UI tests in Scala. Now it's time to get to reporting. 
 
Implementation and maintenance of the automated tests is a hard thing. But if the test results are not presented in a simple and understandable way - the value from automation tests can be dramatically decreased.  
 
Reports are a huge thing not only for managers. Test report can be used by the whole team. Good report can show which areas have been tested, which user stories and features are covered and what are the reasons for failure in a particular case. 
 
[Allure][Allure] reporting is a widely used open-source library for test reporting. It allows you in a matter of a few clicks to get a full featured report that contains a lot of useful data. 
Allure also can be customized a bit in case you want some additional information displayed in the reports.  
 
There plenty of [examples][examples] on how to integrate Allure reports to test automation projects: in Java, Python, JS and other languages.  
 
Today I will show you how to integrate it to your Scala based solution.  
 
## Adding Allure to Scala test project
 
**For test project I am using [Scala][Scala] 2.13.5 and [sbt][sbt] 1.4.9**
 
* Add necessary libraries to build.sbt
 
```
    val allureScalaTestVersion = "2.13.3"
 
    resolvers +=
        "Sonatype OSS Snapshots" at "https://oss.sonatype.org/content/repositories/snapshots"
    
    libraryDependencies += "io.qameta.allure" % "allure-scalatest_2.13" % allureScalaTestVersion % Test
 
    testOptions in Test ++= Seq(
        Tests.Argument(TestFrameworks.ScalaTest, "-oD"),
        Tests.Argument(TestFrameworks.ScalaTest, "-C", "io.qameta.allure.scalatest.AllureScalatest")
        )
```
 
* Modify the test classes and add a **new AllureScalatestcontext** for each test
 
``` scala
  "User" should "be able to view latest blog post" in new AllureScalatestContext {
    val homePage = new HomePage
    go to homePage
    val posts = findAll(CssSelectorQuery("div.post-preview")).toList
    posts should have size 5
    val latestPost = posts.head
    latestPost.text should not be empty
  }
```
 
* Run tests via **"sbt test"** command  
 
    As a result - you  should get test results from test run inside **allure-results** folder. 
 
* If you want to generate reports locally - install [Allure command line][cmd] tool  
 
* For generating report, execute:
 
``` bash
    $ allure generate
    Report successfully generated to allure-report
````    
 
* For opening generated report in a default browser - use:  
 
``` bash
    $ allure open
    Starting web server...
    2021-05-17 14:50:08.203:INFO::main: Logging initialized @258ms to org.eclipse.jetty.util.log.StdErrLog
    Server started at <http://172.20.160.1:52209/>. Press <Ctrl+C> to exit
```   
 
* In the end you get the report:
 
    ![Project Structure]({{ site.baseurl }}/img/20210517/report-example1.png)
 
    ![Project Structure]({{ site.baseurl }}/img/20210517/report-example.png)
 
## Conclusion
 
Allure reporting is a great tool for visualizing test results and trends. It is easy to integrate and use. 
 
In most cases it is more than needed for status reports.  Of course, if you need a completely different report, you can customize HTML sources.  
 
The next logical step is to add Allure report to your CI.  
 
Full code samples can be found in [repository][source repo].  
 
[allure]: https://docs.qameta.io/allure/
[examples]: https://github.com/allure-examples
[source repo]: https://github.com/alexromanov/scala-automation-samples
[cmd]: https://docs.qameta.io/allure/#_commandline
[sbt]: https://www.scala-sbt.org/
[Scala]: https://www.scala-lang.org/
[ui-tests]: https://alexromanov.github.io/2021/05/11/selenium-scala-basic/
