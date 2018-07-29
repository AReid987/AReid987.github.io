---
layout: post
title:      "JavaScript : Variable Scope, Hoisting, and This"
date:       2018-07-29 04:35:45 +0000
permalink:  javascript_variable_scope_hoisting_and_this
---


For a JavaScript developer, variable scope and hoisting are very important concepts to understand. However, they are not exactly the easiest concepts to learn at first. In this post, I'll cover these two concepts, along with an explanation of the `this` keyword in JS.

# Scope 
**var keyword**

Let's begin with scope - specifically using the var keyword. At a high level view, scope is essentially the context in which a variable exists. Well, what does that even mean? As Meek Mill puts it : "There's levels to this ...". At the highest level, variables are in the global scope. At any subsequent level, variables are in local scope. Basically, a variable that is declared outside of a function will be in global scope, and a variable declared inside of a function has local scope. 

Why does this matter? The scope determines accessibility. In the 'world' that is a program, any variables declared within the global scope can be accessed globally. This means a globally declared variable could be accessed from within a function, or even a function within a function. However, the opposite is not true. A variable declared inside a function has local scope, and thus, can only be accessed from within that function. 

In web developer land, the global scope is the window object, otherwise known as the entire HTML document. 

Scope works kind of like a building with an elevator that only goes one direction : up. If the global scope is the top floor, then each function is like a room on a lower floor, and each nested function is a successively lower floor. you can access any variable on a higher floor, but not on a lower floor. A variable declared in a function cannot be accessed from outside of that function, but a global variable can be accessed from inside any function. 

When using the var keyword, that's it, there is either global or local scope. This means that blocks, such as an if statement will not change the scope. A var declared in an if statement or a loop will still be in the global scope, unless that statement or loop is inside a function, in which case the var is function level scoped. 

One sublety of scope is that it is possible to use variables with the same name, but different values as long as they are in different scopes. This is because the interpreter looks for the variable at the local level first when you have a referenced variable in a function. For example : 

```
var person = 'Tom'; // global scope

function sayGreeting() {
  var person = 'Steve'; // local scope
	
  console.log("hello " + person); 
	}
	
	person; //=> "Tom" // referenced in global scope
	
	sayGreeting(); //=> hello Steve // invoked in global scope
	
	person; //=> "Tom" // referenced in global scope
```

On the other hand : 

```
var person = 'Tom';

function sayGreeting() {
  person = 'Steve';
	
  console.log("hello " + person); 
	}
	
	person; //=> "Tom"
	
	sayGreeting(); //=> hello Steve
	
	person; //=> "Steve"
```

In the first example, there are two var declarations, and though they are of the same name, they are actually two different variables. In the second example the var declared in the global scope is accessed and reassigned by the `sayGreeting()` function once it is invoked. 

Let's see another example: 

```
function sayGreeting() {
  var person = 'Steve'; // local scope
	
  console.log("hello " + person); 
	} 
	
	person; //=> Uncaught ReferenceError: person is not defined // global scope
```

Here, we try to access a variable outside of it's scope and get a ReferenceError. Since the var is declared in a function, we aren't able to access it outside of that function level (local) scope. 

However if a variable is declared without the var keyword, it will be added to the global scope (once the function has been invoked) :

```
function sayGreeting() {
  person = 'Steve'; // local scope
	
  console.log("hello " + person); 
	} 
	
	person; //=> Uncaught ReferenceError: person is not defined // global scope
	
	sayGreeting(); //=> hello Steve
	
	person; //=> Steve
 
```

**let/const keywords**

ES6 brought us the gift of let and const! Prior to ES6 there were only global scope and local scope. With let and const however, we now have block level scoping. The new keywords differ from var in that let can be reassigned (like var), but cannot be redeclared, and const cannot be reassigned nor redeclared. Both let and const have block level scope as opposed to the function level of var. What this means is that if a variable is declared and assigned in a block, even without being in a function it will be scoped to that block. 

```
var name = true

let person = "Tom"

if (name) {
  let person = "Steve"
	console.log("hello" + person)
}

console.log("hello" + person)

//output//
//=> helloTom
//=> helloSteve
```

With let we are able to get two different values due to the one variable being scoped to that block. 

Using const, you cannot reassign a variable once it has been declared and assigned or you will get an error. 


**Hoisting**

Hoisting is a JS mechanism through which variables are saved to memory. The JS interpreter does two 'scans' through the code. Upon the first 'scan', function and variable declarations are moved to the top of their scope. It is important to note that only the declaration is hoisted, and not the assignment. 

There is also different behaviour when using let vs var. If we try to access a var before it is assigned a value we will get `undefined` :

```
  console.log(person)
	
	var person = "Tom"
	
	//=> undefined
```

This happens because `var person` was hoisted, but the assignment of "Tom" has not happened when we console.log. Trying the same example with let would produce a reference error.

Inside of a function, the var will be hoisted to the top of the function level scope. Since let and const are block level scoped, if they are declared in a block within a function (i.e. an if statement), they will not be hoisted to the top of the function, but instead to the top of the block. 


Using let: 

```
let name = "Tom"

function sayName() {
  if ('a' === 'b') {
	  let name = "Steve"
		}
		console.log(name)
}

sayName() //=> Tom
```

Using var: 

```
var name = "Tom"

function sayName() {
  if ('a' === 'b') {
	  var name = "Steve"
		}
		console.log(name)
}

sayName() //=> undefined
```

In the first example let is scoped to the block within the if statement, so the console.log is referencing the name variable stored in the global scope. In the second example, the var name is hoisted to the top of the sayName function and is undefined when console.log references it. 

Just like in global scope, when a reference to a var is made in a block before it's declaration, the return will be `undefined`. When a reference to a let is made before it's declaration, the return will be and `ReferenceError`. 

For example :

```
// with var //

function ageLog(){
    console.log(age)
    var age = 31
    }
		
ageLog() //=> undefined


// with let //

function nameLog(){
    console.log(name)
    let name = "Me"
    }
		
nameLog() //=> Uncaught ReferenceError: name is not defined
```

**This**

This is a JS keyword that is pretty similar to self in ruby. I think of this like an identifier or pointer. As the JS interpreter evaluates code, it maintains this which will hold a reference to an object. Initially, this will reference the window object if it is call in the global scope. 

If this was called inside a function like so :

```
function someFunction() {
    console.log(this)
}
```

this would still be the window object.

However, when a function is called on an object, this becomes the object :

```
var obj = {}

function someFunction() {
  return this
}

obj.aFunction = someFunction

// output //

obj.aFunction() === obj  
//=> true

obj.aFunction() === window  
//=> false
```

**Conclusion**

Hopefully, this illustrates the interconnectedness of scope, hoisting and the this keyword. This is a pretty complex topic, that is integral to being a good JavaScript developer. 



