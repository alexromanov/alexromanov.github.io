---
layout: post
title:  "The one with unique requests to web application"
date:   2016-11-08 09:09:47 +0300
author: "Alexander Romanov"
description: "In the following post I will share a simple but useful script for generating requests to web application from various ip adresses"

---

_In the following post I will share a simple but useful script for generating requests to web application from various ip adresses_  

**Target audience:** QA Automation engineers / developers

DISCLAIMER: It is assumed that the reader has some experience with [Python][python-site] programming language.

### Problem: Large set of user requests need to be resolved by ip data?  

In our tester's life we are facing with an enormous amount of tasks every day. Part of these tasks - are really routine and boring. And as it is known - every routine task is done worse and worse as time goes and can be completely ignored at the end.  

The best solution for such situation is using automation in test activities. It will make life more easy and free time to do something interesting (e.g. exploratory testing :)).  

For scripting automated tasks I personally prefer Python programming language. Python is very simple to learn and it allows to create and maintain small - medium scripts even without using any cumbersome IDE. Just use [Sublime Text][sublime-site] and go. 

So here it is the case.  
After getting a new build of web application, the tester should test (fast and reliably) - is system correctly recognize geo ip data of the user (city, country, etc) by his/her IP - address.  

User case scenario can be described as:  
 - the user open desired page at web application (it's assumed that user have unique IP - address);
 - The web app should determine user data based on it's geoip information and save it in some database.  

### Solution: 

The case is fairly simple as its obvious way to test it: just make a request (with modified X-Forwarded-For" header) to web application from unique ip addresses.  
For this purpose Postman extension for Chrome can be used or Fiddler tool. 

If pool of ip adresses to check is large - here when the routine attacks.  

Just a few lines of python code can do this monotonous task for us (save it at request_generator.py):   

{% highlight python %}
 import requests  
 import argparse  
 parser = argparse.ArgumentParser(description="""Generating requests from various ip addresses.""")  
 parser.add_argument('-file', type=str, help='input file with ip adresses range')  
 parser.add_argument('-host', type=str, help='target host')  
 args = parser.parse_args()  
 inpfile = open(str(args.file), 'r')  
 for ip in inpfile.readlines():  
      headers = {'X-Forwarded-For':ip}  
      requests.get(args.host, headers=headers)   
{% endhighlight %}

Script can be run by the following command:  

{% highlight python %}
request_generator.py -file ipadresses.txt -host http://yourhostname.com  
{% endhighlight %}
where - ipadresses.txt - file with a list of unique adresses to test.

### Conclusion
Python scripts can speed up a lot of testing activities and reduce boring and regression work. Sometimes - just in a couple of lines of code.     

[python-site]: https://www.python.org/
[sublime-site]: https://www.sublimetext.com/
