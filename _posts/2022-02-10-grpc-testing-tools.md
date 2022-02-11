---
layout: post
title:  "Quick comparison of gRPC testing tools"
date:   2022-02-10 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220210/tools.jpg"
description: "Which tool to choose for testing gRPC services?"
summary: "Which tool to choose for testing gRPC services?"
header-img: "img/20220210/tools.jpg"

---

Photo by <a href="https://unsplash.com/@carlevarino?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Cesar Carlevarino Aragon</a> on <a href="https://unsplash.com/s/photos/tools?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

In [one of my previous posts](https://alexromanov.github.io/2021/06/12/scala-grpc-api-tests/), I have shared how to implement automation tests for testing gRPC services. Automation is, of course, a fantastic thing.  

Still, there are a lot of cases where it is more suitable to verify new functionality quickly and manually rather than testing writing the code. 

Not so long ago, I discovered that [Postman has added gRPC support](https://blog.postman.com/postman-now-supports-grpc/). 
As of now, the functionality is in "beta." So after trying it by myself, I started to seek alternative solutions on the market. 

I will talk about GUI tools for testing gRPC (besides Postman) services in this post. I only used each tool for a day (so I won't be an in-depth analysis). 

An example of a simple gRPC server to run locally can be taken from my [Github](https://github.com/alexromanov/server-grpc-sample).

### What tools are there for gRPC?

* [Postman](https://blog.postman.com/postman-now-supports-grpc/)
* [Insomnia](https://docs.insomnia.rest/insomnia/grpc)
* [Kreya.app](https://kreya.app/)
* [BloomRPC](https://github.com/bloomrpc/bloomrpc)

### Tools comparison

![Project Structure]({{ site.baseurl }}/img/20220210/compare.png)

All four tools are **easily installed** as desktop applications (in my case, on Windows 11). Also I want to say, the all four tools have a great support for **gRPC streaming**. [k6 - when you will have this feature?](https://k6.io/)

In **BloomRPC**, I did not find how to import proto files via server reflection. But if you want to import it as files - it works perfectly fine. In other tools, both imports work fine.

![Project Structure]({{ site.baseurl }}/img/20220210/bloom.png)

It's rather inconvenient that **Insomnia** can't get a request template based on the proto schema. You have to search for it yourself and insert it as a JSON file. Other tools are OK with it.

![Project Structure]({{ site.baseurl }}/img/20220210/insomnia.png)

**Kreya.app** has a killer feature - you can use the built-in test data sampler - for strings, UUID, and other fundamental data types.

![Project Structure]({{ site.baseurl }}/img/20220210/kreya.png)

**Postman** has not yet delivered the ability to save created requests. But this is only a beta - so I hope this defect will be fixed during the release. Additionally, it is impossible to import a directory of proto files (or a set of dependent proto files at once). So, you need to compile a set of files to import it for now. But it works. 

![Project Structure]({{ site.baseurl }}/img/20220210/postman.png)

For **Postman** and **Insomnia**, gRPC is just one of the many features. HTTP API testing in both tools is the primary function. Plus, they have a lot of additional features for easy writing of API tests.
**Kreya** and **BloomRPC** are strictly working with gRPC.

### What to choose?
If you need a "combine - all-in-one," - then you can temporarily take something specialized, like Kreya or BloomRPC, and wait until **Postman will finish gRPC**. Postman already has a bunch of different goodies for HTTP (plus a bunch of tutorials and community).

If you are only testing gRPC - I would choose **Kreya.** This tool seemed convenient and has a set of unique features, such as a test data generator. In my daily work, I will use it for now. After a while - I will describe my experience in more detail. 