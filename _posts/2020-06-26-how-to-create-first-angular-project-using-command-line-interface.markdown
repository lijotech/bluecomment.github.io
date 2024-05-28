---
layout: post
title: "How to create first angular project using Command Line Interface ?"
date: 2020-06-26 18:30:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["angular new project", "first angular project", "node js install", "npm"]
permalink: /post/how-to-create-first-angular-project-using-command-line-interface
---

Steps Involved are:
- Download and install [node js](https://nodejs.org/en/download/). Node provides some tools to build Angular projects.
- Use NPM to install Angular CLI. Refer to [Install Angular CLI](/post/how-to-install-angular-cli).
- Command to create a new project in Angular:

`ng new hello-world`

Below are the screenshots of the project creation process:

![create-angular-project-step1](/assets/img/posts/2020/06/create-angular-project-step1.jpg)
![create-angular-project-step2](/assets/img/posts/2020/06/create-angular-project-step2.jpg)
![create-angular-project-step3](/assets/img/posts/2020/06/create-angular-project-step3.jpg)
![create-angular-project-step4](/assets/img/posts/2020/06/create-angular-project-step4.jpg)

- During setting up the project, it will ask for enabling Angular routing. Enter "yes" and continue.
- Also, choose the stylesheet format for the project.
- After the project creation is successful, it will try to integrate Git with the email present in the system. If it cannot find a proper Git-configured email, it will ask you to enter the name and email ID to integrate with Git. If you are not using Git, you can ignore this step.
- Finally, to run the project, type the following command:

`ng serve`

The default port for Angular is 4200. The project will be running at [http://localhost:4200/]http://localhost:4200/ after compiling.