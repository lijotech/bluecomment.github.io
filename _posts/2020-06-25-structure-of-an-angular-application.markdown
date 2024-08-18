---
layout: post
title: "Structure of an Angular Application"
date: 2020-06-25 05:46:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["angular", "solution", "structure of solution", "end to end test", "protractor", "tsconfig.json", "e2e", "node_modules", "main.ts ", "polyfills.ts", ".editorconfig", "karma.conf.js", "package.json", "tslint.json", "angular.json"]
permalink: /post/structure-of-an-angular-application
---

Below we explain the basic structure of an angular application. We give a brief idea about what is the purpose of each folder and files.

![structure-of-angular-application](/assets/img/posts/2020/06/structure-of-angular-application.jpg)

**e2e** \[Folder\]  
Used for writing end-to-end tests in the application. Mainly used for automated tests that stimulates a user browsing process on a web page.e2e test is done by using a library called [Protractor](https://www.protractortest.org/ "Protractor"). As an angular developer he doesn't need to worry about this folder, mostly comes in picture when we are doing testing in our application.

**\-protractor.conf.js** \[File\]  
It has all the setting for running test

**\-tsconfig.json** \[File\]  
It has settings for the compiler when running e2e tests.

**\-src** \[Folder\] **> app.e2e-spec.ts** \[File\]  
All our test cases are written in this file. The test code is written using Jasmine.

**\-src** \[Folder\] **\> app.po.ts** \[File\]  
We write code in this file to identify elements on our page.

**node\_modules** \[Folder\]  
It contains all third party libraries which are used for development purpose only. During compiling the contents of this folder are bundled. We will not deploy the _node\_modules_ folder to the production server.

**src** \[Folder\]  
Inside this folder our actual source code is present.

\-**app** \[Folder\]  
Here basic building blocks of angular applications like components and modules are present. Every application has at least one module and one component.

\-**assets** \[Folder\]  
we store all images and static files inside this folder.

\-**environments** \[Folder\]  
We store configuration settings for the different environments in this folder. Mostly it has two files _environment.prod.ts,environment.ts_

\-**main.ts** \[File\]  
This is the starting point of our application.it bootstraps the main module of the application and then the angular will load this module and the rest of the functionality works from there.

\-**polyfills.ts** \[File\]  
It is a bridging file to fill the gap between features that are supported in browsers and features need for the angular application to work.mainly it imports some javascript.

\-**styles.scss** \[File\]  
We add global styles in this file.

\-**test.ts** \[File\]  
It has settings for the testing environment.

**.editorconfig** \[File\]  
It contains the settings for the code editor. when working in a team on the same project all developers should have the same setting on this file.

**.gitignore** \[File\]  
It is used in version control to ignore files when checking out files to the repo.

**angular.json** \[File\]  
It contains standard configurations for the application.

**karma.conf.js** \[File\]  
Karma is a test runner in javascript. All settings are present in this file.

**package.json** \[File\]  
It is a standard file present for all applications. It specifies what all libraries our application is depending upon.it has two sections: _devDependencies_ -dependencies for the developer machine and those libraries are not needed to upload to production, and the other one _Dependencies_\-core libraries needed for the main application.

**tsconfig.json** \[File\]  
It has settings for the typescript compiler.

**tslint.json** \[File\]  
It is a static analysis tool for typescript code.it checks the code for readability and functionality errors.
