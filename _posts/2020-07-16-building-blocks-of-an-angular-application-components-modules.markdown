---
layout: post
title: "Building Blocks of an Angular Application - Components and Modules"
date: 2020-07-16 19:53:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["angular", "building blocks", "components", "Modules", "angular module", "register a component", "steps to create"]
permalink: /post/building-blocks-of-an-angular-application-components-modules
---


The basic building blocks of an Angular application are:

1.  Components
2.  Modules

**Component**

It is the combination of Data, HTML template, and Logic for showing the data.  
  
Component=Data+HTML Template+Logic

**Module**

Container of a group of related components

There are three steps for creating a component

*   Create a component
*   Register it in a Module
*   Add an element in HTML mark up

We will walk you through each step in creating a component and using it in the angular application.

**Creating a Component using Command Line**  
Command to use:

`ng g c Books`

![create-component](/assets/img/posts/2020/07/create-component.jpg)

**ng** stands for Next Generation, **g** stands for Generate, **c** stands for Component and **Books** stands for the name of the Component

> Tip: When you create a component the naming convention is important. We can use camel casing when typing the name of the component in the command window, and once the component is created each capital letter will be replaced by a dash(-). this is applicable to folder name as well as file names.
> 
> ![create-component_naming](/assets/img/posts/2020/07/create-component_naming.jpg)
> 
> ![create-component_naming_folder](/assets/img/posts/2020/07/create-component_naming_folder.jpg)


**Register Component in Module**

When you create a component it creates a folder and adds necessary files related to that. It also does the second step of registering the component in the module. this is the advantage of using Angular CLI. if we are creating a component manually then we need to register it by adding the component name in the declaration section inside Module.

![register-component](/assets/img/posts/2020/07/register-component.jpg)

**Add element in HTML markup**

Once the component is registered we can use the selector inside the component to add the HTML mark up in the main file.

![component-html-markup](/assets/img/posts/2020/07/component-html-markup.jpg)


**Component Structure**

 Once we create a component the contents of it will as shown below.

```typescript
import { Component, OnInit } from '@angular/core';
@Component({
  selector: 'app-books',
  templateUrl: './books.component.html',
  styleUrls: ['./books.component.scss']
})
export class BooksComponent implements OnInit {
  constructor() { }
  ngOnInit() {
  }
}
```

**@Component** is called a decorator function, and it has selector, templateUrl, styleUrls as you can see from the code above.

*   **selector**\- it is used to identify this component using the html markup. for example, the above component will be called in the main index.html file as below:

`<app-books></app-books>`

*   **templateUrl**\-it specifies the file path of  the html content used in this component. Mostly it will be inside the same folder. we can also add html content inline in the component.ts file itself, but it depends upon the purpose and use of the component. if you are using inline html, the keyword should be **template**, and is used is as below

```
template:'<h2>Books Component </h2>'
```

*   **styleUrls**\- It specifies the file path of the styles for this component. the style we add inside this css or scss file will reflect inside our component only. we can have multiple css files so it is denoted by an array.

Then we have **export** class ClassName. the export keyword denotes that this component class can be imported. this same class name we will be adding in the declarations of the module.

The class has a **constructor** which is used to initialize the values of the class and it is also used to set up DI(Dependency Injection). Constructor is called first when we render a component, so it needs to be kept clean and light for the optimal performance of the application.

The ngOnInit() function is called a **Life cycle hook**, which is used to execute some action after the component is fully loaded. there are other Life cycle hooks also that we will discuss more in a different article since it is out of the scope of this one.

In order to make the component fully working, I am adding one message from ts file.  
Below is the code inside **books.component.ts file**


```typescript
export class BooksComponent implements OnInit {
  message:any;
  constructor() { }
 
  ngOnInit() {
    this.message="Message from Component ts file!";
  }
```

Below is the code inside **books.component.html**


<p id="string-interpolation" >The syntax used in the below html is called <a href="#string-interpolation" class="anchor text-muted">String Interpolation</a></p>

{% raw %}
```html
<p>Books Works!</p>
<p>
    {{message}}
</p>
```
{% endraw %}

To run the full application enter the below command in the terminal.

`ng serve`

If everything goes well you will see the below message in the browser once you navigate to [http://localhost:4200/](http://localhost:4200/)


![component-runs](/assets/img/posts/2020/07/component-runs.jpg)

Hope you get a basic idea about component and modules in Angular.