---
layout: post
title:  "Automation In Scala: Publish Allure reports with Github Actions"
date:   2021-07-19 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210719/notification.jpg"
description: "Guide how to publish reports to Allure server and notify users"
summary: "Guide how to publish reports to Allure server and notify users"
header-img: "img/20210719/notification.jpg"

---

Photo by <a href="https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Markus Spiske</a> on <a href="https://unsplash.com/s/photos/report?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

In one of the [previous blog posts](https://alexromanov.github.io/2021/05/17/scala-allure-report/), I showed how to add Allure reporting to the Scala-based automation project.  

But generating reports only on the local machine is not enough. The main goal of test reports is to notify the team and the management about the issues found by automated tests.  

That's why it is essential to share the reports using some notification channels. The channels can be various: emails, Slack or Skype notifications, etc.  

In this blog post, I will continue my talk about Allure reporting: we will generate reports, push them to the server and notify users about test run results via email.  

## Build and test with Github Actions

[Github Actions](https://github.com/features/actions) is a set of predefined steps for automating CICD workflow for the project. It is defined in simple YAML format.  

Action can have one or more triggers: on push, pull request for a particular branch. Special triggers allow to run jobs manually (workflow_dispatch) or after getting a special event from another repository (repository dispatch).  

In our case, we will add a trigger for running workflow by manual execution.
Just add .github/workflows folder structure at the root of the project and add a new .yml file with the description of the flow:

``` yaml

name: Execute Test Run

on:
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Set up JDK
        uses: actions/setup-java@v1.4.3
        with:
          java-version: '11.0.8'

      - name: Compile
        run: sbt compile

      - name: Test
        run:  sbt "testOnly * -- -n Smoke"
```

Here we define test job for our project that will be executed on ubuntu build machine:

* it will perform checkout for branch
* then, it will add JDK to build machine
* then it will compile the project and execute the tests

As we use [sbt](https://www.scala-sbt.org/) as a build tool, then we run test job with filter for specific tests

## Allure server setup

[Allure server](https://github.com/kochetkov-ma/allure-server) is an open-source solution for hosting test reports.  

You can download the project from GitHub and execute it on the local machine, but the best case is to have a dedicated server for reports - e.g., some instance in the cloud.

For configuration, you need to download [Docker image](https://hub.docker.com/r/kochetkovma/allure-server) and start the server:

``` bash

docker pull kochetkovma/allure-server:latest
docker run -p 8080:8080 kochetkovma/allure-server:latest

```

After you start the server, it will be available at http://127.0.0.1:8080.  

![Project Structure]({{ site.baseurl }}/img/20210719/allure-server.png)

Ensure that the Allure server is accessible for calls from Github, as we will call server API for saving reports.  

## Reports publishing  

We will use [github action from @Xotabu4](https://github.com/Xotabu4/send-to-allure-server-action)

![Project Structure]({{ site.baseurl }}/img/20210719/publish-report.png)

Under the hood, this Github action will make an archive from the allure-results folder and store it at the Allure server.  
As a result, this action will return the URL to generated report, stored on the server.  

If you want to use the report URL in further steps (e.g., notification), please add an id property to the action with some name.  

## Email notification

Email notification is executed via [action-send-mail](https://github.com/dawidd6/action-send-mail).  

Here is an example of action configuration:

![Project Structure]({{ site.baseurl }}/img/20210719/send-email.png)

**Few notes:**

1. Username and password for connecting to Gmail should be added as SECRETS for repository

2. As email action connects to Gmail for sending emails - make sure that [security policies allow](https://alexromanov.github.io/2021/05/04/scala-mail-testing/) to do it for external resources

3. You can use Allure report URL from the previous action as steps.allure-report.outputs.report-URL

4. Date variable from Get-Date step is used as steps.date.outputs.date

5. Recipients are configured as a comma-separated list

6. Subject of the email notification will include date and time of the test run. In order to get it, we will use special action:

![Project Structure]({{ site.baseurl }}/img/20210719/get-date.png)

## Executing action  

Navigate to Your repo on Github / Actions tab, select "Execute Test Run" workflow and click Run Workflow.

As a result, you will get an email with a link to the Allure report (saved on the Allure server).

## Conclusions  

As you can see, Github Actions allow the creation of automated testing workflow in less than 50 lines of code.  

If your end-to-end tests are a part of the solution - add these actions into the workflow.  

If your tests are stored in a separate project - you can use [repository_dispatch](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#repository_dispatch) for subscribing to events from other repositories in the build pipeline.

You can find the source code of the action at the [repository](https://github.com/alexromanov/scala-automation-samples/blob/main/.github/workflows/manualtestrun.yml)
