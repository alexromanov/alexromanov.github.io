---
layout: post
title:  "The one where scenario is run from feature file"
date:   2016-11-07 10:09:45 +0300
author: "Oleksandr Romanov"
description: "In the following post I will share a quick tip on how to set up Idea to run scenario from feature file"

---

_In the following post I will share a quick tip on how to set up Idea to run scenario from feature file_  

Intellij IDEA 2016.2 CE is used as IDE

**Target audience:** QA Automation engineers / developers

DISCLAIMER: It is assumed that reader has some experience with [Cucumber][cucumber-site] test tool and Cucumber for Java plugin is installed for Intellij Idea.  

### Problem: Why do we need configurations?  

Cucumber features and separate tests can be run using custom runner classes. In this classes you can set up various configurations, such as: features and tags to run, output formatters, etc.  

For example:  

{% highlight scala %}
 package steps  

 import cucumber.api.CucumberOptions  
 import cucumber.api.junit.Cucumber  
 import org.junit.runner.RunWith  

 @RunWith(classOf[Cucumber])  
 @CucumberOptions(  
  features = Array("classpath:features"),  
  glue = Array("classpath:steps"),  
  tags = Array("@wip"),  
  monochrome = true,  
  plugin = Array("pretty",  
   "html:target/cucumber",  
   "json:target/cucumber/test-report.json",  
   "junit:target/cucumber/test-report.xml")  
 )  
 class TestRunner {}  
{% endhighlight %}

But at some point you may want to have possibility to run feature or single scenario just within feature file.  
And if you test project has a non trivial structure, Cucumber may not see package with Step Definitions - as a result scenario will not be executed and signals that step definitions were not found.  

Below you will a find a tip on how to handle such situation.  

### Solution: How to set up default configuration for Cucumber?

Just follow the next steps:  
 - Open Edit and Configurations window

![Project Structure]({{ site.baseurl }}/img/20161107/edit_configuration.png)

 - Open list of default configurations

![Project Structure]({{ site.baseurl }}/img/20161107/configuration_list.png)

 - Find Cucumber for Java configuration
 - Set main class - e.g. cucumber.api.cli.Main
 - Fill glue section with name of package, where step definitions located
 - Set VM options and classpath if it is needed

![Project Structure]({{ site.baseurl }}/img/20161107/filled_configuration.png)

 - Save default configuration  
 - Run any scenario or feature just by right click on it's name and select "Run scenario" or "Run feature"  

**NOTE:** By default Cucumber will automatically generate program arguments and path to feature file each time, when you run any scenario or feature.  

### Conclusion
Default configuration can be very useful in day to day activities for running / debugging any single scenario or while refactoring step definition code.    

[cucumber-site]: https://cucumber.io/
