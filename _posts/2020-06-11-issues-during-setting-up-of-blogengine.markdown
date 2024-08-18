---
layout: post
title: "Issues during setting up of BlogEngine"
date: 2020-08-11 19:45:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["blogengine", "hosting", " web.config", "write permission"]
permalink: /post/issues-during-setting-up-of-blogengine
---

Here we discuss some of the common issues faced by most people when they start hosting their blog using BlogEngine.Net.For the basic installation setting please refer to [setting up BlogEngine](https://blogengine.io/docs/get-started/).

I am talking about the latest version of blogengine as per writing this article i.e version: 3.3.8.0.

1.  Navigate to the ftp folder in which we are suppose to host the website files and upload the zip file downloaded from the website of [blogengine](https://github.com/BlogEngine/BlogEngine.NET/releases/download/v3.3.8.0/3380.zip "Download files").
2.  Unzip the contents of the file.
3.  Allow write permission to the folders **App\_Data** and **Custom.**  
    ![](/assets/img/posts/2020/08/sn1.png)  
    ![](/assets/img/posts/2020/08/sn2.jpg)
4.  Update the trust level in the web.config file, uncomment the line and change the value from **Medium** to **Full**.  
    ![](/assets/img/posts/2020/08/sn3.png)  
      
    
5.  Now you will be able to view the site at www.yourdomain.com.  
    Type `www.yourdomain.com/admin` to login to the admin panel

    ![](/assets/img/posts/2020/08/sn4.png)

    After login you could see a similar screen.

    ![](/assets/img/posts/2020/08/sn5.png)  
    

After above steps your blog site will be up and running. If you face any other issues during deployment please comment below.
