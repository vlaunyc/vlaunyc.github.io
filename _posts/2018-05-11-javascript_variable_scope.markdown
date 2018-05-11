---
layout: post
title:      "Javascript Variable Scope"
date:       2018-05-11 14:07:04 -0400
permalink:  javascript_variable_scope
---

Scope is the concept of where something is available. In Javascript it has to do with where declared variables and methods are available within our code. Essentially, if a variable or function is not declared inside a function or block it's in the global scope, and is therefore accessible from anywhere in your code. 


**FUNCTION SCOPE**

When a new function is declared, our code inside the function body is no longer in the global scope. Variables declared using `var` keyword are function scoped. Which means it will exist with in the scope of the function it is declared inside of.

```
var name = 'Mario';    // global scope

function evilCharacter() {  
  var name = 'Bowser';  // function scope
  console.log(name); 
}

console.log(name); //=> Mario
evilCharacter(); // => Bowser
console.log(name); //=> Mario
```
The above code declares `name` both inside and outside of function `evilCharacter()`. The variable declared with `var` inside the function `evilCharacter()` is not reachable from outside the function. Here you can only access Bowser though function `evilCharacter()`.

So what happens when we don't declare `var` in the function?

```
var name = 'Mario';

function evilCharacter() {  
  name = 'Bowser';
  console.log(name); 
}

console.log(name); //=> Mario
evilCharacter(); // => Bowser
console.log(name);// => Bowser

// oh no! Bowser took over Mario!
```
`name` outside the function (global scope) was overwritten by `name` inside function `evilCharacter()` because we didn't specify that `name` was to be scoped only to `evilCharacter()` .
<br>
<br>
**BLOCK SCOPE**

**VAR**

Variables declared with `var`  **do not** have block scope. 
```
var name = "Daisy"; 

if (true){
  var name = "Peach";
} 

name; // => 'Peach'
```
`name`'s value was over written within the **if block.** This logs `Peach` because the `var name` statement within the block is in the same scope as the `var name` statement before the block.
<br>
<br>
**`let` and `const` -- THE INTRODUCTION OF BLOCK SCOPE**

In ES6, `let` and `const` were introduced as alternative ways of declaring variables — both being blocked scoped.

In **block scope**, any block will be scoped, such as if-statements. 
<br>
<br>
**LET**

```
let name = "Daisy"; 

if (true){
  let name = "Peach";
} 

name; // => 'Daisy'
```
Even though `name` was assigned to `"Peach"` in the **if block**, that assignment was local to the block and therefore our global `name` was still `"Daisy"`. The **if block's** scope was separate from the global scope.
<br>
<br>
**CONST**

The same is true with `const`
```
const name = "Daisy"; 

if (true){
  const name = "Peach";
} 

name; // => 'Daisy'
```
*Note that the block-scoped `const name = "Peach";` does not give a SyntaxError because it can be declared uniquely within the block.*

This is one of the main reasons to not use `var` because it is not block scoped.  As long as you stick to declaring variables with `const` and `let`, what happens in a block stays in block.

