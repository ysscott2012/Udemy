# Udemy - Understanding the weird parts


## Section 2 - Execution Contexts and Lexical Environments

#### Syntax Parsers
A program that reads your code and determines what it does and its grammar is valid. 
#### Lexical Environments
Where something sits physically in the code you write.
#### Exception Contexts
A wrapper to help manage the code that is running.
#### Name/Value pairs
A name which maps to a unique value.
``` JavaScript
Address = '100 Main St.'
```
#### Objects 
A collection of name/value pairs.
``` JavaScript
Address: {
    Street: 'Main',
    Number: 100,
    Apartment: {
        Floor: 3,
        Number: 301
    }
}
```

#### The Global Environment & The Global Object 
Window object (this) is the global object inside browsers. When the global object is created, and its execution context is also created - global execution context.

The 'Globsl' means - not inside of any function.

In a following code, both variable a and function b are in global environment. You can type 'window' or 'this' in developer tool to see global varables and functions under global environment. If you want to see individual varable, you can type window.a or this.a in developer tool.
``` JavaScript
var a = 'Hello World';
function b () {
}
```

#### Creating and 'Hoisting'
Sets up memory space for variables and functions

In a following code, JavaScript engine reads variables and functions in global context, and set up the memory spaces for them before declaring their value. The engine sets up memory spaces by reading `var a;` and `function b()` lines. After setting up the memory spaces, the engine starts to read code line by line. First, the engine reads `b()` and goes into the `function b()` to get `console.log('Called b')`. Second, the engine reads `console.log(a)`. Although the memory `a` is already set up, it hasn't declare its value yet, so it returns `undefined`. Finally, engine reads `var a = 'Hello World'`, it decalres the value for the memory space `a`. 

``` JavaScript
b();
console.log(a);
var a = 'Hello World';
function b () {
    console.log('Called b!');
}
```
In a following code, you can see the `var a = 'Hello World'` is removed. In this case, memory space `a` will never be set up, which means the engine will throw exception when it reads `console.log(a)` : `Uncaught ReferenceError: a is not defined`.
``` JavaScript
b();
console.log(a);
function b () {
    console.log('Called b!');
}
```

#### JavaScript and 'undefined'
Undefined is a special value / special keyword in JavaScript. And it's the value that variables receive during the creation phase. The first phase of creating an execution context sets up the memory of the variable and in that memory space puts the value called 'undefined'.

#### Single Thread: 
One command at a time.
#### Synchronous Execution
One at a time.
#### Function Invocation
Running a function or calling a function.

In a following code, `function b()` is invoked inside of `function a()`


#### The Execution Stack
Execution stack means a sequence of created execution. For the following code example.

1. Global execution context is created. 
2. `function a()` and `function b()` are stored in memory space. 
3. After the first creating phase is finished, engine starts to execute code line by line.
4. When the engine executes an invoked `a()` function, a new execution context is created for `function a()` and placed what is called on execution stack. 
5. After the execution context for `function a()` is created, the engine executes line by line inside of `function a()`
6. While engines is executing the code inside of `function a()`, it executes invoked `b()`.
7. A new execution context is created for `function b()`, and then the engine executes code inside of `function b()` line by line.
8. After `function b()` is finished, the engine will popped off the stack back down to global execution context. 

```JavaScript
function b() {
   var d;
}
function a() {
    b();
    var c;
}
a();
var e;
```

#### Variable envirnoments 
where the variables live. (how they related to each other in memory)

Each variable (myVar) where we are looking at on a following code is defined within its own execution context.

```JavaScript
function b() {
    var myVar; // within function b execution context
}

function a() {
    var myVar = 2; // within function a execution context
    b();
}

var myVar = 1; // within global execution context.
a();
```
#### Scope 
Where are we able to see varialbes. 

#### The Scope Chain
Look down of the execution stack to find the variable.

In a following code, we can see there is no `myVar` declaration inside of `function b()`, so a JavaScript engines starts to look at memory space called `myVar` in its outer envirnoment (global execution context).

```JavaScript
function b() {
    console.log(myVar); // 1
}
function a() {
    var myVar = 2;
    b();
}
var myVar = 1;
a();
```

#### let: 
Block scoping.

### What about Asynchronous callbacks? 

#### Asynchronous
More than one at a time. 

Browser handles events or http requests asynchronously, but javascript is still synchronously. When the execution stack is empty. Browser look at the event queue to see which event needs to be run.

ex: In a following code, we will click while the `waitThreeSeconds()` function is running. In the developer tool, you can see the engine deal with click event after running the execution stack. 

```Javascript
// long running function
function waitThreeSeconds() {
    var ms = 3000 + new Date().getTime();
    while (new Date() < ms) {}
    console.log('finished function');
}

function clickHandler() {
    console.log('click event!');
}

document.addEventListener('click', clickHandler);

waitThreeSeconds();
console.log('finished execution');
```

## Section 3 - Types and Operators

#### Types and JavaScript

Dynamic typing: you don't tell the engin what type of data a variable holds, it figures it out while your code is running. 

```Javascript
// Static typing
bool isNew = 'hello'; // an error

// Dynamic typing
var isNew = true; // no error
isNew = 'yup!';
isNew = 1;
```

#### Primitive types

A type of data that represents a single value, and there are six type of primitive.

1. Undefined: it represents lack of existence. 
2. Null: it also represents lack of existence. 
3. Boolean: true or false.
4. Number: Floating point number. (There is only one 'number' type)
5. String: a sequence of characters.
6. Symbol: Used in ES6. 

#### Operators

A special function that is syntactically (written) differently. Generally, operators take two parameters and return one result.

#### Operator Precedence
Which operator function gets called first. Functions are called in order of precedence.

#### Associativity
What order operator functions get called in: left-to-right or right-to-left.

In a following code, you can search online about the [operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) order. As you can see, `Multiplication (*)` is 14, `Addition (+)` is 13 and `Equal (=)` is 3. So multiplication will be called before addition, and equal gets called last. 

```JavaScript
var a = 3 + 4 * 5;
console.log(a);
```

#### Coercion
Converting a value from one type to another.

JavaScript engine will coerce 1 to '1' by itself.
```javascript
var a = 1 + '2'
```

#### Comparison Operators

In `console.log(3 < 2 < 1)`. The engine run `3 < 2` first based on the precedence
 order and it returns `false`. It will be `false < 1`. When the engine corces false to number, it reutrns to `0`. Therefore, `0 < 1` is true.

```javascript
console.log(1 < 2 < 3); // return true
console.log(3 < 2 < 1); // return true
```

Equality `==` and Inequality `!=` coerce while comparing two value, this may work different from what you think, and this is hard to debug. 

```javascript
console.log(3 == 3); // return true
console.log('3' == 3); // return true
console.log(false == 0) // return true
console.log("" == 0) // return true
console.log("" == false) // return true
```

In order to prevent this happens, it is better to use Strick Equality `===` and Strick Inequality `!==`. More information, please check [Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)

```javascript
console.log(3 === 3); // return true
console.log('3' === 3); // return false
console.log(false === 0) // return false
console.log("" === 0) // return false
console.log("" === false) // return false
```

#### Existence and Booleans

As you can see a following example, JavaScript engine treats `""`, `undefined`, and `null` as false when they coerce to `Boolean`. Which means, we can use this as adventage to check if variable is valid. However, `0` value can also be treated as false. In order to prevent this happening, we need to add `a === 0` in the statement.

```javascript
Boolean("") // false
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false

var a;
if (a || a === 0) {
    console.log("Something is there");
}
```

#### Default values

We can use or operator another behavior to set a default value. Usually, the engine returns true or false while using or `||` operator. However, in a following case, engine returns first value which can be coerced as true. For example, `name || 'ABC'` will return name if name can be coerced as true, otherwise it returns 'ABC'.

```javascript
function greet(name) {
    name = name || '<Your name here>';
    console.log('Hello' + name);
}
greet();
```

### Framework Aside

#### Default values

If an index.html references three scripts. And both library1.js and library2.js have a declared variable called `libraryName`. If we want to use `libraryName` inside of app.js, what value will we get? 

These three javascript files share the same golbal execution context. Based on the order of script, `library 2` is assigned to `libraryName` after `library 1`, so app.js gets `library 2`. How can we prevent this? 

```html
// index.html
<html>
    <head>
    </head>
    <body>
        <script src="library1.js"></script>
        <script src="library2.js"></script>
        <script src="app.js"></script>
    </body>
</html>
```

```Javascript
// library1.js
var libraryName = 'library 1';
```
```Javascript
// library2.js
var libraryName = 'library 2';
```
```Javascript
// app.js
console.log(libraryName);
```

Same! In order to prevent this happening is to give the default value by using `or` operator. 

 ```Javascript
// library1.js
window.libraryName = window.libraryName || 'library 1';
```
```Javascript
// library2.js
window.libraryName = window.libraryName || 'library 2';
```
```Javascript
// app.js
console.log(libraryName);
```


## Section 4 - Objects and Functions

#### Objects and the Dot

1. Primitive "property".
2. Object "property".
3. Function "method".

``` JavaScript
// object (not the perfer way to create an object)
var person = new Object();

// property
person["firstName"] = "YS";
person["lastName"] = "Chen"

var firstNameProperty = "firstName";
console.log(person[firstNameProperty]);

console.log(person.firstName);

person.address = new Object();
person.address.street = "100 Main St.";
person.address.state = "Maryland";
person.address.city = "Rockville";

console.log(person["address"]["city"]);
```

#### Objects and object literals

A following initialization of object is an object literal syntax.

``` JavaScript
// curly brace - better way to create an object with its initialization.
var YS = {
    firstName : 'YS',
    lastName: 'Chen',
    address: {
        street: '100 Main St.',
        state: 'Maryland',
        city: 'Rockville'
    }
}; 

function greet(person) {
    console.log('Hi ', person.firstName);
}

greet(YS);
greet({firstName: 'a', lastName: 'b'});

```

### Framework Aside

#### Faking namespace
A container for variables and functions. Typically to keep variables and functions with the same name separate.

``` JavaScript
var greet = 'Hello!';
var greet = 'Hola!';
console.log(greet);

var english = {};
var spanish = {};

english.greet = 'Hello!';
spanish.greet = 'Hola!';
```

#### Functions are objects
#### First class functions
Everything you can do with other types you can do with functions. Assign them to variables, pass them around, create them on the fly.

Function 

1. Primitive 
2. Object
3. Function 
4. Name (optional)
5. Code 


``` JavaScript
function greet() {
    console.log('hi');
}
// property attaches to the function.
greet.language = 'English';
```

#### Function statements and function expressions
#### Expreession 
A unit of code that results in a value. It doesn't have to save variables.

``` JavaScript
// function expression (it results the values)

anonymousGreet(); // this will not work
var anonymousGreet = function() {
    console.log("hi");
};

// function statement 

greet(); // this works
function greet() {
    console.log("hi");
};
```


### Conceptual Aside

#### Mutate 
To change something.  
#### Immutate 
Can't be changed.
#### By value and by reference

In a following code, 3 (a primitive value) sits in a memory `a`, and `b` copy `a`'s value and with its copied primitive value. Copying the value into two seperate spots in memory.
```JavaScript
a = 3; // primitive type
b = a; // by vaule. 
```

In a following code, `{}` (an object) sits in a memory `a`, and `b` points to the same location of memory insteading of creating a new memory lication. Two names point to the same address.
`All objects interact with reference.` 
```JavaScript
a = {}; // object
b = a; // by reference.
```
Example: 

```JavaScript 
// by value (primitives);
var a = 3;
var b;

b = a;
a = 2;
console.log(b);
console.log(a);


// by reference (all objects (including functions))
var c = { greet: 'hi'};
var d;

d = c;
c.greet = 'hello'; // mutate
console.log(c);
console.log(d);


// by reference (even as parameter)
function changeGreeting(obj) {
    obj.greeting = 'Hola!' // mutate
} 

changeGreeting(d);
console.log(c);
console.log(d);

// equals operator sets up new memory space (new address)

c = { greeting: 'howday' };
console.log(c);
console.log(d);

```

#### Objects, functions and 'this'

'this' will point to different object, function and things depends on how the function is invoked.

Creating the function no matter using function statement or function expression, 'this' points to the global object.

```javascript

// 'this' still points to global object
function a () {
    console.log(this)
    this.newVariable = 'hello'
}
a();
var b = function () {
    console.log(this)
}
b();
console.log(newVariable);

// 'this' becomes the object - c 
var c = {
    name: 'The c object',
    log: function() {
        // self points to this object - c
        var self = this;
        this.name = 'Updated c object';
        console.log(self);

        var setname = function (newname) {
            self.name = newname;
        }
        setname('update again! the c object');
        console.log(self);
    }
}

c.log();
```

#### Arrays
#### arguments 
The parameters you pass to a function

```javascript
function greet (firstName, lastName, language, ...other) {
    if (arguments.length === 0) {
        console.log('Missing parameters!');
        return
    }
    console.log(firstName)
    console.log(lastName)
    console.log(language)
    // special word in javascript: contains all the values are passed. 
    console.log(arguments);
    console.log(arguments[0]);
}

// don't pass value, result will be undefined
greet()
greet('John')
greet('John', 'Doe')
greet('John', 'Doe', 'English')
greet('John', 'Doe', 'English', 'Rockville', 'Maryland')
```

#### spread parameter
If the parameters are passed more than function arguments, those parameter will be taken as array in '...other';

```javascript
function greet (firstName, ...other) {

}
 ```

### Framework Aside

#### function overloading
Function which has the same name, but with different parameters. 

Javascript doesn't have function overloading because functions are objects. So `function overloading` is not availabe in the way that JavaScript deal with functions. 

The alternative way:

```javascript
function greet (firstName, lastName, language) {
    language = language || 'en';
    if (language === 'en') {
        console.log('Hello ' + firstName + ' ' + lastName);
    }

    if (language === 'es') {
        console.log('Hola ' + firstName + ' ' + lastName);
    }
}

function greetEngilish(firstName, lastName) {
    greet(firstName, lastName, 'en');
}

function greetSpanish(firstName, lastName) {
    greet(firstName, lastName, 'es');
}

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doe');
```

### Dangerous Aside
#### Automatic semicolon insertion 

In a following code, the engine automatically put a semicolon after return, so it return 'undefined'.

```Javascript
function getPerson () {
    // this causes error.
    return
    {
        firstname: 'YS',
        lastname:'chen' 
    }
    // this prevents automatically semicolon insertion.
    return {
        firstname: 'YS',
        lastname:'chen' 
    }
}

console.log(getPerson());
```

### Framework Aside
#### Whitespace
Invisible characters that create literal 'space' in your written code.

```Javascript
var 
    // firstname of the person.
    firstname, 
    
    // lastname of the person.
    lastname, 
    
    // the language
    // can be 'es' or 'en';
    language;
var person = {

    // the firstname
    firstname: 'YS',

    // the lastname
    lastname: 'chen'
}
console.log(person);
```

#### Immediately invoked function expressions (IIFE)S (important)


```Javascript
// using function statement
function greet(name) {
    console.log('Hello ' + name);
}
greet('YS');

// using function expression
var greetFunction = function(name) {
    console.log('Hello ' + name);
}

greetFunction('YS');

// using Immediately invoked function expressions (IIFE)S
var greeting = function(name) {
    console.log('Hello ' + name);
}('YS');

// using return
var greeting1 = function(name) {
    return 'Hello ' + name;
}('YS');
console.log(greeting1)

// using Immediately invoked function expressions (IIFE)S
(function (name){
    var greeting = 'Hello';
    console.log('Inside IIFES ' + greeting + ' ' + name);
}('YS'));

```

### Framework Aside

#### IIFES and save code. (important)

Execution stack: the engine creats a memory for anonymous function in global execution context. An new execution context is created when engine reads `('YS')`. The variable is declared inside of the anonymous function is with its own execution context, not touching global execution context. 

Putting `()` first in the code will prevent overriding variables from the global execution context. It wraps all code inside of IIFES

```js
// IIFE
(function (name){
    var greeting = 'Hello';
    console.log('Inside IIFES ' + greeting + ' ' + name);
}('YS'));

```

If I want to pass or declare global variables or functions into IIFES? What should I do? Please see a following code. Passing window object into IIFES.

```js
// IIFE
(function (golbal, name){
    var greeting = 'Hello';
    golbal.greeting = 'Hello';
    console.log('Inside IIFES ' + greeting + ' ' + name);
}(window, 'YS'));
```

#### Understanding closures (important)

```js
function greet(whattosay) {

    return function (name) {
        console.log(whattosay + ' ' + name)
    }

}

greet('Hi')('YS'); 
var sayhi = greet('Hi');
sayhi('YS');
```

In a following case, it returns 3 for `fs[0]()`, `fs[1]()` and `fs[2]()`.

```js
function buildFunction() {

    var arr = [];

    for (var i = 0; i < 3; i++) {
        arr.push(
            function() {
                console.log(i); 
            }
        )
    }

    return arr;

}

var fs = buildFunction();
fs[0]();
fs[1]();
fs[2]();
```

### Framework Aside
#### function factories (important)

A following code is the way to use function factories by using closures capability. 

```js
// creating a factory
function makeGreeting(language) {

    return function (firstname, lastname) {
        if (language === 'en') {
            console.log ('Hello' + firstname + ' ' + lastname);
        }

        if (language === 'es') {
            console.log ('Hola' + firstname + ' ' + lastname);
        }
    }

}

var greetEnglish = makeGreeting('en');
var greetSpanish = makeGreeting('es');

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doe');

```

#### closures and callbacks (important) 

```js
function sayHiLater() {

    var greeting = 'hi';

    setTimeout( function() {
        console.log(greet);
    }, 3000);
}

sayHiLater();

// jQuery uses function expressions and first-class functions!
$('button').click(function() {

});
```

#### callback functions (important)
A function you give to another function, to be run when the other function is finished.

```js
function tellMeWhenDone (callback) {
    var a = 1000;
    var b = 2000; 
    callback();
}

tellMeWhenDone (function() {
    console.log('I am done');
})

tellMeWhenDone (function() {
    console.log('All done');
})
```

#### call(), apply() and bind() (important)

All functions have call, apply and bind methods. All three of these have to do with the `'this'` variable and the arguments that you pass to the function as well.

The `.bind` create a copy of whatever function you are calling it on. And whatever method you pass to it, whatever object you pass to this method. The object you pass is what the `this` variable points to by reference.

The `.call` call a function and pass object to this method, and also invoke it. 

The `.apply` call a function and pass object to this method, and also invoke it. The difference from '.call' is to use array.

```js
var person = {
    firstname: 'YS',
    lastname: 'Chen',
    getFullname: function () {

         var fullname = this.firstname + ' ' + this.lastname;
         return fullname;

    }
}

var logName = function (lang1, lang2) {

    console.log('Logged: ' + this.getFullname());
    console.log('My arguments ' + lang1 + ' ' + lang2);
    console.log('----------')

}

var logPersonName = logName.bind(person)

logPersonName('en');

logName.call(person, 'en', 'es');
logName.apply(person, ['en', 'es']);


(function (lang1, lang2) {

    console.log('Logged: ' + this.getFullname());
    console.log('My arguments ' + lang1 + ' ' + lang2);
    console.log('----------')

}).apply(person, ['en', 'es'])


//////
// function borrowing
var person2 = {
    firstname: 'Jane',
    lastname: 'Doe'
}

// setting `this` keyword to person2 object
person.getFullname.apply(person2);

//////
// function currying 
function multiply(a, b) {
    return a*b;
}

var multipleByTwo = multiply.bind(this, 2);
console.log(multipleByTwo(4)); // 8

var multipleByTheee = multiply.bind(this, 3);
console.log(multipleByTheee(4)); // 12
```

#### function currying (important)
Creating a copy of a function but with some preset parameters


#### functional programming (important)

```js
function mapForEach(arr, fn) {

    var newArr = [];
    for (var i = 0; i < arr.length; i++) {

        newArr.push(
            fn(arr[i]);
        )

    }
} 

var arr1 = [1, 2, 3];
console.log(arr1)

var arr2 = [];
for (var i=0; i<3 ; i++) {

    arr2.push(arr1[i] * 2);

}

var arr3 = mapForEach(arr1, function (item){
    return item * 2;
})
console.log(arr3);


var arr4 = mapForEach(arr1, function (item){
    return item > 3;
})
console.log(arr4);

var checkPassLimit = function (limiter, item) {
    return item > limiter;
}

var arr5 = mapForEach(arr1, checkPassLimit.bind(this, 1))
console.log(arr5);

var checkPassLimitSimplified = function (limiter) {
    return function (limiter, item) {
        return item > limiter;
    }.bind(this, limiter);
}
var arr6 = mapForEach(arr1, checkPassLimitSimplified(2))
console.log(arr6);

```

#### functional programming 2
using [underscore.js](http://underscorejs.org/docs/underscore.html)

```js
var arr7 = _.map(arr1, function(item) { return item * 3});
console.log(arr7);

var arr8 = _.filter([2,3,4,5,6,7], function (item) {return item % 2 === 0;})
console.log(arr8)
```     

## Section 5: Object-Oriented Javascript and prototypal inheritance.

#### Classical and prototypal inheritance
`Inheritance`: one object gets access to the properties and methods of another object.

`Classical Inheritance`: Verbose. 
- friend
- protected
- private
- interface

`Prototypal Inheritance`: Simple
- flexible
- extensible
- easy to understand

#### Understanding the prototype

Object looks for its own properties first before looking for in prototype.

```js
ver person = {
    firstname: 'default',
    lastname: 'default',
    getFullname: function () {
        return this.firstname + ' ' + this.lastname;
    }
}

var john = {
    firstname: 'John',
    lastname: 'Doe'
}

// don't do this ever, for demo purpose only.
john.__proto__ = person;
console.log(john.getFullname());
console.log(john.firstname);
console.log(john.lastname);

var jane = {
    firstname: 'Jane'
}
jane.__proto__ = person;
```

#### Reflection and extend
`Reflection`: An object can look at itself, listing and changing its properties and methods.

```js
for (var prop in john) {
    if (john.hasOwnProperty(prop)) {
         console.log(prop + ': ' + john[prop]);
    }
}

var jane = {
    address: '100 Main St.',
    getFormalFullName: function () {
        return this.firstname + ' ' + this.lastname;
    }
}

var jim = {
    getFirstName: function () {
        return this.firstname;
    }
}

_.extend (john, jane, jim);
console.log(john);
``` 


## Section 6: Building Objects

#### Function constructors, 'new', and the history of javascript.

`Function Constructor`: A normal function that is used to construct objects. (The 'this' variable points a new empty object, and that object is returned from the function automatically)

```js
function Person(firstname, lastname) {
    console.log(this)
    this.firstname = firstname;
    this.lastname = lastname;
    console.log('The function is invoked')
    return { greeting: 'I got in the way'}; 
}

var john = new Person('John', 'Doe');
console.log(john);
```

#### Function constructors and '.Prototype'

```js
function Person(firstname, lastname) {
    
    this.firstname = firstname;
    this.lastname = lastname;
    // this will use a lot of memory space if we decalre 1000 new objects
    this.getFillName = function() {

    }
    
}

// this will save memory space
Person.prototype.getFullName = function() {
    return this.firstname + ' ' + this.lastname;
}

var john = new Person('John', 'Doe');
console.log(john);

Person.prototype.getFormalFullName = function() {
    return this..lastname + ', ' + this.firstname;
}
```


### Dangerous Aside
#### 'new' and functions

Forgot to put 'new' before Person.

```js
function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}

// this will save memory space
Person.prototype.getFullName = function() {
    return this.firstname + ' ' + this.lastname;
}
// dangerous
var john = Person('John', 'Doe');
console.log(john);
// dangerous
var jane = Person('jane', 'Doe');
console.log(jane);

Person.prototype.getFormalFullName = function() {
    return this..lastname + ', ' + this.firstname;
}
```

#### Built-in function constructors 

We can add customized function into javascript built-in object function. 

```js
String.prototype.isLengthGreaterThan = function (limit) {
    return this.length > limit;
}

console.log("john".isLengthGreaterThan(3)); // true

Number.prototype.isPositive = function () {
    return this > 0;    
}

```

#### Dangerous Aside: built-in function constructors

Try not to use built-in function construstor, try to use primitive value.

In a following code, a is an primitive value, b is a object. 
```js
var a = 3;
var b = new Number(3);
a == b // true
a === b //false
```

#### Dangerous Aside: for...in..
Don't use for..in to for Array.

```js
Array.prototype.myCustomFeature = 'cool';

// this will get myCustomFeature = cool
var arr = ['John', 'Jane', 'Jim'];
for (var prop in arr) {
    console.log(prop + ':' + arr[prop]);
}

for (var i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
```

#### Object.create and Pure Prototypal Inheritance

Pure Prototypal Inheritance: `Object.create()` creates an empty object with its prototype.

```js
var person = {
    fristname: 'Default',
    lastname: 'Default',
    greet: function() {
        return 'Hi, ' + this.firstname;
    }
}

var john = Object.create(person);
john.firstname = 'John';
john.lastname = 'Doe';
console.log(john)
```

#### Ployfill
Code that adds a feature which the engine may lack.

```js
// polyfill
if (!Object.create) {
    Object.create = function (o) {
        if (arguments.length > 1) {
            throw new Error('Object.create implementation only accepts a first parameter')
        }
        function F() {}
        F.prototype = o;
        return new F();
    }
}
```

#### ES6 and classes
This is another way to approach creating object.

`extends`: sets the Prototype (__proto__)

```js
class Person {
    constructor(firstname, lastname) {
        this.firstname = firstname;
        this.lastname = lastname;
    }
    greet() {
        return 'Hi ' + firstname;
    }
}

var john = new Person('john', 'doe');

class InformalPerson extends Person {
    constructor(firstname, lastname) {
        super(firstname, lastname)
    }
    // override 
    greet() {
        return 'Yo ' + firstname;
    }
}


```
#### Syntactic sugar
A different way to type something that doesn't change how it works under the hood.


## Section 7: Initialization

#### ODDS and ENDS

```js
var people = [
    {
        // the john object
        firstname: 'John',
        lastname: 'Doe',
        addresses: [
            '111 Main St.',
            '222 Main St.'
        ]
    },
    {
        // the jane object
        firstname: 'Jane',
        lastname: 'Doe',
        addresses: [
            '333 Main St.',
            '444 Main St.'
        ],
        greet: function () {
            return 'Hello!'
        }
    }
];
```

#### typeof, instanceof, and figuaring out what something is.

`typeof`: is an operator that accepts a parameter, but it's essentially a function returning a string.

`instanceof`: if you are dealing with an object chains, will tell you something has in its prototype chain.

#### strict mode

If we don't use `use strict`, this will pass. If we use `use strict`, it throws an exception.

```js
"use strict";

var person;

persom = {};
console.log(persom);
```

