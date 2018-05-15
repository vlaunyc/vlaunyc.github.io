---
layout: post
title:      " JavaScript Hoisting"
date:       2018-05-15 00:50:51 +0000
permalink:  javascript_hoisting
---

<hr/>
**hoist** by definition means to raise or lift something.
<hr/>

Hoisting in JavaScript is the ***concept*** that the interpreter moves **function** and **variable** declarations to the top of the current referenced scope. However, they aren't *actually* moved to the top. Instead, all the variable and function declarations are put into memory first during the **compilation phase**, before going line by line though **execution phase**.

<br>

**WHAT IS VARIABLE HOISTING?**

Variables declared using `var` are always **hoisted** to the top of their **scope**.
```
1.    console.log(daisy)  // ReferenceError: daisy is not defined
2.    
3.    console.log(peach)  // undefined
4.    var peach = "Princess Peach"
```
As you see above, the variable `daisy` was never declared, so we get a `ReferenceError` saying "I've never heard of `daisy`!".

`peach` was declared first due to **hoisting** and returns `undefined` because it was logged before initialized. The reason for this is, during compilation phase, `var peach` was put into memory. The above code is executed as this:
```
1.    var peach  //  *declaration
2.    
3.    console.log(daisy)  // ReferenceError: daisy is not defined
4.    
5.    console.log(peach)  // undefined
6.    peach = "Princess Peach" //  *initialization
```
The interpreter **hoisted** the variable declaration to the top of the scope. However, in the example above, (at line 5) the variable was not assigned yet. `undefined` is saying "I know `peach` exists, but theres no value *yet*."

<br>

**BEST PRACTICE**

**Declare all variables at the top of the current scope.**

```
1.    var peach = "Princess Peach";
2.    var daisy = "Princess Daisy"; 
3.    
4.    console.log(peach);  // => Princess Peach
5.    console.log(daisy);  // => Princess Daisy
```


**Only use `let` and  `const`. Never use `var`.**

`let` and `const` aren't referenced before they've been initialized. 
```
1.    console.log('Hello', m); // =>  Uncaught ReferenceError: m is not defined
2.    let m = 'Mario'; 
```
```
1.    console.log('Hello', b); // =>  Uncaught ReferenceError: b is not defined
2.    const b = 'Bowser'; 
```
This ensures that we always declare our variables first.

Unlike `var` which is **hoisted** and can be accessed before they are defined. In such case will return `undefined`.
```
1.    console.log('Hello', y);  // =>  Hello undefined
2.    var y = 'Yoshi'; 
```
**Hoisting and `let`**

The `let` variable is scoped to the nearest block, instead of the nearest function. This means that `let` variables defined within non-function blocks like `if` will only be available within those blocks. 

The `let` variable, like `var`, can be created in two parts—declaration and initialization. However, as mentioned above, `let` isn't hoisted and it will return a `ReferenceError` if you try to reference it before it's initialized.


**Hoisting and `const`**

The `const` variable is a constant. Similar to `let`, it is scoped to a block instead of the nearest function. However, unlike `let` and `var`, `const` must be declared and initialized in one step and cannot be modified. Because of this, hoisting does not apply to `const` either.

<br>

**WHAT IS FUNCTION HOISTING?**

There are two ways of creating functions in JavaScript, through **Function Declaration** or **Function Expression**.

**Function Declarations** are **always hoisted** to the top of their scope. Function declarations, as with the `const` variable, are defined and initialized in one step. However, functions are hoisted by moving the entire function, not just the declaration, to the top of the scope. 
```
1.    sayHelloTo("Mario");  // => "Hello Mario"
2.    
3.    function sayHelloTo(character) {
4.      return "Hello " + character;
5.    }
```
Even though  `sayHelloTo` was defined after it was called, it still worked because of **hoisting**.  This is because functions are put into memory during compilation phase. The above code is executed as this:
```
1.    function sayHelloTo(character) {
2.      return "Hello " + character;
3.    }
4.    
5.    sayHelloTo("Mario");  // => "Hello Mario"
```
A function declaration must have a name. The JavaScript engine requires that we assign it an identifier that we can then use to refer to the function object in memory. 

However, there's no such constraint on function expressions. You can add a name or create an anonymous function expression.

<br>
**Function Expressions** are **not hoisted**. If you are assigning a function to a variable or property, you are dealing with a function expression. The variable declaration is hoisted (just like any other variable) but you won’t be able to call the function until the variable is assigned.
```
1.    hello("Mario"); // TypeError: hello is not a function
2.    greet("Luigi"); //TypeError: greet is not a function
3.    hey("Bowser"); //ReferenceError: hey is not defined
4.       
5.    // named function expression (only 'greet' gets hoisted)
6.    var greet = function hey(character) {
7.      console.log("Hey " + character);
8.    };
9.    
10.   // anonymous function expression ('hello' gets hoisted)
11.   var hello = function(character) {
12.     console.log("Hello " + character);
13.   };
```
The first error says "I've heard of `hello`, but it's not a function."  The second error says "I've heard of `greet`, but it's not a function."  The third error says "I've never heard of `hey`". The above code was executed as this:
```
1.    var hello;
2.    var greet;
3.  
4.    hello("Mario"); // TypeError: hello is not a function
5.    greet("Luigi"); //TypeError: greet is not a function
6.    hey("Bowser"); //ReferenceError: hey is not defined
7.    
8.    // named function expression (only 'greet' gets hoisted)
9.    var greet = function hey(character) {
10.    console.log("Hey " + character);
11.   };
12.  
13.   // anonymous function expression ('hello' gets hoisted)
14.   var hello = function(character) {
15.     console.log("Hello " + character);
16.   };
```
The `hello` and `greet` variables declarations were **hoisted** to the top, but haven't been assigned to anything so they're `undefined`. `undefined` is not a function and we get a `TypeError`.

<br>
**Named function expressions** 
```
// named function expression (only 'greet' gets hoisted)
var greet = function hey(character) {
  console.log("Hey " + character);
};
```
As you see in the above code, the function `hey` was not captured as part of the compilation phase. The function greet is still not hoisted even if we name our function. Only `greet`’s variable was hoisted and in memory as undefined during compilation phase. 

A named function expression can be handy when errors are thrown. The console will tell you what functions caused errors instead of stating anonymous aka stack trace.

If you want to refer to the current function inside the function body, you need to create a named function expression. The name is then local only to the function body scope. 

<br>
**Anonymous function expression**
```
// anonymous function expression ('hello' gets hoisted)
var hello = function(character) {
  console.log("Hello " + character);
};
```
Anonymous functions are functions without a name. There are a couple of ways to create functions however they are all created at application run time. Which means `hello` function is only created when we hit this line in the program.

One of the more famous use cases for anonymous functions are Immediately Invokable Function Expressions (IIFE). 

<br>
**IIFE (Immediately Invokable Function Expression)**

Function expressions can be **named** or **anonymous**, but they cannot start with the function keyword like this:

```
// IIFE (Immediately Invokable Function Expression)
(function() {
  var peach = "Hello Princess Peach"
  console.log(peach); // Hello Princess Peach
})();

//Variable name is not accessible from outside scope
console.log(peach) // ReferenceError: peach is not defined
```
An immediately-invoked function expression (or IIFE, pronounced "iffy"), is a function that runs as soon as it's defined. 

By placing the anonymous function in parentheses (a group context), the entire group is evaluated and the value returned. The returned value is actually the entire anonymous function itself, so all we have to do is add two parentheses after it to invoke it. 

The primary reason to use an IIFE is to is to obtain data privacy and avoid declaring variables in the global scope. Since JavaScript's `var` scopes variables to their containing function, any variables declared within the IIFE cannot be accessed by the outside world.

<br>

**RECAP**

* Undeclared `var` variables will lead to the variable being assigned a value of undefined upon hoisting.

* Undeclared variables  `let` and `const`, will lead to a Reference Error because the variable remains uninitialized at execution.

* Unlike **function declarations**, **function expressions** are not hoisted. 

* **Named function expressions** yields to a better debugging experience than **anonymous function expressions**.

**... thus,**

* When writing ES2015, the best practice is to always use `const`. If you need the variable to be updated, then use `let`. There should be no need to use `var`. If you must use `var`, declare all variables at the top of the current scope.  

* If your preference is to have functions hoist to the top use a **function declaration** otherwise use a **function expression**. 

* Declare all functions at the top of their scope. If the functions are declared in the global scope, put them at the top of the JavaScript file, else if they're declared inside another function, put the declaration at the top of the function body.

* Storing a **function expression** in a `const`declared variable is a good strategy since you get `const`'s scoping behavior and this allows better control over when the function is available. 



