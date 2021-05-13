---
layout: post
title:  "Test Automation In Scala: Basic Selenium Tests"
date:   2021-05-12 00:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210512/selenium-scala.jpg"
description: "How to write basic Selenium tests using ScalaTest"
summary: "How to write basic Selenium tests using ScalaTest"
header-img: "img/20210512/selenium-scala.jpg"

---

## Scala in test automation

[Scala][Scala] programming language provides a good mix between functional and object-oriented programming. It is used in projects, where pure functions and functional paradigm is crucial. 

But how Selenium tests look like in Scala?

## ScalaTest

The most widespread testing framework in Scala is [ScalaTest][Scalatest]. It provides a lot of functionality for writing unit and integration tests.

## Implementation

As for experiment, we will write a couple of UI tests for current blog.  
In order to implement first basic tests you can follow the steps: 

* Add necessary libraries to [build.sbt][sbt]

```
    libraryDependencies += "org.scalatestplus" %% "selenium-3-141" % "3.2.7.0" % "test"
    libraryDependencies += "org.scalatest" %% "scalatest-flatspec" % "3.2.7" % "test"
    libraryDependencies += "org.scalatest" %% "scalatest-shouldmatchers" % "3.2.7" % "test"
    libraryDependencies += "com.google.guava" % "guava" % "30.1.1-jre"

```

* Add pages objects. In our case it will be all main pages for the blog: Home Page, Contact Page, About Page.

``` scala
    class HomePage(implicit val driver: ChromeDriver) extends Page {
        val url = "https://alexromanov.github.io/"

        val homePageLink: String = "//a[@href=\"/\"]"
        val aboutPageLink: String = "//a[@href=\"/about/\"]"
        val contactPageLink: String = "//a[@href=\"/contact/\"]"
    }
```

If you want to have an access to "go to page" functionality in tests, you can extend your page class from Page trait and add url variable. 

* Add class with tests

``` scala
    class BlogTest extends AnyFlatSpec with BeforeAndAfterAll with Matchers with Eventually with WebBrowser {

        implicit val webDriver: ChromeDriver = {
            System.setProperty("webdriver.chrome.driver", "C:\\tools\\chromedriver_win32\\chromedriver.exe")
            new ChromeDriver()
        }

        "User" should "be able to view latest blog post" in {
            val homePage = new HomePage
            go to homePage

            val posts = findAll(CssSelectorQuery("div.post-preview")).toList
            posts should have size 5

            val latestPost = posts.head
            latestPost.text should not be empty
        }

        "User" should "be able to open Contact Page" in {
            val homePage = new HomePage
            go to homePage
            homePage.url should equal (currentUrl)

            click on xpath(homePage.contactPageLink)
            eventually {
                currentUrl != homePage.url
            }

            val contactPage = new ContactPage
            contactPage.url should equal (currentUrl)
        }

        "User" should "be able to send message to author" in {
            val contactPage = new ContactPage
            go to contactPage
            contactPage.url should be (currentUrl)
            
            textField(contactPage.nameField).value = "Reader"

            click on id(contactPage.emailField)
            enter("reader@mail.com")

            click on id(contactPage.phoneField)
            enter( "123456789")

            textArea(contactPage.messageField).value = "Hello Author"

            click on cssSelector(contactPage.sendButton)
        }

        override def afterAll(): Unit = {
            quit()
        }
    }
```

For tests we use AnyFlatSpec and [Should Matchers][Matchers]. It allows us to write assertions in a readable format with Should keyword.  

[ScalaTest-Selenium][ScalaTest-Selenium] provides a very nice wrapper against WebDriver. You just need to add WebBrowser trait. As a result, you will have methods such as "click on id()", "enter()", "close()", etc.  

It also defines Webdriver instance as implicit value - so you don't need to pass driver whenever it is used (in tests or pages).  

For adding an additional before and after logic we need to add BeforeAndAfterAll trait and override desired methods. In our case, afterAll() method is overriden - it will quit driver after tests are finished.

## Conclusion

As you can see, UI tests in Scala are very close to human readable format with using Should matchers and scalatest-selenium traits. 

ScalaTest-Selenium provides nice wrapper of WebDriver, but it is not extensible from the first sight. Maybe, it will require a complete framework re-write.  

But for simple UI tests it is more than enough.  

In the next articles we will see even more interesting use cases of using Scala in test automation (also with Selenium)  

Full code samples can be found at src/test/scala/ui package in [code samples][source repo].  

[Scala]: https://www.scala-lang.org/
[ScalaTest-Selenium]: https://www.scalatest.org/plus/selenium
[source repo]: https://github.com/alexromanov/scala-automation-samples
[ScalaTest]: https://www.scalatest.org/
[Matchers]: https://www.scalatest.org/user_guide/using_matchers
[sbt]: https://www.scala-sbt.org/
