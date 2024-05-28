---
layout: post
title: "Basic concepts of Typescript"
date: 2020-06-17 06:10:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Typescript", "Type assertion", "Interface", "Constructor", "Access Modifiers", "Properties", "Arrow Function"]
permalink: /post/basic-concepts-of-typescript
---

Here we briefly discussed some of the important concepts of TypeScript:
- Type Assertion
- Arrow Function
- Interface
- Class
- Constructor
- Access Modifiers
- Properties and Fields

---

1. **Type Assertion**

    It helps to explicitly specify the type of a variable when the type of the variable is unknown.
    
    Two ways to do that:

    - Using the angle-bracket syntax: `<variableName>value`
    - Using the `as` keyword: `value as variableName`

    ```typescript
    let msg;
    msg='hello';
    
    let firstmethod=(<string> msg).toUpperCase();
    let secondmethod=(msg as string).toUpperCase();
    ```
2. **Arrow Function**

    Arrow functions are similar to lambda expressions in C# code. They simplify function declarations in many scenarios.
    
    Function without any parameter

    `let main = () => console.log('hello');`

    Function with parameters and no return type   

    ```typescript
     let sum = (x, y) => console.log(`sum of ${x} and ${y}`); 
     ```

    Function with parameters and return type

    ```typescript
    let sumxy = (x: number, y: number): number => {
        return x + y;
    };

    ```
    Here, we specify the type of the parameters in the function declaration itself. This is called **inline annotation**.
        
3. **Interface**

    It is used to encapsulate a set of related parameters into a single object. Below is an example of how to use a point object to draw using the interface.

    ```typescript 
    interface Point {
        x: number,
        y: number
    }

    let drawObject = (point: Point) => {
        // logic to draw Object
    }

    drawObject({
        x: 1,
        y: 2
    })

    ```
    The advantage of using the interface is that it provides nice intellisense and helps ensure type safety in programming.

    ![Interface screen shot](/assets/img/posts/2020/06/interface_screen_shot.jpg)

3. **Class**

    The concept of a class is used to group together related properties and functions. Below is an example of a simple `Point` class with two fields (`x` and `y`) and a `drawObject` function:

    ```typescript
    class Point {
        x: number;
        y: number;

        drawObject() {
            // logic to draw Object
            console.log(`Point x is: ${this.x} Point y is: ${this.y}`);
        }
    }

    let point = new Point();
    point.x = 1;
    point.y = 2;
    point.drawObject();

    ```

    An object is an instance of a class. In this case, the point object is used to initialize the `Point` class. The advantage of using classes is that they allow you to organize your code more effectively and encapsulate related functionality. Additionally, classes provide a blueprint for creating multiple instances with similar behavior.

4. **Constructor**

    A constructor is a method called when we initialize a class. In the following example, we add a constructor to pass values to the `x` and `y` variables.

    ```typescript
    class Point {
        x: number;
        y: number;

        constructor(_x?: number, _y?: number) {
            this.x = _x;
            this.y = _y;
        }

        drawObject() {
            // logic to draw Object
            console.log(`Point x is: ${this.x} Point y is: ${this.y}`);
        }
    }

    let point = new Point(1, 2);
    point.drawObject();
    ```
    `_x?:number` this syntax indicates that the parameter is optional

    The constructor allows you to set initial values for class properties when creating an instance of the class. It’s particularly useful for initializing variables or performing setup tasks.

5. **Access Modifiers**

    Access modifiers are keywords that can be applied to various members of a class to control their visibility from outside the class. The three common access modifiers in TypeScript (and many other programming languages) are:

    1. **Public** (by default, all members are public):
        - Public members are accessible from any code that can see the class.
        - They can be accessed both within and outside the class.

    2. **Private**:
        - Private members are only accessible within the class where they are defined.
        - They cannot be accessed from outside the class.
        - Useful for encapsulating implementation details.

    3. **Protected**:
        - Protected members are accessible within the class and its subclasses (derived classes).
        - They are not accessible from outside the class hierarchy.
        - Useful for providing a level of access between public and private.

    Here's an example of a `Point` class with private `x` and `y` properties:

    ```typescript
    class Point {
        private x: number;
        private y: number;
        // ... other members and methods
    }
    ```
    By using access modifiers, you can control the visibility and accessibility of class members, ensuring proper encapsulation and data hiding.

    **Access Modifier in Constructor**

    When we add an access modifier to the parameters of the constructor, it will generate fields of that class at runtime and initializes the values.
    
    ```typescript
    class Point {
        constructor(private x?: number, private y?: number) {
        // Constructor logic
        }
    }
    ```
6. **Properties and Fields**

    1. **Properties**:
    - Properties are like fields from outside the class, but internally, they are implemented as methods within the class.
    - They provide a convenient way to access and manipulate class data.
    - Properties can have getter and setter methods or just one of them.
    - By using properties, you can encapsulate logic related to data access.

    2. **Fields**:
    - Fields are variables that are privately accessible inside a class.
    - Conventionally, field names are prefixed with an underscore (e.g., `_x`).
    - Fields store the actual data associated with the class.

    ## Example: Point Class

    Consider the following TypeScript code:

    ```typescript
    class Point {
        private _x?: number;
        private _y?: number;

        constructor(_x?: number, _y?: number) {
            this._x = _x;
            this._y = _y;
        }

        get x() {
            return this._x;
        }

        set x(value) {
            this._x = value;
        }
    }

    // Usage
    const point = new Point(1, 2);
    const x = point.x; // Accessing the property
    point.x = 10; // Modifying the property
    ```
    In this example:

    The `Point` class has private fields `_x` and `_y`.
    The `x` property provides a getter and setter method for accessing and modifying the `_x` field.
    From outside the class, you can use `point.x` to get or set the value of `_x`.
    Remember that properties allow you to maintain control over data access while providing a clean interface for users of your class.