---
layout: post
title:  "Automation In Scala: Publish Allure reports with Github Actions"
date:   2021-07-18 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20210718/notification.jpg"
description: "Guide how to publish reports to Allure server and notify users"
summary: "Guide how to publish reports to Allure server and notify users"
header-img: "img/20210718/notification.jpg"

---

Photo by <a href="https://unsplash.com/@prateekkatyal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Prateek Katyal</a> on <a href="https://unsplash.com/s/photos/notification?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

In the previous blog post, I showed how to add Allure reporting to the Scala-based automation project.  

But generating reports only on the local machine is not enough. The main goal of test reports is to notify the team and the management about the issues found by automated tests.  

That's why it is essential to share the reports using some notification channels. The channels can be various: emails, Slack or Skype notifications, etc.  

In this blog post, I will continue my talk about Allure reporting: we will generate reports, push them to the server and notify users about test results.  

## Add basic action for manual run  

[Github Actions](https://github.com/features/actions) is a set of predefined steps for automating CICD workflow for the project. It is defined in simple YAML format.  

Triggers can execute actions: on push, pull request for a particular branch. Special triggers allow to run jobs manually (workflow_dispatch) or after getting a special event from another repository (repository dispatch).  

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

## Set up Allure server  

[Allure server](https://github.com/kochetkov-ma/allure-server) is an open-source solution for hosting test reports.  

You can download the project from GitHub to a local machine, but the best case is configuring it on a dedicated server for reports.  

For configuration, you need to download [Docker image](https://hub.docker.com/r/kochetkovma/allure-server) and start the server:

``` bash

docker pull kochetkovma/allure-server:latest
docker run -p 8080:8080 kochetkovma/allure-server:latest

```

After you start the server, it will be available at http://127.0.0.1:8080.  

![Project Structure]({{ site.baseurl }}/img/20210718/allure-server.png)

Do not forget to open 8080 port (or any other configured port) to the world for Github Action.  

## Allure publish action  

We will use [github action from @Xotabu4](https://github.com/Xotabu4/send-to-allure-server-action)
Action will automatically archive the allure-results folder and send it to the Allure server.

![Project Structure]({{ site.baseurl }}/img/20210718/publish-report.png)


As a result, this action will return the URL to generated report, stored on the server.  
You can add an id property to action for further references.  

## Email notification action

If you want to add date and time to email notification, you can add an additional step for it:

![Project Structure]({{ site.baseurl }}/img/20210718/get-date.png)

Email notification is executed via [dedicated action](https://github.com/dawidd6/action-send-mail):  

![Project Structure]({{ site.baseurl }}/img/20210718/send-email.png)

Few notes:

1. Username and password for connecting to Gmail should be added as SECRETS for repository

2. Allure report url can be extracted from previous action as steps.allure-report.outputs.report-url

3. Date can be extracted as steps.date.outputs.date

4. List of recipients (to: property) should be configured as comma-separated

## Executing action  

Navigate to Your repo on Github / Actions tab, select "Execute Test Run" workflow and click Run Workflow.

As a result you will get an email with link to Allure report (saved on Allure server).

## Conclusions  

As you can see, Github Actions allow to automated automated testing workflow pretty easilty and in less then 50 lines of code.  

Source code of the action can be found in [repository](https://github.com/alexromanov/scala-automation-samples/blob/main/.github/workflows/manualtestrun.yml)
