---
layout: post
title: "How to Configure Email setting in Blogengine admin for gmail"
date: 2020-08-13 17:55:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["blogengine", " email", " admin", "gmail", "godaddy"]
permalink: /post/How-to-Configure-Email-setting-in-Blogengine-admin-for-gmail
---


Below I will explain how to configure the Email setting for your blogengine website when you host in the GoDaddy server.

First, log in to Admin side using URL:`www.yourdomain.com/admin`

In the left side, menu navigate to SETTINGS > E-mail

![emailsetting](/assets/img/posts/2020/08/emailsetting.jpg)

> Please note that we are discussing a scenario where we want to receive emails from our blog site into your gmail account. The steps mentioned are only relating to gmail account.

Fill in the email address of the Gmail account and password. In the SMTP Server field instead of **smtp.gmail.com** put **relay-hosting.secureserver.net**. This is for the GoDaddy hosting provider this setting may be different for other hosting providers.

Also, change the port number from **587** to **25**.  
The subject prefix is the default heading of the email when we receive email through our website using the credentials of this Gmail account provided in the admin side.

After you enter the following details test the Email using TEST EMAIL SETTING.

Enjoy using Blogengine.