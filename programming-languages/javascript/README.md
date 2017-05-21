# Javascript

## Statements
- A block statement is something between { }

## Variables

We have three way of declaring variables:

```javascript
var width = 100;
let height = 200;
const key = 'key';
```

The `var` keyword is the oldest one and:
- you can assign and update them
- when you update one variable we can still define the keyword `var`in front of them and it does not throw any errors (to remember in case you define two variables with the same name in the same scope)
- they are function scope or globally scope
- if you define a var inside an if statement thinking it will work as temporary variable, it will not, because is function scope and if you define the if in global scope, the variable it will leaked outside
- One way to not leaked out the variables is something called Immediately-Invoked Function Expression.
- If you use the variable before its definition you will get `undefined`

the `let` keyword:
- it is block scoped
- you can't redeclare a variable with the same name in the same scope
- you can update it
- if you use the variable before its definition you will get a not defined error. this is called temporal dead zone.

If you write something like this:
```javascript
for(var i=0; i<10; i++) {
  setTimeout(function() {
    console.log('The number is +'i);
  }, 1000);
}
```
You will display ten times `The number is 10` and because `var` is function scoped, instead if you use `let` you will display correctly the sequence because they are block scoped.

The `const` keyword:
- it is read only variable. I. e. you can't update it.
- it is block scoped
- you can't redeclare a variable with the same name in the same scope
- if you define an object you can still update its attribute. In case you want something immutable you can use`Object.freeze`
- If you use the variable before its definition you will get a not defined error. This is called temporal dead zone.

## Functions

### Arrow functions
Benefits:
- They are more concise
- They have implicit return
- It does not rebind the value of `this` and inherits it from the parent
- They are anonymous function
- When you need to use `var self = this` now instead you can use an arrow function

Do not use:
- in `addEventListener` because arrow function does not rebind this and it will be `window`
- when you need to bind to an object
- when you need to add a prototype method
- when you need `arguments` object

Before:
```javascript
const names = ['a', 'b', 'c'];
const hinames = names.map(function(name) {
  return `hi ${name}`;
});
```

After:
```javascript
const names = ['a', 'b', 'c'];
const hinames = names.map(name => `hi ${name}`);
```

In case you don't have parameters:
```javascript
const hinames = names.map(() => `hi simon`);
```

### Default Arguments
You can define default arguments: if you want to trigger one default argument pass `undefined`.

## String

### Template strings
You can use backtick to:

- stick variables inside a string. E.g.:

```javascript
const name = 'simon';
const sentence = `hi ${name}`;
```

- to create string multiline like html fragment
- you can tag it with a function. E.g.:

```javascript
# In this case we have:
# strings = ['hi', '']
# values = ['simon']
function highlight(strings, ...values) {
  let str = ''
  strings.forEach((string, i) => {
   str += `${string} <span>${values[i] || ''}</span>`;
  });
  return str;
}

const name = 'simon';
const sentence = highlight`hi ${name}`;
```

The value of `sentence` will be `hi  <span>simon</span> <span></span>`.

### Destructuring
It allows us to extract data from an object or array.

```javascript
const person = {
  first: 'Simon',
  last: 'Vocella',
  country: 'Italy',
  city: 'Milan',
  twitter: '@simonvocella'
};
const { first, last, twitter } = person;
```

We can:

- rename one property using `const { twitter: tweet} = person;` and in this case we have a variable named `tweet`
- add default values using `const { facebook = 'unknown' } = person;`

We can destructuring array with:

```javascript
const details = ['Wes Bos', 123, 'wesbos.com'];
const [name, id, website] = details;
```

We can use destructuring in function:

```javascript
function tipCalc({ total = 100, tip = 0.15, tax = 0.13 }) {
  return total + (tip * total) + (tax * total);
}
const bill = tipCalc({ tip: 0.20, total: 200 });
```

In this case `bill` has tax `0.13` and we can specify parameters in any order.
If we want `const bill = tipCalc();` we need to declare in this way.

```javascript
function tipCalc({ total = 100, tip = 0.15, tax = 0.13 } = {}) {
  return total + (tip * total) + (tax * total);
}
```

In this case `bill` will be `128`.

## Looping
We can loop with:

```javascript

const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

// 1. normal way
for (let i = 0; i < cuts.length; i++) {
  console.log(cuts[i]);
}

// 2. for each and anonymous function
cuts.forEach((cut) => {
  console.log(cut);
  if(cut === 'Brisket') {
    continue;
  }
});

// 3. for in loop
for (const index in cuts) {
  console.log(cuts[index]);
}

// 4. for of loop
for (const cut of cuts) {
  console.log(cut);
}
```

For the second case you can't use `break` or `continue`.
For the third case you it iterates *absolutely* through the array including properties or what we add to the prototype of Array. It is useful if you want to iterate through object properties.
The fourth case works as normal `foreach`. If you want to iterate through object properties you should use `Object.keys` or `entries`.

## Array

*Array.from*
*Array.of*
*Array.find*
*Array.findIndex*
*Array.some*
*Array.every*

## Operators

### Spread
In functions:

```javascript
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
console.log(f(...[1,2,3]))
```

In arrays:

```javascript
var parts = ["shoulders", "knees"];
var lyrics = ["head", ...parts, "and", "toes"];

console.log(lyrics)
```

### Rest
We can allow unlimited params to function by using the rest operator.
```javascript
function demo(part1, ...part2) {
  return {part1, part2}
}

console.log(demo(1,2,3,4,5,6))
```

## Enhanced Object Literals
Object literals are extended to support setting the prototype at construction, shorthand for `foo: foo` assignments, defining methods, making super calls, and computing property names with expressions.  Together, these also bring object literals and class declarations closer together, and let object-based design benefit from some of the same conveniences.

```JavaScript
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // Shorthand for ‘handler: handler’
    handler,
    // Methods
    toString() {
     // Super calls
     return "d " + super.toString();
    },
    // Computed (dynamic) property names
    [ 'prop_' + (() => 42)() ]: 42
};
```

## Promises
Promises are a library for asynchronous programming. Promises are a first class representation of a value that may be made available in the future. Promises are used in many existing JavaScript libraries.

```JavaScript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

### Quick Promise
Need a quick always resolved promise?

```javascript

var p1 = Promise.resolve("1")
var p2 = Promise.reject("2")

Promise.race([p1, p2]).then((res) => {
   console.log(res)
})
```

### Fail fast
If a promise fails, `all` and `race` will reject as well.

```javascript
var p1 = new Promise((resolve, reject) => {
  setTimeout(() => resolve("1"), 1001)
})
var p2 = new Promise((resolve, reject) => {
  setTimeout(() => reject("2"), 1)
})

Promise.race([p1, p2]).then((res) => {
   console.log("success" + res)
}, res => {
   console.log("error " + res)
})

Promise.all([p1, p2]).then((res) => {
   console.log("success" + res)
}, res => {
   console.log("error " + res)
})
```

### Primitive types
There are seven primitive types: Number, String, Object, Boolean, null, undefined, Symbol

Numbers, String, Boolean are immutable object

### Number
- 64 bit floating point
- IEEE-754 (aka "Double")
- Associative law does not hold: (a + b) + c === a + (b + c).
Produce false for some values of a, b, c, e.g. decimal fractions, big integers
- They are first class objects:
  - A number can be stored in a variable
  - A number can be passed as a parameter
  - A number can be returned from a function
  - A number can be stored in a object

### NaN
- Special number: Not a Number
- It is result of undefined or erronous operations
- Toxic: any operations with NaN as an input will have NaN as output
- NaN is not equal to anything, including NaN itself

### Boolean
- true, false

### String
- A sequence of 0 or more 16-bit Unicode characters
- No separate type for character type (they are just string with length 1)
- Strings are immutable
- Similar string are equal (===)
- String literals can use single/double quotes with \ for escapment

### Array
- Array inherits from Object
- Indexes are converted to strings and used as names for retrieving values
- Very efficient for sparse array
- Not very efficient in most of other cases
- One advantage: No need to provide a length or type when creating an array
- [] literal version of Array
- It can contain any number of expressions separated by commas
- New items can be appendend
- The dot notation should be not used by arrays

sort myArray:
-> it sorts alphabetically (everything number too, but we can pass a function in sort)

delete myArray[1];
-> it puts undefined in number 1 index;
-> we need to use splice to delete the element and resize the array

### Get, set and delete
- get
  object.name
  object[expression]

- set
  object.name = value
  object[expression] = value

- delete
  delete object.name
  delete object[expression]

### Properties
A property is a named collection of attribute
- value -> any Javascript value
- writeble -> boolean
- enumerable -> boolean
- configurable -> boolean
- get -> function() { ... return value }
- set -> function(value) { ... }

### Object literal
An expressive notation to define object

### Working with prototypes
- Make an object that you like
- Create new instances that inherit from that object
- Customize new objects
- Taxonomy and classification are not necessary
- Object can have prototype attribute (object or null)

### New operator
```javascript
function new(func, arguments) {
  var that = Object.creatre(func.prototype),
      result = func.apply(that, arguments);
  return (typeof result === 'object' && result) || that;
}
```

Keys must be string

null and undefined are not ojects.

### undefined
a value that isn't even that
the default value for variables and parameters
the value of missing members in objects

### typeof
object -> 'object'
function -> 'function'
array -> 'object' ???
number -> 'number'
string -> 'string'
boolean -> 'boolean'
null -> 'object' ???
undefined -> 'undefined'

### Falsy values
- false
- null
- undefined
- ""
- 0
- NaN
-> all other values (including objects) are thruthy, e.g. "0", "false"

Thruthy means that if you put in if statemnt it will go in the then branch, otherwise falsy means that it will go in the else branch

### Reference
Objects can be passed as arguments to functions, and can be returned by functions
- Objects are passed by references
- Objects are not passed by value
The === operator compares object references, not values (true if only it is the same object)
Loosely typed: any of these can be stored in a variable, or passed as a parameter to any function, the language is not "untyped"

### Identifiers
- Starts with a letter or _ or $
- Followed by zero or more letters, digits, _ or $
- By convention all variables starts with lowercase letter
- Constructor functions start with uppercase letter
- Initial _ should be reserved for implementation
- $ should be reserved for machines

### With statement
```javascript
with(o) {
  foo = koda;
}
```

it can do any of them
- o.foo = koda;
- o.foo = o.koda;
- foo = koda;
- foo = o.koda;

- Ambiguous and error-prone: Don't use it

### Functions
- word function
- optional name
- parameters
  - wrapped in parens
  - Zero or more parameters
  - Separeted by comma
- body
  - wrapped in curly braces
  - Zero or more statements

### function expression
- Produce an instance of a function object
- Function object are first class
  - May be passed as an argument to a function
  - May be returned from a function
  - May assigned to a variable
  - May be stored in an object or array
- Function objects inherit from Function.prototype

### var statement
- declare and initializes variables within a function
- types are not specified
- a variable declared anywhere within a function is visible everywhere within the function
- the statement gets split into two parts:
  - the declaration part gets hoisted to the top of the function, initializing with undefined
  - the initialization part turns into an ordinary assignment

```javascript
var myvar = 0;
```

became

```javascript
var myvar = undefined;

myvar = 0;
```

### function statement
- the function statement is just a short-hand for a var statement with a function value

```javascript
function foo() {}
```

expands to

```javascript
var foo = function foo() {}
```

expands to

```javascript
var foo = undefined;
foo = function foo() {}
```

function statement is when function word is defined otherwise is a function expression (e.g. inside another function)
Only function has scope until es5.
For return statement if there is no expression, then the return value is undefined.
Excpet for constructors, whose default return value is this.
It has a special parameter called arguments, it is array-like object. Treat as read-only structure.

#### this
The this parameter contains a reference to the object of invocation.
this allows a method to know what object it is concerned with.
this allows a single function object to service many functions.
this is key to prototypal inheritance.

if a function is called with too many arguments, the extra args are ignored
if a function is called with few parameters, the remains will be setted to undefined
no type checking on parameters

### Invocation
four ways to call a function:
- Function form
  - functionObject(arguments)
- Method form
  - thisObject.methodName(arguments)
  - thisObject[methodName](arguments)
- Constructor form
  - new FunctionObject(arguments)
  - when a function is called with the new operator, a new object is created and assigned to this
  - if there is not an explicit return then this will be returned
- Apply form
  - function.apply(thisObject, [arguments])

They all attach this into function (or object) and the difference is in the function invocation (see below).

call attaches this into function and executes the function immediately:
```javascript
var person = {
  name: "James Smith",
  hello: function(thing) {
    console.log(this.name + " says hello " + thing);
  }
}

person.hello.call(person, "world"); // output: "James Smith says hello world"
```

bind attaches this into function and it needs to be invoked separately like this:
```javascript
var person = {  
  name: "James Smith",
  hello: function(thing) {
    console.log(this.name + " says hello " + thing);
  }
}

var helloFunc = person.hello.bind(person);
helloFunc("world");  // output: "James Smith says hello world"
```

apply is similar to call except that it takes an array-like object instead of a list the arguments.

### CSS
- Unhealthy separation of structure and presentation
- long fagile list of self-contradictory fules
- eah rule has two parts: selector and declaration
- difficult to understand. Difficult to use.

Five problems:
- Lack of modularity
- Selector management is complicated
- Declaration are too weak for modern web app
- Not intenden for dynamic content
- It is unimplementable. It's all about the quirks.

Document Object Model
- The "DOM"
- the browser's API
- document is the page
- document.body, document.head, document.documentElement is <html>
- each node has firstChild, lastChild, children
- each node has parent node
- each node has sibilings (nextSibiling, prevSibiling)
- each node has className, style
- each node has appendChild(new), insertBefore(new, sibiling), replaceChild(new, old), removeChild(old)
- When use removeChild be sure to remove any event handler
- Event Handles
  node.addEventListener(type, f, false);

CSS use hyphens, Javascript is camelCase

Bubbling: means that the event is given to the target, and then its parent, and then its paretnt, and so on until the event is canceled. To cancel propagation call `stopPropagation()`. To prevent default action (e.g. submiting a form) call `preventDefault()`.

```javascript
function walkTheDOM(node, func) {
  func(node);
  node = node.firstChild;
  while(node) {
    walkTheDOM(node, func);
    node = node.nextSibiling;
  }
}
```

Pre-order
1. Check if the current node is empty / null.
2. Display the data part of the root (or current node).
3. Traverse the left subtree by recursively calling the pre-order function.
4. Traverse the right subtree by recursively calling the pre-order function.

In-order
1. Check if the current node is empty / null.
2. Traverse the left subtree by recursively calling the in-order function.
3. Display the data part of the root (or current node).
4. Traverse the right subtree by recursively calling the in-order function.

Post-order
1. Check if the current node is empty / null.
2. Traverse the left subtree by recursively calling the post-order function.
3. Traverse the right subtree by recursively calling the post-order function.
4. Display the data part of the root (or current node).

flatten of array
flatten([[1], 2, 3, 4]) = [1, 2, 3, 4];
flatten([1, 2, 3, 4]) = [1, 2, 3, 4];
flatten([]) = [];
flatten(1) = 1;
flatten(null) = ?
flatten(undefined) = ?

```javascript
function flatten(array) {
  let newArray = [];
  for(const element of array) {
    if(Object.prototype.toString.call( element ) === '[object Array]') {
      const subElements = flatten(element);
      for(const subElement of subElements) {
        newArray.push(subElement);
      }
    } else {
      newArray.push(element);
    }
  }
  return newArray;
}
```