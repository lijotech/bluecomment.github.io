---
layout: post
title: "Apache Camel ActiveMq Installation"
date: 2021-07-13 21:42:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["ActiveMq Installation","Apache ActiveMQ","Apache ActiveMQ installation"]
permalink: /post/apache-camel-activemq-installation
---
## ActiveMQ

Apache ActiveMQ is the most popular open source, multi-protocol, Java-based message broker. It supports industry standard protocols so users get the benefits of client choices across a broad range of languages and platforms. Connect from clients written in JavaScript, C, C++, Python, .Net, and more. Integrate your multi-platform applications using the ubiquitous **AMQP** protocol. Exchange messages between your web applications using **STOMP** over websockets. Manage your IoT devices using **MQTT**. Support your existing **JMS** infrastructure and beyond. ActiveMQ offers the power and flexibility to support any messaging use-case.

## Installation Steps

1.  Browse [ActiveMq](https://activemq.apache.org/download) Official page.![](/assets/img/posts/2021/07/officialpageActiveMq.JPG)
2.  Choose the ActiveMQ distribution you want to install.
3.  Choose the zip file for the respective OS distribution.![](/assets/img/posts/2021/07/activeMqVersion.JPG)
4.  Unzip the downloaded zip file and open the activeMQ folder.        ![](/assets/img/posts/2021/07/directory.JPG)
5.  Move into bin directory and choose the preferred architecture(win32 or win64).![](/assets/img/posts/2021/07/starter.JPG)
6.  Open the **activeMq.bat** file.![](/assets/img/posts/2021/07/startedlog.JPG)
7.  Browse to [http://localhost:8161/](http://localhost:8161/) to check whether activeMq is up and running.
8.  You will be prompted to enter the credentials. 
    
    ``` 
    username : admin
    password : admin
    ```
    
9.  Login to index page and choose Manage activeMQ broker![](/assets/img/posts/2021/07/manage.jpg)
10.  Browse to ActiveMQ Home page and enjoy.![](/assets/img/posts/2021/07/installedPage.JPG)

For further clarification please watch the video [https://youtu.be/QHIzu9rluIw](https://youtu.be/QHIzu9rluIw)