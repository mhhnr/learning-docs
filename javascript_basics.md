# Complete JavaScript Fundamentals Guide

## Table of Contents
1. [Introduction to JavaScript](#introduction-to-javascript)
2. [Basic Syntax](#basic-syntax)
3. [Variables and Data Types](#variables-and-data-types)
4. [Operators](#operators)
5. [Control Flow](#control-flow)
6. [Functions](#functions)
7. [Objects and Arrays](#objects-and-arrays)
8. [DOM Manipulation](#dom-manipulation)
9. [Error Handling](#error-handling)
10. [Modules](#modules)
11. [Object-Oriented Programming](#object-oriented-programming)
12. [Asynchronous JavaScript](#asynchronous-javascript)
13. [Built-in Objects and Methods](#built-in-objects-and-methods)
14. [Advanced Concepts](#advanced-concepts)
15. [Modern JavaScript (ES6+)](#modern-javascript-es6)
16. [Best Practices](#best-practices)

## Introduction to JavaScript

JavaScript is a high-level, interpreted programming language that was originally designed for web browsers but now runs in many environments. Created by Brendan Eich in 1995, JavaScript is one of the core technologies of the World Wide Web.

### Key Features:
- **Interpreted**: No compilation step needed
- **Dynamically typed**: Variable types are determined at runtime
- **First-class functions**: Functions can be assigned to variables, passed as arguments
- **Prototype-based**: Object-oriented programming through prototypes
- **Event-driven**: Responds to user interactions and system events
- **Multi-paradigm**: Supports procedural, object-oriented, and functional programming

```javascript
// Your first JavaScript program
console.log("Hello, World!");  // This prints "Hello, World!" to the console
```

## Basic Syntax

JavaScript syntax is flexible and forgiving, with automatic semicolon insertion and loose type checking.

### Statements and Semicolons
```javascript
// Semicolons are optional but recommended
let x = 5;
let y = 10;
console.log(x + y);

// Multiple statements on one line
let a = 1; let b = 2; let c = 3;

// Automatic semicolon insertion
let name = "John"  // Semicolon automatically inserted
console.log(name)
```

### Comments
```javascript
// This is a single-line comment

/*
This is a multi-line comment
that can span several lines
*/

/**
 * This is a JSDoc comment
 * Used for documentation
 * @param {string} name - The name parameter
 */
```

### Case Sensitivity
```javascript
// JavaScript is case-sensitive
let myVariable = 10;
let MyVariable = 20;  // Different variable
let MYVARIABLE = 30;  // Also different

console.log(myVariable);  // 10
console.log(MyVariable);  // 20
```

### Code Blocks
```javascript
// Code blocks are defined with curly braces
{
    let blockScoped = "I'm in a block";
    console.log(blockScoped);
}
// console.log(blockScoped);  // Error: blockScoped is not defined
```

## Variables and Data Types

### Variable Declarations
```javascript
// var (function-scoped, can be redeclared)
var oldStyle = "I'm var";
var oldStyle = "I can be redeclared";  // No error

// let (block-scoped, cannot be redeclared in same scope)
let modernStyle = "I'm let";
// let modernStyle = "Error!";  // SyntaxError

// const (block-scoped, cannot be reassigned)
const constant = "I cannot be reassigned";
// constant = "Error!";  // TypeError

// Variable naming rules:
// - Must start with letter, underscore, or dollar sign
// - Can contain letters, numbers, underscores, dollar signs
// - Case-sensitive
let validName = 1;
let _underscore = 2;
let $dollar = 3;
let camelCase = 4;
```

### Primitive Data Types

#### Numbers
```javascript
// All numbers are floating-point
let integer = 42;
let decimal = 3.14159;
let negative = -7;
let scientific = 1.23e-4;  // Scientific notation

// Special numeric values
let infinity = Infinity;
let negInfinity = -Infinity;
let notANumber = NaN;

// Number operations
console.log(5 / 0);        // Infinity
console.log(-5 / 0);       // -Infinity
console.log("hello" * 5);  // NaN
console.log(typeof NaN);   // "number" (surprisingly!)

// Checking for NaN
console.log(isNaN(NaN));           // true
console.log(Number.isNaN(NaN));    // true (preferred)
```

#### Strings
```javascript
// String creation
let singleQuoted = 'Hello';
let doubleQuoted = "World";
let templateLiteral = `Hello, ${doubleQuoted}!`;  // ES6 template literals

// String concatenation
let greeting = singleQuoted + " " + doubleQuoted;  // "Hello World"
let repeated = "Echo ".repeat(3);  // "Echo Echo Echo "

// String methods
let text = "JavaScript";
console.log(text.length);        // 10
console.log(text.toUpperCase()); // "JAVASCRIPT"
console.log(text.toLowerCase()); // "javascript"
console.log(text.charAt(0));     // "J"
console.log(text.indexOf("Script")); // 4
console.log(text.slice(0, 4));   // "Java"
console.log(text.substring(4));  // "Script"

// Template literals (ES6)
let name = "Alice";
let age = 25;
let message = `My name is ${name} and I'm ${age} years old.`;
console.log(message);  // "My name is Alice and I'm 25 years old."

// Multi-line strings with template literals
let multiLine = `
    This is a
    multi-line
    string
`;

// Escape sequences
let escaped = "She said, \"Hello!\"";  // "She said, "Hello!""
let newLine = "Line 1\nLine 2";
let tab = "Column 1\tColumn 2";
```

#### Booleans
```javascript
// Boolean values
let isTrue = true;
let isFalse = false;

// Boolean conversion (truthy/falsy values)
console.log(Boolean(1));        // true
console.log(Boolean(0));        // false
console.log(Boolean(""));       // false (empty string)
console.log(Boolean("hello"));  // true
console.log(Boolean(null));     // false
console.log(Boolean(undefined)); // false
console.log(Boolean([]));       // true (empty array)
console.log(Boolean({}));       // true (empty object)
```

#### null and undefined
```javascript
// undefined: variable declared but not assigned
let notAssigned;
console.log(notAssigned);  // undefined
console.log(typeof notAssigned);  // "undefined"

// null: intentional absence of value
let intentionallyEmpty = null;
console.log(intentionallyEmpty);  // null
console.log(typeof intentionallyEmpty);  // "object" (this is a known quirk)

// Checking for null and undefined
console.log(notAssigned == null);   // true (loose equality)
console.log(notAssigned === null);  // false (strict equality)
console.log(notAssigned === undefined);  // true
```

#### Symbols (ES6)
```javascript
// Symbols are unique identifiers
let sym1 = Symbol();
let sym2 = Symbol();
console.log(sym1 === sym2);  // false (each symbol is unique)

let sym3 = Symbol("description");
console.log(sym3.toString());  // "Symbol(description)"

// Symbols as object properties
let obj = {};
obj[sym1] = "value1";
obj[sym2] = "value2";
console.log(obj[sym1]);  // "value1"
```

#### BigInt (ES2020)
```javascript
// For integers larger than Number.MAX_SAFE_INTEGER
let bigNumber = 1234567890123456789012345678901234567890n;
let anotherBig = BigInt("1234567890123456789012345678901234567890");

console.log(typeof bigNumber);  // "bigint"
console.log(bigNumber + 1n);    // Need 'n' suffix for BigInt operations
```

### Type Checking
```javascript
// typeof operator
console.log(typeof 42);         // "number"
console.log(typeof "hello");    // "string"
console.log(typeof true);       // "boolean"
console.log(typeof undefined);  // "undefined"
console.log(typeof null);       // "object" (quirk!)
console.log(typeof {});         // "object"
console.log(typeof []);         // "object"
console.log(typeof function(){}); // "function"

// More precise type checking
console.log(Array.isArray([]));     // true
console.log(Array.isArray({}));     // false
console.log(null === null);         // true
```

## Operators

### Arithmetic Operators
```javascript
let a = 10, b = 3;

console.log(a + b);   // Addition: 13
console.log(a - b);   // Subtraction: 7
console.log(a * b);   // Multiplication: 30
console.log(a / b);   // Division: 3.3333...
console.log(a % b);   // Modulus (remainder): 1
console.log(a ** b);  // Exponentiation (ES2016): 1000

// Unary operators
console.log(+a);      // Unary plus: 10
console.log(-a);      // Unary minus: -10
console.log(++a);     // Pre-increment: 11 (a becomes 11)
console.log(a++);     // Post-increment: 11 (then a becomes 12)
console.log(--a);     // Pre-decrement: 11 (a becomes 11)
console.log(a--);     // Post-decrement: 11 (then a becomes 10)
```

### Assignment Operators
```javascript
let x = 10;

x += 5;   // x = x + 5;  (15)
x -= 3;   // x = x - 3;  (12)
x *= 2;   // x = x * 2;  (24)
x /= 4;   // x = x / 4;  (6)
x %= 5;   // x = x % 5;  (1)
x **= 3;  // x = x ** 3; (1)

// Logical assignment (ES2021)
let y = false;
y ||= 10;  // y = y || 10; (assigns if y is falsy)
y &&= 5;   // y = y && 5;  (assigns if y is truthy)
y ??= 7;   // y = y ?? 7;  (assigns if y is null/undefined)
```

### Comparison Operators
```javascript
let a = 10, b = "10", c = 5;

// Equality (loose - performs type coercion)
console.log(a == b);   // true (10 == "10")
console.log(a != c);   // true (10 != 5)

// Strict equality (no type coercion)
console.log(a === b);  // false (10 !== "10")
console.log(a !== b);  // true (different types)

// Relational operators
console.log(a > c);    // true
console.log(a < c);    // false
console.log(a >= 10);  // true
console.log(c <= 5);   // true
```

### Logical Operators
```javascript
let x = true, y = false;

// Logical AND
console.log(x && y);        // false
console.log(true && true);  // true

// Logical OR
console.log(x || y);         // true
console.log(false || false); // false

// Logical NOT
console.log(!x);  // false
console.log(!y);  // true

// Short-circuit evaluation
let result = x && someFunction();  // someFunction only called if x is true
let value = y || "default";       // "default" used if y is falsy
```

### Bitwise Operators
```javascript
let a = 5;   // Binary: 101
let b = 3;   // Binary: 011

console.log(a & b);   // Bitwise AND: 1 (001)
console.log(a | b);   // Bitwise OR: 7 (111)
console.log(a ^ b);   // Bitwise XOR: 6 (110)
console.log(~a);      // Bitwise NOT: -6
console.log(a << 1);  // Left shift: 10 (1010)
console.log(a >> 1);  // Right shift: 2 (010)
console.log(a >>> 1); // Unsigned right shift: 2
```

### Other Operators
```javascript
// Ternary operator
let age = 18;
let status = age >= 18 ? "adult" : "minor";
console.log(status);  // "adult"

// Nullish coalescing operator (ES2020)
let name = null;
let defaultName = name ?? "Anonymous";
console.log(defaultName);  // "Anonymous"

// Optional chaining operator (ES2020)
let user = { profile: { name: "John" } };
console.log(user?.profile?.name);     // "John"
console.log(user?.profile?.age);      // undefined
console.log(user?.nonexistent?.name); // undefined (no error)

// typeof operator
console.log(typeof "hello");  // "string"
console.log(typeof 42);       // "number"

// instanceof operator
console.log([] instanceof Array);   // true
console.log({} instanceof Object);  // true

// in operator
let obj = { name: "John", age: 30 };
console.log("name" in obj);     // true
console.log("address" in obj);  // false
```

## Control Flow

### Conditional Statements
```javascript
// if statement
let score = 85;
if (score >= 90) {
    console.log("Grade: A");
} else if (score >= 80) {
    console.log("Grade: B");
} else if (score >= 70) {
    console.log("Grade: C");
} else {
    console.log("Grade: F");
}

// Ternary operator
let grade = score >= 60 ? "Pass" : "Fail";

// switch statement
let day = "Monday";
switch (day) {
    case "Monday":
        console.log("Start of work week");
        break;
    case "Friday":
        console.log("TGIF!");
        break;
    case "Saturday":
    case "Sunday":
        console.log("Weekend!");
        break;
    default:
        console.log("Midweek");
}
```

### Loops

#### for Loop
```javascript
// Traditional for loop
for (let i = 0; i < 5; i++) {
    console.log(i);  // Prints 0, 1, 2, 3, 4
}

// for...in loop (iterates over object properties)
let person = { name: "John", age: 30, job: "developer" };
for (let key in person) {
    console.log(`${key}: ${person[key]}`);
}

// for...of loop (iterates over iterable values)
let fruits = ["apple", "banana", "cherry"];
for (let fruit of fruits) {
    console.log(fruit);
}

// for...of with index
for (let [index, fruit] of fruits.entries()) {
    console.log(`${index}: ${fruit}`);
}
```

#### while Loop
```javascript
// while loop
let count = 0;
while (count < 5) {
    console.log(count);
    count++;
}

// do...while loop (executes at least once)
let x = 0;
do {
    console.log(x);
    x++;
} while (x < 3);
```

### Loop Control
```javascript
// break statement
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        break;  // Exit the loop
    }
    console.log(i);  // Prints 0, 1, 2, 3, 4
}

// continue statement
for (let i = 0; i < 5; i++) {
    if (i === 2) {
        continue;  // Skip the rest of this iteration
    }
    console.log(i);  // Prints 0, 1, 3, 4
}

// Labels (rarely used)
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) {
            break outer;  // Break out of both loops
        }
        console.log(`${i}, ${j}`);
    }
}
```

## Functions

### Function Declarations
```javascript
// Function declaration (hoisted)
function greet(name) {
    return `Hello, ${name}!`;
}

console.log(greet("Alice"));  // "Hello, Alice!"

// Function expression
const greetExpression = function(name) {
    return `Hello, ${name}!`;
};

// Arrow functions (ES6)
const greetArrow = (name) => {
    return `Hello, ${name}!`;
};

// Concise arrow function (single expression)
const greetConcise = name => `Hello, ${name}!`;

// Arrow function with multiple parameters
const add = (a, b) => a + b;

// Arrow function with no parameters
const sayHello = () => "Hello!";
```

### Parameters and Arguments
```javascript
// Default parameters (ES6)
function greet(name = "World", greeting = "Hello") {
    return `${greeting}, ${name}!`;
}

console.log(greet());               // "Hello, World!"
console.log(greet("Alice"));        // "Hello, Alice!"
console.log(greet("Bob", "Hi"));    // "Hi, Bob!"

// Rest parameters (variable number of arguments)
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4));  // 10

// Spread operator in function calls
let numbers = [1, 2, 3, 4, 5];
console.log(Math.max(...numbers));  // 5

// Destructuring parameters
function describePerson({name, age, job = "unemployed"}) {
    return `${name} is ${age} years old and works as a ${job}.`;
}

let person = { name: "Alice", age: 30, job: "developer" };
console.log(describePerson(person));
```

### Function Scope and Closures
```javascript
// Global scope
let globalVar = "I'm global";

function outerFunction(x) {
    // Function scope
    let outerVar = "I'm in outer function";
    
    function innerFunction(y) {
        // Inner function has access to outer variables (closure)
        let innerVar = "I'm in inner function";
        console.log(globalVar);  // Accessible
        console.log(outerVar);   // Accessible
        console.log(innerVar);   // Accessible
        return x + y;
    }
    
    return innerFunction;
}

// Closure example
function createCounter() {
    let count = 0;
    return function() {
        count++;
        return count;
    };
}

let counter1 = createCounter();
let counter2 = createCounter();

console.log(counter1());  // 1
console.log(counter1());  // 2
console.log(counter2());  // 1 (independent counter)
```

### Higher-Order Functions
```javascript
// Functions that take other functions as arguments
function calculate(operation, a, b) {
    return operation(a, b);
}

const add = (x, y) => x + y;
const multiply = (x, y) => x * y;

console.log(calculate(add, 5, 3));      // 8
console.log(calculate(multiply, 4, 6)); // 24

// Functions that return other functions
function createMultiplier(multiplier) {
    return function(x) {
        return x * multiplier;
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5));  // 10
console.log(triple(4));  // 12
```

### IIFE (Immediately Invoked Function Expression)
```javascript
// IIFE pattern
(function() {
    let privateVar = "I'm private";
    console.log("IIFE executed!");
})();

// IIFE with parameters
(function(name) {
    console.log(`Hello, ${name}!`);
})("World");

// Arrow function IIFE
(() => {
    console.log("Arrow IIFE executed!");
})();
```

### Recursion
```javascript
// Factorial function
function factorial(n) {
    if (n <= 1) {
        return 1;  // Base case
    }
    return n * factorial(n - 1);  // Recursive case
}

console.log(factorial(5));  // 120

// Fibonacci sequence
function fibonacci(n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log(fibonacci(8));  // 21
```

## Objects and Arrays

### Objects
```javascript
// Object creation
let emptyObject = {};
let person = {
    name: "John",
    age: 30,
    job: "developer",
    isEmployed: true
};

// Accessing properties
console.log(person.name);        // Dot notation
console.log(person["age"]);      // Bracket notation
console.log(person["is" + "Employed"]);  // Dynamic property names

// Adding/modifying properties
person.salary = 50000;
person["location"] = "New York";
person.age = 31;  // Modify existing property

// Deleting properties
delete person.isEmployed;

// Methods in objects
let calculator = {
    add: function(a, b) {
        return a + b;
    },
    // ES6 method shorthand
    subtract(a, b) {
        return a - b;
    },
    // Arrow function (be careful with 'this')
    multiply: (a, b) => a * b
};

console.log(calculator.add(5, 3));       // 8
console.log(calculator.subtract(10, 4)); // 6
```

### Object Methods and Properties
```javascript
let person = { name: "Alice", age: 25, job: "designer" };

// Object.keys() - get all property names
console.log(Object.keys(person));  // ["name", "age", "job"]

// Object.values() - get all values
console.log(Object.values(person));  // ["Alice", 25, "designer"]

// Object.entries() - get key-value pairs
console.log(Object.entries(person));  // [["name", "Alice"], ["age", 25], ["job", "designer"]]

// Object.assign() - copy properties
let copy = Object.assign({}, person);
let extended = Object.assign({}, person, { salary: 60000 });

// Object.freeze() - make object immutable
Object.freeze(person);
// person.age = 26;  // Won't work in strict mode

// Object.seal() - prevent adding/deleting properties
let sealedObj = { a: 1, b: 2 };
Object.seal(sealedObj);
sealedObj.a = 10;  // Can modify existing properties
// sealedObj.c = 3;  // Can't add new properties

// hasOwnProperty() - check if property exists
console.log(person.hasOwnProperty("name"));  // true
console.log(person.hasOwnProperty("toString"));  // false (inherited)
```

### Arrays
```javascript
// Array creation
let emptyArray = [];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, "hello", true, null, {name: "John"}];
let arrayFromRange = Array.from({length: 5}, (_, i) => i + 1);  // [1, 2, 3, 4, 5]

// Accessing elements
console.log(numbers[0]);    // First element: 1
console.log(numbers[-1]);   // undefined (no negative indexing in JS)
console.log(numbers[numbers.length - 1]);  // Last element: 5

// Array properties
console.log(numbers.length);  // 5

// Modifying arrays
numbers.push(6);           // Add to end: [1, 2, 3, 4, 5, 6]
numbers.pop();             // Remove from end: [1, 2, 3, 4, 5]
numbers.unshift(0);        // Add to beginning: [0, 1, 2, 3, 4, 5]
numbers.shift();           // Remove from beginning: [1, 2, 3, 4, 5]

// Splice method (add/remove elements at any position)
numbers.splice(2, 1);      // Remove 1 element at index 2: [1, 2, 4, 5]
numbers.splice(2, 0, 3);   // Insert 3 at index 2: [1, 2, 3, 4, 5]
numbers.splice(1, 2, "a", "b");  // Replace 2 elements: [1, "a", "b", 4, 5]
```

### Array Methods
```javascript
let numbers = [1, 2, 3, 4, 5];
let words = ["apple", "banana", "cherry"];

// Iteration methods
numbers.forEach((num, index) => {
    console.log(`${index}: ${num}`);
});

// Map - transform each element
let doubled = numbers.map(num => num * 2);  // [2, 4, 6, 8, 10]

// Filter - select elements that meet condition
let evens = numbers.filter(num => num % 2 === 0);  // [2, 4]

// Reduce - accumulate values
let sum = numbers.reduce((total, num) => total + num, 0);  // 15
let product = numbers.reduce((total, num) => total * num, 1);  // 120

// Find - get first element that meets condition
let found = numbers.find(num => num > 3);  // 4

// Some - check if any element meets condition
let hasEven = numbers.some(num => num % 2 === 0);  // true

// Every - check if all elements meet condition
let allPositive = numbers.every(num => num > 0);  // true

// Sort
let sorted = words.sort();  // ["apple", "banana", "cherry"]
let sortedNumbers = numbers.sort((a, b) => b - a);  // [5, 4, 3, 2, 1] (descending)

// Join - convert array to string
let joined = words.join(", ");  // "apple, banana, cherry"

// Slice - extract portion of array (doesn't modify original)
let portion = numbers.slice(1, 4);  // [2, 3, 4]

// Concat - combine arrays
let combined = numbers.concat([6, 7, 8]);  // [1, 2, 3, 4, 5, 6, 7, 8]

// Includes - check if array contains element
let hasThree = numbers.includes(3);  // true

// IndexOf - find index of element
let index = numbers.indexOf(3);  // 2

// Reverse - reverse array in place
numbers.reverse();  // [5, 4, 3, 2, 1]
```

### Array Destructuring (ES6)
```javascript
let numbers = [1, 2, 3, 4, 5];

// Basic destructuring
let [first, second] = numbers;
console.log(first);   // 1
console.log(second);  // 2

// Skip elements
let [a, , c] = numbers;  // Skip the second element
console.log(a);  // 1
console.log(c);  // 3

// Rest operator in destructuring
let [head, ...tail] = numbers;
console.log(head);  // 1
console.log(tail);  // [2, 3, 4, 5]

// Default values
let [x, y, z = 0] = [1, 2];
console.log(z);  // 0 (default value)

// Swapping variables
let p = 1, q = 2;
[p, q] = [q, p];
console.log(p, q);  // 2 1
```

### Object Destructuring (ES6)
```javascript
let person = { name: "Alice", age: 30, job: "developer" };

// Basic destructuring
let { name, age } = person;
console.log(name);  // "Alice"
console.log(age);   // 30

// Rename variables
let { name: personName, job: occupation } = person;
console.log(personName);  // "Alice"
console.log(occupation);  // "developer"

// Default values
let { name: n, salary = 50000 } = person;
console.log(salary);  // 50000 (default value)

// Nested destructuring
let employee = {
    info: { name: "Bob", age: 25 },
    position: { title: "Manager", department: "Sales" }
};

let { info: { name: empName }, position: { title } } = employee;
console.log(empName);  // "Bob"
console.log(title);    // "Manager"

// Function parameter destructuring
function greetPerson({ name, age }) {
    return `Hello, ${name}! You are ${age} years old.`;
}

console.log(greetPerson(person));  // "Hello, Alice! You are 30 years old."
```

## DOM Manipulation

### Selecting Elements
```javascript
// Select by ID
let elementById = document.getElementById("myId");

// Select by class name
let elementsByClass = document.getElementsByClassName("myClass");
let firstByClass = elementsByClass[0];

// Select by tag name
let elementsByTag = document.getElementsByTagName("div");

// Query selectors (CSS-style selectors)
let firstElement = document.querySelector(".myClass");  // First match
let allElements = document.querySelectorAll(".myClass"); // All matches

// More complex selectors
let specificElement = document.querySelector("#container .item:first-child");
let multipleElements = document.querySelectorAll("div.container > p");
```

### Modifying Content
```javascript
let element = document.getElementById("myElement");

// Text content
element.textContent = "New text content";
console.log(element.textContent);

// HTML content
element.innerHTML = "<strong>Bold text</strong>";

// Attributes
element.setAttribute("data-value", "123");
let value = element.getAttribute("data-value");
element.removeAttribute("data-value");

// Classes
element.classList.add("newClass");
element.classList.remove("oldClass");
element.classList.toggle("active");
element.classList.contains("myClass");  // Returns boolean

// Styles
element.style.color = "red";
element.style.backgroundColor = "yellow";
element.style.fontSize = "16px";
```

### Creating and Modifying Elements
```javascript
// Create new elements
let newDiv = document.createElement("div");
newDiv.textContent = "I'm a new div";
newDiv.className = "dynamic-element";

// Append to parent
let container = document.getElementById("container");
container.appendChild(newDiv);

// Insert at specific position
let firstChild = container.firstChild;
container.insertBefore(newDiv, firstChild);

// Remove elements
let elementToRemove = document.getElementById("removeMe");
elementToRemove.parentNode.removeChild(elementToRemove);
// Or in modern browsers:
// elementToRemove.remove();

// Clone elements
let original = document.getElementById("original");
let clone = original.cloneNode(true);  // true = deep clone (including children)
container.appendChild(clone);
```

### Event Handling
```javascript
// Add event listeners
let button = document.getElementById("myButton");

// Method 1: addEventListener (preferred)
button.addEventListener("click", function(event) {
    console.log("Button clicked!");
    console.log(event.target);  // The element that triggered the event
});

// Method 2: Arrow function
button.addEventListener("click", (e) => {
    e.preventDefault();  // Prevent default behavior
    console.log("Arrow function click handler");
});

// Method 3: Direct assignment (overwrites previous handlers)
button.onclick = function() {
    console.log("Direct assignment click handler");
};

// Common events
let input = document.getElementById("myInput");

input.addEventListener("focus", () => console.log("Input focused"));
input.addEventListener("blur", () => console.log("Input lost focus"));
input.addEventListener("change", (e) => console.log("Value changed:", e.target.value));
input.addEventListener("keydown", (e) => {
    if (e.key === "Enter") {
        console.log("Enter key pressed");
    }
});

// Mouse events
let div = document.getElementById("myDiv");
div.addEventListener("mouseenter", () => console.log("Mouse entered"));
div.addEventListener("mouseleave", () => console.log("Mouse left"));
div.addEventListener("mouseover", () => console.log("Mouse over"));

// Form events
let form = document.getElementById("myForm");
form.addEventListener("submit", (e) => {
    e.preventDefault();  // Prevent form submission
    console.log("Form submitted");
});

// Remove event listeners
function clickHandler() {
    console.log("Click handled");
}
button.addEventListener("click", clickHandler);
button.removeEventListener("click", clickHandler);
```

### Event Delegation
```javascript
// Event delegation - handling events on parent for child elements
let list = document.getElementById("myList");

list.addEventListener("click", function(e) {
    if (e.target.tagName === "LI") {
        console.log("List item clicked:", e.target.textContent);
        e.target.classList.toggle("selected");
    }
});

// This works even for dynamically added list items
let newItem = document.createElement("li");
newItem.textContent = "Dynamic item";
list.appendChild(newItem);  // Will also respond to clicks
```

## Error Handling

### Try-Catch-Finally
```javascript
// Basic try-catch
try {
    let result = riskyOperation();
    console.log(result);
} catch (error) {
    console.error("An error occurred:", error.message);
}

// Specific error types
try {
    JSON.parse("invalid json");
} catch (error) {
    if (error instanceof SyntaxError) {
        console.log("JSON syntax error");
    } else if (error instanceof ReferenceError) {
        console.log("Reference error");
    } else {
        console.log("Unknown error:", error);
    }
}

// Try-catch-finally
try {
    // Risky code
    let data = JSON.parse(jsonString);
    processData(data);
} catch (error) {
    console.error("Processing failed:", error.message);
} finally {
    // Always executed
    console.log("Cleanup operations");
}
```

### Throwing Custom Errors
```javascript
function validateAge(age) {
    if (typeof age !== "number") {
        throw new TypeError("Age must be a number");
    }
    if (age < 0) {
        throw new RangeError("Age cannot be negative");
    }
    if (age > 150) {
        throw new RangeError("Age seems unrealistic");
    }
    return true;
}

try {
    validateAge("25");  // Will throw TypeError
} catch (error) {
    console.log(error.name);     // "TypeError"
    console.log(error.message);  // "Age must be a number"
}

// Custom error classes
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}

function validateEmail(email) {
    if (!email.includes("@")) {
        throw new ValidationError("Invalid email format");
    }
}

try {
    validateEmail("invalid-email");
} catch (error) {
    if (error instanceof ValidationError) {
        console.log("Validation failed:", error.message);
    }
}
```

### Error Types
```javascript
// Common JavaScript error types

// SyntaxError - code syntax problems
try {
    eval("const x =");  // Invalid syntax
} catch (e) {
    console.log(e instanceof SyntaxError);  // true
}

// ReferenceError - undefined variables
try {
    console.log(undefinedVariable);
} catch (e) {
    console.log(e instanceof ReferenceError);  // true
}

// TypeError - wrong type operations
try {
    null.someProperty;
} catch (e) {
    console.log(e instanceof TypeError);  // true
}

// RangeError - number out of range
try {
    let arr = new Array(-1);  // Invalid array length
} catch (e) {
    console.log(e instanceof RangeError);  // true
}
```

## Modules

### ES6 Modules (Import/Export)
```javascript
// math.js - Named exports
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

export const PI = 3.14159;

// Alternative syntax
function multiply(a, b) {
    return a * b;
}

function divide(a, b) {
    return a / b;
}

export { multiply, divide };

// Default export
export default function calculator(operation, a, b) {
    switch (operation) {
        case "add": return add(a, b);
        case "subtract": return subtract(a, b);
        case "multiply": return multiply(a, b);
        case "divide": return divide(a, b);
        default: throw new Error("Unknown operation");
    }
}
```

```javascript
// main.js - Importing modules
// Named imports
import { add, subtract, PI } from "./math.js";

// Import with alias
import { multiply as mult, divide as div } from "./math.js";

// Import default
import calculator from "./math.js";

// Import everything
import * as MathUtils from "./math.js";

// Using imports
console.log(add(5, 3));           // 8
console.log(PI);                  // 3.14159
console.log(mult(4, 6));          // 24
console.log(calculator("add", 2, 3)); // 5
console.log(MathUtils.subtract(10, 4)); // 6

// Dynamic imports (ES2020)
async function loadMath() {
    const mathModule = await import("./math.js");
    console.log(mathModule.add(1, 2));
}

// Conditional imports
if (someCondition) {
    import("./conditionalModule.js").then(module => {
        module.doSomething();
    });
}
```

### CommonJS (Node.js)
```javascript
// math.js - CommonJS exports
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract,
    PI: 3.14159
};

// Or individual exports
exports.multiply = function(a, b) {
    return a * b;
};
```

```javascript
// main.js - CommonJS imports
const { add, subtract, PI } = require("./math.js");
const math = require("./math.js");

console.log(add(5, 3));      // 8
console.log(math.subtract(10, 4)); // 6
```

## Object-Oriented Programming

### Constructor Functions (ES5 Style)
```javascript
// Constructor function
function Person(name, age) {
    this.name = name;
    this.age = age;
}

// Adding methods to prototype
Person.prototype.greet = function() {
    return `Hello, my name is ${this.name}`;
};

Person.prototype.celebrateBirthday = function() {
    this.age++;
    return `${this.name} is now ${this.age} years old`;
};

// Creating instances
let person1 = new Person("Alice", 30);
let person2 = new Person("Bob", 25);

console.log(person1.greet());  // "Hello, my name is Alice"
console.log(person2.celebrateBirthday());  // "Bob is now 26 years old"
```

### ES6 Classes
```javascript
// Class declaration
class Person {
    // Constructor
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    // Instance methods
    greet() {
        return `Hello, my name is ${this.name}`;
    }
    
    celebrateBirthday() {
        this.age++;
        return `${this.name} is now ${this.age} years old`;
    }
    
    // Static methods
    static species() {
        return "Homo sapiens";
    }
    
    // Getter
    get info() {
        return `${this.name} (${this.age})`;
    }
    
    // Setter
    set age(newAge) {
        if (newAge >= 0) {
            this._age = newAge;
        }
    }
    
    get age() {
        return this._age;
    }
}

// Using the class
let person = new Person("Alice", 30);
console.log(person.greet());        // "Hello, my name is Alice"
console.log(person.info);           // "Alice (30)" (getter)
console.log(Person.species());      // "Homo sapiens" (static method)
```

### Inheritance
```javascript
// Parent class
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        return `${this.name} makes a sound`;
    }
    
    move() {
        return `${this.name} moves`;
    }
}

// Child class
class Dog extends Animal {
    constructor(name, breed) {
        super(name);  // Call parent constructor
        this.breed = breed;
    }
    
    // Override parent method
    speak() {
        return `${this.name} barks`;
    }
    
    // New method
    fetch() {
        return `${this.name} fetches the ball`;
    }
    
    // Call parent method
    describe() {
        return `${super.speak()} and ${this.move()}`;
    }
}

// Multiple inheritance simulation with mixins
const CanFly = {
    fly() {
        return `${this.name} flies`;
    }
};

const CanSwim = {
    swim() {
        return `${this.name} swims`;
    }
};

class Duck extends Animal {
    constructor(name) {
        super(name);
        // Apply mixins
        Object.assign(this, CanFly, CanSwim);
    }
    
    speak() {
        return `${this.name} quacks`;
    }
}

// Using inheritance
let dog = new Dog("Rex", "Golden Retriever");
console.log(dog.speak());    // "Rex barks"
console.log(dog.fetch());    // "Rex fetches the ball"
console.log(dog.describe()); // "Rex makes a sound and Rex moves"

let duck = new Duck("Donald");
console.log(duck.fly());     // "Donald flies"
console.log(duck.swim());    // "Donald swims"
```

### Encapsulation and Private Fields
```javascript
// Private fields (ES2022)
class BankAccount {
    #balance = 0;  // Private field
    #accountNumber;
    
    constructor(owner, accountNumber) {
        this.owner = owner;
        this.#accountNumber = accountNumber;
    }
    
    // Public method to access private field
    getBalance() {
        return this.#balance;
    }
    
    deposit(amount) {
        if (amount > 0) {
            this.#balance += amount;
            return `Deposited ${amount}. New balance: ${this.#balance}`;
        }
        throw new Error("Deposit amount must be positive");
    }
    
    withdraw(amount) {
        if (amount > 0 && amount <= this.#balance) {
            this.#balance -= amount;
            return `Withdrew ${amount}. New balance: ${this.#balance}`;
        }
        throw new Error("Invalid withdrawal amount");
    }
    
    // Private method
    #validateTransaction(amount) {
        return amount > 0 && amount <= this.#balance;
    }
}

let account = new BankAccount("Alice", "12345");
console.log(account.deposit(1000));    // "Deposited $1000. New balance: $1000"
console.log(account.withdraw(500));    // "Withdrew $500. New balance: $500"
console.log(account.getBalance());     // 500

// console.log(account.#balance);      // SyntaxError: Private field '#balance' must be declared in an enclosing class
```

### Prototypes and Prototype Chain
```javascript
// Understanding prototypes
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    return `${this.name} makes a sound`;
};

function Dog(name, breed) {
    Animal.call(this, name);  // Call parent constructor
    this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
    return `${this.name} barks`;
};

let dog = new Dog("Rex", "Golden Retriever");
console.log(dog.speak());  // "Rex makes a sound" (inherited)
console.log(dog.bark());   // "Rex barks" (own method)

// Prototype chain
console.log(dog.__proto__ === Dog.prototype);                    // true
console.log(Dog.prototype.__proto__ === Animal.prototype);       // true
console.log(Animal.prototype.__proto__ === Object.prototype);    // true
console.log(Object.prototype.__proto__ === null);               // true
```

## Asynchronous JavaScript

### Callbacks
```javascript
// Basic callback
function fetchData(callback) {
    setTimeout(() => {
        const data = { id: 1, name: "John" };
        callback(data);
    }, 1000);
}

fetchData((data) => {
    console.log("Data received:", data);
});

// Callback with error handling
function fetchDataWithError(callback) {
    setTimeout(() => {
        const success = Math.random() > 0.5;
        if (success) {
            callback(null, { id: 1, name: "John" });
        } else {
            callback(new Error("Failed to fetch data"), null);
        }
    }, 1000);
}

fetchDataWithError((error, data) => {
    if (error) {
        console.error("Error:", error.message);
    } else {
        console.log("Data:", data);
    }
});

// Callback hell example
getData((data1) => {
    processData(data1, (processedData1) => {
        getData2(processedData1, (data2) => {
            processData2(data2, (finalResult) => {
                console.log(finalResult);
            });
        });
    });
});
```

### Promises
```javascript
// Creating a Promise
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const success = Math.random() > 0.3;
            if (success) {
                resolve({ id: 1, name: "John" });
            } else {
                reject(new Error("Failed to fetch data"));
            }
        }, 1000);
    });
}

// Using Promises with .then() and .catch()
fetchData()
    .then(data => {
        console.log("Data received:", data);
        return data.id;
    })
    .then(id => {
        console.log("Processing ID:", id);
        return `Processed: ${id}`;
    })
    .then(result => {
        console.log("Final result:", result);
    })
    .catch(error => {
        console.error("Error:", error.message);
    })
    .finally(() => {
        console.log("Operation completed");
    });

// Promise.all() - wait for all promises
const promise1 = Promise.resolve(3);
const promise2 = new Promise(resolve => setTimeout(() => resolve("foo"), 1000));
const promise3 = Promise.resolve(42);

Promise.all([promise1, promise2, promise3])
    .then(values => {
        console.log(values);  // [3, "foo", 42]
    });

// Promise.race() - first promise to resolve/reject
const promiseA = new Promise(resolve => setTimeout(() => resolve("A"), 1000));
const promiseB = new Promise(resolve => setTimeout(() => resolve("B"), 500));

Promise.race([promiseA, promiseB])
    .then(value => {
        console.log(value);  // "B" (resolves first)
    });

// Promise.allSettled() - wait for all to settle (resolve or reject)
const promises = [
    Promise.resolve("Success"),
    Promise.reject("Error"),
    Promise.resolve("Another success")
];

Promise.allSettled(promises)
    .then(results => {
        results.forEach((result, index) => {
            if (result.status === "fulfilled") {
                console.log(`Promise ${index} succeeded:`, result.value);
            } else {
                console.log(`Promise ${index} failed:`, result.reason);
            }
        });
    });
```

### Async/Await
```javascript
// Async function declaration
async function fetchUserData(userId) {
    try {
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        return userData;
    } catch (error) {
        console.error("Failed to fetch user data:", error);
        throw error;
    }
}

// Using async/await
async function displayUserInfo() {
    try {
        const user = await fetchUserData(123);
        console.log("User:", user.name);
        
        const posts = await fetchUserPosts(user.id);
        console.log("Posts count:", posts.length);
    } catch (error) {
        console.error("Error displaying user info:", error);
    }
}

// Async arrow function
const getData = async () => {
    const data = await fetchData();
    return data;
};

// Parallel execution with async/await
async function fetchMultipleUsers() {
    try {
        // Sequential (slower)
        const user1 = await fetchUserData(1);
        const user2 = await fetchUserData(2);
        
        // Parallel (faster)
        const [user3, user4] = await Promise.all([
            fetchUserData(3),
            fetchUserData(4)
        ]);
        
        return [user1, user2, user3, user4];
    } catch (error) {
        console.error("Error fetching users:", error);
    }
}

// Error handling in async/await
async function riskyOperation() {
    try {
        const result1 = await operationThatMightFail();
        const result2 = await anotherRiskyOperation(result1);
        return result2;
    } catch (error) {
        if (error instanceof NetworkError) {
            console.log("Network error, retrying...");
            return await riskyOperation();  // Retry
        } else {
            console.error("Operation failed:", error);
            throw error;
        }
    }
}
```

### Fetch API
```javascript
// Basic GET request
fetch("/api/data")
    .then(response => {
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
    })
    .then(data => console.log(data))
    .catch(error => console.error("Fetch error:", error));

// POST request with JSON
fetch("/api/users", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
    },
    body: JSON.stringify({
        name: "John Doe",
        email: "john@example.com"
    })
})
.then(response => response.json())
.then(data => console.log("User created:", data));

// Async/await with fetch
async function fetchUserData(userId) {
    try {
        const response = await fetch(`/api/users/${userId}`);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const contentType = response.headers.get("content-type");
        if (contentType && contentType.includes("application/json")) {
            return await response.json();
        } else {
            return await response.text();
        }
    } catch (error) {
        console.error("Error fetching user data:", error);
        throw error;
    }
}

// File upload
async function uploadFile(file) {
    const formData = new FormData();
    formData.append("file", file);
    
    try {
        const response = await fetch("/api/upload", {
            method: "POST",
            body: formData
        });
        
        return await response.json();
    } catch (error) {
        console.error("Upload failed:", error);
    }
}
```

## Built-in Objects and Methods

### String Methods
```javascript
let text = "Hello, JavaScript World!";

// Basic methods
console.log(text.length);                    // 23
console.log(text.charAt(7));                 // "J"
console.log(text.charCodeAt(0));            // 72 (Unicode value of 'H')
console.log(text.indexOf("Script"));         // 11
console.log(text.lastIndexOf("o"));          // 20
console.log(text.includes("Java"));          // true
console.log(text.startsWith("Hello"));       // true
console.log(text.endsWith("World!"));        // true

// Case methods
console.log(text.toLowerCase());             // "hello, javascript world!"
console.log(text.toUpperCase());             // "HELLO, JAVASCRIPT WORLD!"

// Extraction methods
console.log(text.slice(7, 17));             // "JavaScript"
console.log(text.substring(7, 17));         // "JavaScript"
console.log(text.substr(7, 10));            // "JavaScript" (deprecated)

// Replace methods
console.log(text.replace("JavaScript", "JS")); // "Hello, JS World!"
console.log(text.replaceAll("o", "0"));      // "Hell0, JavaScript W0rld!"

// Split and join
let words = text.split(" ");                 // ["Hello,", "JavaScript", "World!"]
let rejoined = words.join("-");              // "Hello,-JavaScript-World!"

// Trim methods
let padded = "  Hello World  ";
console.log(padded.trim());                  // "Hello World"
console.log(padded.trimStart());             // "Hello World  "
console.log(padded.trimEnd());               // "  Hello World"

// Padding methods (ES2017)
let num = "5";
console.log(num.padStart(3, "0"));           // "005"
console.log(num.padEnd(3, "0"));             // "500"

// Repeat method
console.log("Ha".repeat(3));                 // "HaHaHa"
```

### Array Methods (Advanced)
```javascript
let numbers = [1, 2, 3, 4, 5];
let people = [
    { name: "Alice", age: 30 },
    { name: "Bob", age: 25 },
    { name: "Charlie", age: 35 }
];

// Reduce examples
let sum = numbers.reduce((acc, num) => acc + num, 0);           // 15
let product = numbers.reduce((acc, num) => acc * num, 1);       // 120
let max = numbers.reduce((acc, num) => Math.max(acc, num));     // 5

// Group by age (manual implementation)
let groupedByAge = people.reduce((acc, person) => {
    let age = person.age;
    if (!acc[age]) {
        acc[age] = [];
    }
    acc[age].push(person);
    return acc;
}, {});

// Flatten arrays
let nested = [[1, 2], [3, 4], [5, 6]];
let flattened = nested.reduce((acc, arr) => acc.concat(arr), []); // [1, 2, 3, 4, 5, 6]
// Or use flat() method
let flattenedES2019 = nested.flat();                              // [1, 2, 3, 4, 5, 6]

// Deep flatten
let deepNested = [1, [2, [3, [4, 5]]]];
let deepFlattened = deepNested.flat(Infinity);                     // [1, 2, 3, 4, 5]

// FlatMap (ES2019)
let sentences = ["Hello world", "How are you"];
let words = sentences.flatMap(sentence => sentence.split(" "));    // ["Hello", "world", "How", "are", "you"]

// Array.from() examples
let arrayFromString = Array.from("Hello");                         // ["H", "e", "l", "l", "o"]
let arrayFromSet = Array.from(new Set([1, 2, 2, 3]));             // [1, 2, 3]
let mappedArray = Array.from({length: 5}, (_, i) => i * 2);       // [0, 2, 4, 6, 8]
```

### Math Object
```javascript
// Constants
console.log(Math.PI);       // 3.141592653589793
console.log(Math.E);        // 2.718281828459045

// Rounding methods
console.log(Math.round(4.7));    // 5
console.log(Math.round(4.4));    // 4
console.log(Math.ceil(4.1));     // 5 (round up)
console.log(Math.floor(4.9));    // 4 (round down)
console.log(Math.trunc(4.9));    // 4 (remove decimal part)

// Min and max
console.log(Math.min(1, 3, 2));      // 1
console.log(Math.max(1, 3, 2));      // 3
console.log(Math.min(...numbers));   // 1 (using spread operator)

// Power and roots
console.log(Math.pow(2, 3));      // 8
console.log(Math.sqrt(16));       // 4
console.log(Math.cbrt(27));       // 3 (cube root)

// Absolute value
console.log(Math.abs(-5));        // 5

// Random numbers
console.log(Math.random());                           // Random between 0 and 1
console.log(Math.floor(Math.random() * 10));         // Random integer 0-9
console.log(Math.floor(Math.random() * 11));         // Random integer 0-10

// Random between min and max (inclusive)
function randomBetween(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

// Trigonometric functions
console.log(Math.sin(Math.PI / 2));   // 1
console.log(Math.cos(Math.PI));       // -1
console.log(Math.tan(Math.PI / 4));   // 1

// Logarithmic functions
console.log(Math.log(Math.E));        // 1 (natural log)
console.log(Math.log10(100));         // 2 (base 10 log)
console.log(Math.log2(8));            // 3 (base 2 log)
```

### Date Object
```javascript
// Creating dates
let now = new Date();                           // Current date and time
let specific = new Date(2023, 11, 25);          // Year, month (0-indexed), day
let fromString = new Date("2023-12-25");        // ISO string
let fromTimestamp = new Date(1672617600000);    // Milliseconds since epoch

// Getting date components
console.log(now.getFullYear());     // Current year
console.log(now.getMonth());        // Month (0-11)
console.log(now.getDate());         // Day of month (1-31)
console.log(now.getDay());          // Day of week (0-6, Sunday = 0)
console.log(now.getHours());        // Hours (0-23)
console.log(now.getMinutes());      // Minutes (0-59)
console.log(now.getSeconds());      // Seconds (0-59)
console.log(now.getMilliseconds()); // Milliseconds (0-999)

// Setting date components
let date = new Date();
date.setFullYear(2024);
date.setMonth(5);          // June (0-indexed)
date.setDate(15);
date.setHours(14, 30, 0);  // 2:30 PM

// Formatting dates
console.log(date.toString());           // Full date string
console.log(date.toDateString());       // Date portion only
console.log(date.toTimeString());       // Time portion only
console.log(date.toISOString());        // ISO format
console.log(date.toLocaleDateString()); // Locale-specific date
console.log(date.toLocaleTimeString()); // Locale-specific time

// Date arithmetic
let tomorrow = new Date();
tomorrow.setDate(tomorrow.getDate() + 1);

let nextWeek = new Date();
nextWeek.setDate(nextWeek.getDate() + 7);

// Time difference
let start = new Date("2023-01-01");
let end = new Date("2023-12-31");
let diffInMs = end - start;
let diffInDays = diffInMs / (1000 * 60 * 60 * 24);

// Static methods
console.log(Date.now());                    // Current timestamp
console.log(Date.parse("2023-12-25"));      // Parse date string to timestamp
```

### JSON Object
```javascript
// JavaScript object
let person = {
    name: "John Doe",
    age: 30,
    hobbies: ["reading", "swimming"],
    address: {
        street: "123 Main St",
        city: "Anytown"
    }
};

// Convert to JSON string
let jsonString = JSON.stringify(person);
console.log(jsonString);
// {"name":"John Doe","age":30,"hobbies":["reading","swimming"],"address":{"street":"123 Main St","city":"Anytown"}}

// Parse JSON string back to object
let parsedPerson = JSON.parse(jsonString);
console.log(parsedPerson.name);  // "John Doe"

// JSON.stringify with replacer function
let jsonWithReplacer = JSON.stringify(person, (key, value) => {
    if (key === "age") {
        return undefined;  // Exclude age from JSON
    }
    return value;
});

// JSON.stringify with space parameter (pretty printing)
let prettyJson = JSON.stringify(person, null, 2);
console.log(prettyJson);

// Handling dates in JSON
let objWithDate = {
    name: "Event",
    date: new Date()
};

let jsonWithDate = JSON.stringify(objWithDate);
let parsedWithDate = JSON.parse(jsonWithDate);
console.log(typeof parsedWithDate.date);  // "string" (dates become strings)

// Custom toJSON method
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
        this.password = "secret";
    }
    
    toJSON() {
        return {
            name: this.name,
            age: this.age
            // password intentionally excluded
        };
    }
}

let user = new Person("Alice", 30);
console.log(JSON.stringify(user));  // Only name and age included
```

### Set and Map (ES6)
```javascript
// Set - collection of unique values
let mySet = new Set();
mySet.add(1);
mySet.add(2);
mySet.add(2);  // Duplicate, won't be added
mySet.add("hello");

console.log(mySet.size);        // 3
console.log(mySet.has(1));      // true
console.log(mySet.has(3));      // false

// Set from array (removes duplicates)
let numbers = [1, 2, 2, 3, 3, 4];
let uniqueNumbers = new Set(numbers);
console.log([...uniqueNumbers]);  // [1, 2, 3, 4]

// Set operations
let set1 = new Set([1, 2, 3]);
let set2 = new Set([3, 4, 5]);

// Union
let union = new Set([...set1, ...set2]);  // Set {1, 2, 3, 4, 5}

// Intersection
let intersection = new Set([...set1].filter(x => set2.has(x)));  // Set {3}

// Difference
let difference = new Set([...set1].filter(x => !set2.has(x)));  // Set {1, 2}

// Iterating over Set
for (let value of mySet) {
    console.log(value);
}

mySet.forEach(value => console.log(value));

// Map - key-value pairs with any type of keys
let myMap = new Map();
myMap.set("name", "John");
myMap.set(1, "number key");
myMap.set(true, "boolean key");

console.log(myMap.get("name"));     // "John"
console.log(myMap.get(1));          // "number key"
console.log(myMap.has("name"));     // true
console.log(myMap.size);            // 3

// Map from array of arrays
let mapFromArray = new Map([
    ["name", "Alice"],
    ["age", 30],
    ["job", "developer"]
]);

// Object as key
let objKey = { id: 1 };
let objMap = new Map();
objMap.set(objKey, "object value");
console.log(objMap.get(objKey));    // "object value"

// Iterating over Map
for (let [key, value] of myMap) {
    console.log(`${key}: ${value}`);
}

myMap.forEach((value, key) => {
    console.log(`${key}: ${value}`);
});

// Get all keys, values, or entries
console.log([...myMap.keys()]);     // ["name", 1, true]
console.log([...myMap.values()]);   // ["John", "number key", "boolean key"]
console.log([...myMap.entries()]);  // [["name", "John"], [1, "number key"], [true, "boolean key"]]

// Convert Map to Object
let mapToObj = Object.fromEntries(myMap);

// Convert Object to Map
let obj = { a: 1, b: 2, c: 3 };
let objToMap = new Map(Object.entries(obj));
```

## Advanced Concepts

### Closures
```javascript
// Basic closure
function outerFunction(x) {
    return function innerFunction(y) {
        return x + y;  // Inner function has access to outer function's variable
    };
}

let addFive = outerFunction(5);
console.log(addFive(3));  // 8

// Practical closure example - counter
function createCounter() {
    let count = 0;
    return {
        increment: () => ++count,
        decrement: () => --count,
        getCount: () => count
    };
}

let counter = createCounter();
console.log(counter.increment());  // 1
console.log(counter.increment());  // 2
console.log(counter.getCount());   // 2

// Module pattern using closures
const Calculator = (function() {
    let result = 0;
    
    return {
        add: function(x) {
            result += x;
            return this;
        },
        subtract: function(x) {
            result -= x;
            return this;
        },
        multiply: function(x) {
            result *= x;
            return this;
        },
        getResult: function() {
            return result;
        },
        reset: function() {
            result = 0;
            return this;
        }
    };
})();

// Method chaining
Calculator.add(10).multiply(2).subtract(5);
console.log(Calculator.getResult());  // 15
```

### Hoisting
```javascript
// Variable hoisting
console.log(x);  // undefined (not an error)
var x = 5;

// This is what actually happens due to hoisting:
// var x;
// console.log(x);  // undefined
// x = 5;

// let and const are not hoisted in the same way
// console.log(y);  // ReferenceError: Cannot access 'y' before initialization
let y = 10;

// Function hoisting
sayHello();  // Works! Prints "Hello!"

function sayHello() {
    console.log("Hello!");
}

// Function expressions are not hoisted
// sayGoodbye();  // TypeError: sayGoodbye is not a function
var sayGoodbye = function() {
    console.log("Goodbye!");
};
```

### this Keyword
```javascript
// Global context
console.log(this);  // Window object (in browser) or global object (in Node.js)

// Object method
let person = {
    name: "Alice",
    greet: function() {
        console.log(`Hello, I'm ${this.name}`);
    },
    // Arrow function doesn't have its own 'this'
    greetArrow: () => {
        console.log(`Hello, I'm ${this.name}`);  // 'this' refers to global object
    }
};

person.greet();      // "Hello, I'm Alice"
person.greetArrow(); // "Hello, I'm undefined" (or error in strict mode)

// Function context
function regularFunction() {
    console.log(this);  // Global object (or undefined in strict mode)
}

// Constructor function
function Person(name) {
    this.name = name;
    this.greet = function() {
        console.log(`Hello, I'm ${this.name}`);
    };
}

let alice = new Person("Alice");
alice.greet();  // "Hello, I'm Alice"

// Explicit binding with call, apply, bind
function introduce(greeting, punctuation) {
    console.log(`${greeting}, I'm ${this.name}${punctuation}`);
}

let person1 = { name: "Bob" };
let person2 = { name: "Charlie" };

// call() - arguments passed individually
introduce.call(person1, "Hi", "!");      // "Hi, I'm Bob!"

// apply() - arguments passed as array
introduce.apply(person2, ["Hello", "."]);  // "Hello, I'm Charlie."

// bind() - creates new function with bound 'this'
let boundIntroduce = introduce.bind(person1);
boundIntroduce("Hey", "!!!");  // "Hey, I'm Bob!!!"

// Event handler context
let button = document.getElementById("myButton");
button.addEventListener("click", function() {
    console.log(this);  // The button element
});

// Arrow function in event handler
button.addEventListener("click", () => {
    console.log(this);  // Global object (not the button)
});
```

### Prototypes and Prototype Chain
```javascript
// Every function has a prototype property
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    return `${this.name} makes a sound`;
};

// Create instances
let dog = new Animal("Rex");
let cat = new Animal("Whiskers");

console.log(dog.speak());  // "Rex makes a sound"
console.log(cat.speak());  // "Whiskers makes a sound"

// Prototype chain
console.log(dog.__proto__ === Animal.prototype);         // true
console.log(Animal.prototype.__proto__ === Object.prototype);  // true
console.log(Object.prototype.__proto__ === null);       // true

// Adding methods to existing prototypes
String.prototype.reverse = function() {
    return this.split("").reverse().join("");
};

console.log("hello".reverse());  // "olleh"

// Object.create() - create object with specific prototype
let animalProto = {
    speak: function() {
        return `${this.name} makes a sound`;
    }
};

let dog2 = Object.create(animalProto);
dog2.name = "Buddy";
console.log(dog2.speak());  // "Buddy makes a sound"

// Prototype-based inheritance
function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
    return `${this.name} barks`;
};

let myDog = new Dog("Max", "Golden Retriever");
console.log(myDog.speak());  // "Max makes a sound" (inherited)
console.log(myDog.bark());   // "Max barks" (own method)
```

### Generators (ES6)
```javascript
// Generator function
function* numberGenerator() {
    yield 1;
    yield 2;
    yield 3;
    return "Done";
}

let gen = numberGenerator();
console.log(gen.next());  // { value: 1, done: false }
console.log(gen.next());  // { value: 2, done: false }
console.log(gen.next());  // { value: 3, done: false }
console.log(gen.next());  // { value: "Done", done: true }

// Infinite generator
function* infiniteSequence() {
    let i = 0;
    while (true) {
        yield i++;
    }
}

let infinite = infiniteSequence();
console.log(infinite.next().value);  // 0
console.log(infinite.next().value);  // 1
console.log(infinite.next().value);  // 2

// Generator with parameters
function* parameterGenerator() {
    let value = yield "First";
    yield `Received: ${value}`;
}

let paramGen = parameterGenerator();
console.log(paramGen.next());           // { value: "First", done: false }
console.log(paramGen.next("Hello"));    // { value: "Received: Hello", done: false }

// Fibonacci generator
function* fibonacci() {
    let a = 0, b = 1;
    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

let fib = fibonacci();
for (let i = 0; i < 10; i++) {
    console.log(fib.next().value);
}

// Using generators with for...of
function* range(start, end) {
    for (let i = start; i <= end; i++) {
        yield i;
    }
}

for (let num of range(1, 5)) {
    console.log(num);  // 1, 2, 3, 4, 5
}
```

### Symbols
```javascript
// Creating symbols
let sym1 = Symbol();
let sym2 = Symbol("description");
let sym3 = Symbol("description");

console.log(sym2 === sym3);  // false (each symbol is unique)
console.log(sym2.toString()); // "Symbol(description)"

// Symbols as object properties
let obj = {};
let nameSymbol = Symbol("name");
let ageSymbol = Symbol("age");

obj[nameSymbol] = "John";
obj[ageSymbol] = 30;
obj.regularProperty = "regular";

console.log(obj[nameSymbol]);  // "John"

// Symbols are not enumerable in for...in or Object.keys()
for (let key in obj) {
    console.log(key);  // Only "regularProperty"
}

console.log(Object.keys(obj));           // ["regularProperty"]
console.log(Object.getOwnPropertySymbols(obj));  // [Symbol(name), Symbol(age)]

// Well-known symbols
let myArray = [1, 2, 3];
myArray[Symbol.iterator] = function* () {
    for (let i = this.length - 1; i >= 0; i--) {
        yield this[i];
    }
};

for (let item of myArray) {
    console.log(item);  // 3, 2, 1 (reversed)
}

// Symbol.for() - global symbol registry
let globalSym1 = Symbol.for("global");
let globalSym2 = Symbol.for("global");
console.log(globalSym1 === globalSym2);  // true

console.log(Symbol.keyFor(globalSym1));  // "global"
```

### Iterators and Iterables
```javascript
// Creating a custom iterable
let myIterable = {
    data: [1, 2, 3, 4, 5],
    [Symbol.iterator]: function() {
        let index = 0;
        let data = this.data;
        
        return {
            next: function() {
                if (index < data.length) {
                    return { value: data[index++], done: false };
                } else {
                    return { done: true };
                }
            }
        };
    }
};

// Using the iterable
for (let value of myIterable) {
    console.log(value);  // 1, 2, 3, 4, 5
}

// Manual iteration
let iterator = myIterable[Symbol.iterator]();
console.log(iterator.next());  // { value: 1, done: false }
console.log(iterator.next());  // { value: 2, done: false }

// Range iterable
function range(start, end) {
    return {
        [Symbol.iterator]: function* () {
            for (let i = start; i <= end; i++) {
                yield i;
            }
        }
    };
}

for (let num of range(1, 5)) {
    console.log(num);  // 1, 2, 3, 4, 5
}

// Spread operator with iterables
console.log([...range(1, 3)]);  // [1, 2, 3]
```

## Modern JavaScript (ES6+)

### Template Literals
```javascript
// Basic template literals
let name = "Alice";
let age = 30;
let message = `Hello, my name is ${name} and I'm ${age} years old.`;

// Multi-line strings
let multiLine = `
    This is a
    multi-line
    string
`;

// Expression in template literals
let a = 5, b = 10;
console.log(`The sum of ${a} and ${b} is ${a + b}.`);

// Function calls in template literals
function getGreeting() {
    return "Hello";
}

console.log(`${getGreeting()}, World!`);

// Tagged template literals
function highlight(strings, ...values) {
    return strings.reduce((result, string, i) => {
        return result + string + (values[i] ? `<mark>${values[i]}</mark>` : "");
    }, "");
}

let product = "laptop";
let price = 999;
let html = highlight`The ${product} costs ${price}`;
console.log(html);  // "The <mark>laptop</mark> costs $<mark>999</mark>"

// Raw strings
function raw(strings, ...values) {
    console.log(strings.raw[0]);  // Access raw string (with escape sequences)
}

raw`Hello\nWorld`;  // "Hello\nWorld" (literal \n, not newline)
```

### Destructuring Assignment
```javascript
// Array destructuring
let [a, b, c] = [1, 2, 3];
console.log(a, b, c);  // 1 2 3

// Skipping elements
let [first, , third] = [1, 2, 3];
console.log(first, third);  // 1 3

// Rest in destructuring
let [head, ...tail] = [1, 2, 3, 4, 5];
console.log(head);  // 1
console.log(tail);  // [2, 3, 4, 5]

// Default values
let [x, y = 10] = [1];
console.log(x, y);  // 1 10

// Swapping variables
let p = 1, q = 2;
[p, q] = [q, p];
console.log(p, q);  // 2 1

// Object destructuring
let person = { name: "Bob", age: 25, job: "developer" };
let { name, age } = person;
console.log(name, age);  // "Bob" 25

// Renaming variables
let { name: personName, age: personAge } = person;
console.log(personName, personAge);  // "Bob" 25

// Default values in object destructuring
let { name: n, salary = 50000 } = person;
console.log(n, salary);  // "Bob" 50000

// Nested destructuring
let employee = {
    info: { name: "Charlie", age: 35 },
    position: { title: "Manager", level: "Senior" }
};

let {
    info: { name: empName, age: empAge },
    position: { title, level }
} = employee;

console.log(empName, empAge, title, level);  // "Charlie" 35 "Manager" "Senior"

// Function parameter destructuring
function greet({ name, age, job = "unemployed" }) {
    return `Hello ${name}, ${age} year old ${job}`;
}

console.log(greet(person));  // "Hello Bob, 25 year old developer"

// Mixed destructuring
let data = {
    users: ["Alice", "Bob", "Charlie"],
    count: 3
};

let { users: [firstUser, secondUser], count } = data;
console.log(firstUser, secondUser, count);  // "Alice" "Bob" 3
```

### Spread and Rest Operators
```javascript
// Spread operator with arrays
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combined = [...arr1, ...arr2];  // [1, 2, 3, 4, 5, 6]

// Copy array
let original = [1, 2, 3];
let copy = [...original];

// Add elements while spreading
let extended = [0, ...arr1, 4];  // [0, 1, 2, 3, 4]

// Spread in function calls
function sum(a, b, c) {
    return a + b + c;
}

let numbers = [1, 2, 3];
console.log(sum(...numbers));  // 6

// Math functions with spread
let nums = [1, 5, 3, 9, 2];
console.log(Math.max(...nums));  // 9
console.log(Math.min(...nums));  // 1

// Spread with objects
let obj1 = { a: 1, b: 2 };
let obj2 = { c: 3, d: 4 };
let merged = { ...obj1, ...obj2 };  // { a: 1, b: 2, c: 3, d: 4 }

// Override properties
let person = { name: "Alice", age: 30 };
let updatedPerson = { ...person, age: 31 };  // { name: "Alice", age: 31 }

// Rest parameters in functions
function multiply(multiplier, ...numbers) {
    return numbers.map(num => num * multiplier);
}

console.log(multiply(2, 1, 2, 3, 4));  // [2, 4, 6, 8]

// Rest in destructuring
let [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]

let { name, ...otherProps } = { name: "Bob", age: 25, job: "dev" };
console.log(name);       // "Bob"
console.log(otherProps); // { age: 25, job: "dev" }
```

### Default Parameters
```javascript
// Basic default parameters
function greet(name = "World", greeting = "Hello") {
    return `${greeting}, ${name}!`;
}

console.log(greet());              // "Hello, World!"
console.log(greet("Alice"));       // "Hello, Alice!"
console.log(greet("Bob", "Hi"));   // "Hi, Bob!"

// Default parameters with expressions
function createUser(name, role = "user", id = generateId()) {
    return { name, role, id };
}

function generateId() {
    return Math.random().toString(36).substr(2, 9);
}

// Default parameters with previous parameters
function buildUrl(protocol = "https", domain, path = "/") {
    return `${protocol}://${domain}${path}`;
}

console.log(buildUrl(undefined, "example.com", "/api"));  // "https://example.com/api"

// Default parameters with destructuring
function processUser({ name, age = 18, role = "user" } = {}) {
    return `${name} (${age}) - ${role}`;
}

console.log(processUser({ name: "Alice" }));  // "Alice (18) - user"
console.log(processUser());                   // "undefined (18) - user"
```

### Enhanced Object Literals
```javascript
// Property shorthand
let name = "Alice";
let age = 30;

// ES5 way
let person1 = {
    name: name,
    age: age
};

// ES6 shorthand
let person2 = { name, age };

// Method shorthand
let calculator = {
    // ES5 way
    add: function(a, b) {
        return a + b;
    },
    
    // ES6 shorthand
    subtract(a, b) {
        return a - b;
    },
    
    // Async method
    async fetchData() {
        // async operation
    },
    
    // Generator method
    *numberGenerator() {
        yield 1;
        yield 2;
        yield 3;
    }
};

// Computed property names
let propName = "dynamicProperty";
let obj = {
    [propName]: "dynamic value",
    ["computed" + "Property"]: "another value",
    [Symbol.iterator]: function* () {
        yield 1;
        yield 2;
    }
};

console.log(obj.dynamicProperty);     // "dynamic value"
console.log(obj.computedProperty);    // "another value"

// Mixing shorthand and regular properties
function createPerson(name, age) {
    return {
        name,                    // shorthand
        age,                     // shorthand
        id: generateId(),        // regular property
        greet() {               // method shorthand
            return `Hello, I'm ${this.name}`;
        },
        [Symbol.toStringTag]: "Person"  // computed property
    };
}
```

### Arrow Functions
```javascript
// Basic arrow function
let add = (a, b) => a + b;
console.log(add(5, 3));  // 8

// Single parameter (parentheses optional)
let square = x => x * x;
let greet = name => `Hello, ${name}!`;

// No parameters
let sayHello = () => "Hello!";
let getRandom = () => Math.random();

// Multiple statements (need braces and return)
let processNumber = x => {
    let doubled = x * 2;
    let squared = doubled * doubled;
    return squared;
};

// Returning object literals (wrap in parentheses)
let createPerson = (name, age) => ({ name, age });

// Arrow functions in array methods
let numbers = [1, 2, 3, 4, 5];
let doubled = numbers.map(x => x * 2);
let evens = numbers.filter(x => x % 2 === 0);
let sum = numbers.reduce((acc, x) => acc + x, 0);

// Arrow functions don't have their own 'this'
let obj = {
    name: "Alice",
    regularMethod: function() {
        console.log(this.name);  // "Alice"
        
        let arrowFunction = () => {
            console.log(this.name);  // "Alice" (inherits from regularMethod)
        };
        arrowFunction();
    },
    
    arrowMethod: () => {
        console.log(this.name);  // undefined (or error in strict mode)
    }
};

// Arrow functions can't be used as constructors
// let Person = (name) => { this.name = name; };  // Error!
// let person = new Person("Bob");                // TypeError

// Arrow functions don't have arguments object
function regularFunction() {
    console.log(arguments);  // Arguments object
}

let arrowFunction = () => {
    // console.log(arguments);  // ReferenceError
};

// Use rest parameters instead
let arrowWithRest = (...args) => {
    console.log(args);  // Array of arguments
};
```

### Promises and Async/Await (Advanced)
```javascript
// Promise chaining
fetch("/api/user/1")
    .then(response => response.json())
    .then(user => fetch(`/api/posts/${user.id}`))
    .then(response => response.json())
    .then(posts => {
        console.log("User posts:", posts);
    })
    .catch(error => {
        console.error("Error:", error);
    });

// Async/await equivalent
async function getUserPosts(userId) {
    try {
        let userResponse = await fetch(`/api/user/${userId}`);
        let user = await userResponse.json();
        
        let postsResponse = await fetch(`/api/posts/${user.id}`);
        let posts = await postsResponse.json();
        
        console.log("User posts:", posts);
        return posts;
    } catch (error) {
        console.error("Error:", error);
        throw error;
    }
}

// Promise.all with async/await
async function fetchMultipleUsers(userIds) {
    try {
        let promises = userIds.map(id => fetch(`/api/user/${id}`));
        let responses = await Promise.all(promises);
        let users = await Promise.all(responses.map(r => r.json()));
        return users;
    } catch (error) {
        console.error("Failed to fetch users:", error);
    }
}

// Error handling with specific error types
async function robustFetch(url, retries = 3) {
    for (let i = 0; i < retries; i++) {
        try {
            let response = await fetch(url);
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}: ${response.statusText}`);
            }
            return await response.json();
        } catch (error) {
            if (error.name === "TypeError" && i < retries - 1) {
                console.log(`Network error, retrying... (${i + 1}/${retries})`);
                await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
                continue;
            }
            throw error;
        }
    }
}

// Async iterators
async function* fetchUserPages() {
    let page = 1;
    while (true) {
        let response = await fetch(`/api/users?page=${page}`);
        let data = await response.json();
        
        if (data.users.length === 0) break;
        
        yield data.users;
        page++;
    }
}

// Using async iterators
async function processAllUsers() {
    for await (let users of fetchUserPages()) {
        users.forEach(user => console.log(user.name));
    }
}
```

## Best Practices

### Code Organization
```javascript
// Use meaningful variable and function names
// Bad
let d = new Date();
let u = users.filter(x => x.a);

// Good
let currentDate = new Date();
let activeUsers = users.filter(user => user.isActive);

// Use constants for magic numbers
// Bad
if (user.age >= 18) { /* ... */ }

// Good
const LEGAL_AGE = 18;
if (user.age >= LEGAL_AGE) { /* ... */ }

// Group related functionality
const UserService = {
    async fetchUser(id) {
        // implementation
    },
    
    async updateUser(id, data) {
        // implementation
    },
    
    async deleteUser(id) {
        // implementation
    }
};

// Use pure functions when possible
// Pure function - same input always produces same output
function calculateTax(amount, rate) {
    return amount * rate;
}

// Avoid side effects in functions
// Bad
let total = 0;
function addToTotal(amount) {
    total += amount;  // Modifies external state
    return total;
}

// Good
function calculateTotal(currentTotal, amount) {
    return currentTotal + amount;
}
```

### Error Handling Best Practices
```javascript
// Specific error handling
try {
    await someAsyncOperation();
} catch (error) {
    if (error instanceof ValidationError) {
        // Handle validation errors
        showValidationMessage(error.message);
    } else if (error instanceof NetworkError) {
        // Handle network errors
        showRetryButton();
    } else {
        // Handle unexpected errors
        logError(error);
        showGenericErrorMessage();
    }
}

// Custom error classes
class APIError extends Error {
    constructor(message, status, endpoint) {
        super(message);
        this.name = "APIError";
        this.status = status;
        this.endpoint = endpoint;
    }
}

// Input validation
function validateEmail(email) {
    if (!email || typeof email !== 'string') {
        throw new ValidationError('Email is required and must be a string');
    }
    
    if (!email.includes('@')) {
        throw new ValidationError('Email must contain @ symbol');
    }
    
    return email.toLowerCase().trim();
}

// Graceful degradation
function getLocalStorage(key, defaultValue = null) {
    try {
        return JSON.parse(localStorage.getItem(key)) || defaultValue;
    } catch (error) {
        console.warn('Failed to read from localStorage:', error);
        return defaultValue;
    }
}
```

### Performance Best Practices
```javascript
// Debouncing for frequent events
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

// Usage
const searchInput = document.getElementById('search');
const debouncedSearch = debounce(function(event) {
    performSearch(event.target.value);
}, 300);

searchInput.addEventListener('input', debouncedSearch);

// Throttling for scroll events
function throttle(func, limit) {
    let inThrottle;
    return function() {
        const args = arguments;
        const context = this;
        if (!inThrottle) {
            func.apply(context, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// Usage
const throttledScroll = throttle(function() {
    console.log('Scroll event fired');
}, 100);

window.addEventListener('scroll', throttledScroll);

// Efficient array operations
// Instead of nested loops
const users = [/*...*/];
const orders = [/*...*/];

// Bad - O(n²)
const usersWithOrders = users.map(user => ({
    ...user,
    orders: orders.filter(order => order.userId === user.id)
}));

// Good - O(n)
const ordersByUserId = orders.reduce((acc, order) => {
    if (!acc[order.userId]) acc[order.userId] = [];
    acc[order.userId].push(order);
    return acc;
}, {});

const usersWithOrdersOptimized = users.map(user => ({
    ...user,
    orders: ordersByUserId[user.id] || []
}));

// Lazy loading / code splitting
const LazyComponent = lazy(() => import('./LazyComponent'));

// Memory management - avoid memory leaks
class EventManager {
    constructor() {
        this.listeners = new Map();
    }
    
    addEventListener(element, event, handler) {
        element.addEventListener(event, handler);
        
        if (!this.listeners.has(element)) {
            this.listeners.set(element, []);
        }
        this.listeners.get(element).push({ event, handler });
    }
    
    cleanup() {
        for (let [element, events] of this.listeners) {
            events.forEach(({ event, handler }) => {
                element.removeEventListener(event, handler);
            });
        }
        this.listeners.clear();
    }
}
```

### Security Best Practices
```javascript
// Input sanitization
function sanitizeHTML(str) {
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
}

// Safe innerHTML alternative
function safeSetHTML(element, html) {
    // Use textContent for user input
    element.textContent = html;
    // Or use a library like DOMPurify for actual HTML
}

// Avoid eval() and similar dangerous functions
// Bad
const userInput = "alert('XSS')";
eval(userInput);  // Never do this!

// Good - use JSON.parse for data
try {
    const data = JSON.parse(userInput);
    // Process data safely
} catch (error) {
    console.error('Invalid JSON input');
}

// CSRF protection
function makeAPIRequest(url, data) {
    return fetch(url, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'X-Requested-With': 'XMLHttpRequest',
            'X-CSRF-Token': getCsrfToken()
        },
        body: JSON.stringify(data)
    });
}

// Secure cookie handling
function setSecureCookie(name, value, days) {
    const expires = new Date();
    expires.setTime(expires.getTime() + (days * 24 * 60 * 60 * 1000));
    
    document.cookie = `${name}=${value}; expires=${expires.toUTCString()}; path=/; secure; samesite=strict`;
}
```

### Testing Best Practices
```javascript
// Unit testing example (using Jest syntax)
// math.js
export function add(a, b) {
    return a + b;
}

export function divide(a, b) {
    if (b === 0) {
        throw new Error('Division by zero');
    }
    return a / b;
}

// math.test.js
import { add, divide } from './math.js';

describe('Math functions', () => {
    test('add should return sum of two numbers', () => {
        expect(add(2, 3)).toBe(5);
        expect(add(-1, 1)).toBe(0);
        expect(add(0.1, 0.2)).toBeCloseTo(0.3);
    });
    
    test('divide should return quotient', () => {
        expect(divide(10, 2)).toBe(5);
        expect(divide(7, 3)).toBeCloseTo(2.33, 2);
    });
    
    test('divide should throw error for division by zero', () => {
        expect(() => divide(5, 0)).toThrow('Division by zero');
    });
});

// Mocking external dependencies
// api.js
export async function fetchUser(id) {
    const response = await fetch(`/api/users/${id}`);
    return response.json();
}

// user.test.js
import { fetchUser } from './api.js';

// Mock fetch
global.fetch = jest.fn();

test('fetchUser should return user data', async () => {
    const mockUser = { id: 1, name: 'John' };
    fetch.mockResolvedValueOnce({
        json: async () => mockUser
    });
    
    const user = await fetchUser(1);
    expect(user).toEqual(mockUser);
    expect(fetch).toHaveBeenCalledWith('/api/users/1');
});

// Integration testing
test('user registration flow', async () => {
    // Test the complete flow
    const userData = { email: 'test@example.com', password: 'password123' };
    
    // Register user
    const registerResponse = await request(app)
        .post('/api/register')
        .send(userData)
        .expect(201);
    
    // Verify user was created
    expect(registerResponse.body.user.email).toBe(userData.email);
    
    // Test login
    const loginResponse = await request(app)
        .post('/api/login')
        .send(userData)
        .expect(200);
    
    expect(loginResponse.body.token).toBeDefined();
});
```

### Code Style and Formatting
```javascript
// Use consistent naming conventions
// PascalCase for classes and constructors
class UserManager {
    constructor() {
        this.users = [];
    }
}

// camelCase for variables and functions
const userName = 'john_doe';
function getUserById(id) {
    return users.find(user => user.id === id);
}

// UPPER_SNAKE_CASE for constants
const API_BASE_URL = 'https://api.example.com';
const MAX_RETRY_ATTEMPTS = 3;

// Use meaningful comments
/**
 * Calculates the compound interest
 * @param {number} principal - The initial amount
 * @param {number} rate - Annual interest rate (as decimal)
 * @param {number} time - Time period in years
 * @param {number} compound - Compounding frequency per year
 * @returns {number} The final amount after compound interest
 */
function calculateCompoundInterest(principal, rate, time, compound = 1) {
    return principal * Math.pow(1 + rate / compound, compound * time);
}

// Use consistent indentation and spacing
const user = {
    name: 'John Doe',
    age: 30,
    preferences: {
        theme: 'dark',
        notifications: true
    }
};

// Function declarations vs expressions
// Use function declarations for main functions
function processData(data) {
    return data.map(transformItem);
}

// Use function expressions for callbacks and utilities
const transformItem = item => ({
    ...item,
    processed: true,
    timestamp: Date.now()
});

// Consistent use of semicolons (choose a style and stick to it)
const message = "Hello World";
const numbers = [1, 2, 3];
console.log(message);

// Use template literals for string interpolation
const greeting = `Hello, ${user.name}! You have ${unreadCount} unread messages.`;

// Prefer const and let over var
const config = { apiUrl: 'https://api.example.com' };
let currentUser = null;

// Use meaningful destructuring
// Instead of
function processUser(user) {
    console.log(user.name);
    console.log(user.email);
    console.log(user.role);
}

// Do this
function processUser({ name, email, role }) {
    console.log(name);
    console.log(email);
    console.log(role);
}
```

### Module Organization
```javascript
// user.js - Single responsibility
export class User {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }
    
    getDisplayName() {
        return this.name;
    }
}

export function validateUser(userData) {
    // Validation logic
}

export const USER_ROLES = {
    ADMIN: 'admin',
    USER: 'user',
    MODERATOR: 'moderator'
};

// api.js - API related functions
export class APIClient {
    constructor(baseURL) {
        this.baseURL = baseURL;
    }
    
    async get(endpoint) {
        const response = await fetch(`${this.baseURL}${endpoint}`);
        return this.handleResponse(response);
    }
    
    async post(endpoint, data) {
        const response = await fetch(`${this.baseURL}${endpoint}`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(data)
        });
        return this.handleResponse(response);
    }
    
    async handleResponse(response) {
        if (!response.ok) {
            throw new APIError(
                `API Error: ${response.status}`,
                response.status,
                response.url
            );
        }
        return response.json();
    }
}

// utils.js - Utility functions
export function formatDate(date, format = 'YYYY-MM-DD') {
    // Date formatting logic
}

export function deepClone(obj) {
    return JSON.parse(JSON.stringify(obj));
}

export function generateId() {
    return Math.random().toString(36).substr(2, 9);
}

// main.js - Application entry point
import { User, validateUser, USER_ROLES } from './user.js';
import { APIClient } from './api.js';
import { formatDate } from './utils.js';

const api = new APIClient('https://api.example.com');

async function initializeApp() {
    try {
        const userData = await api.get('/user/profile');
        const user = new User(userData.name, userData.email);
        
        console.log(`Welcome, ${user.getDisplayName()}!`);
    } catch (error) {
        console.error('Failed to initialize app:', error);
    }
}

initializeApp();
```

This comprehensive JavaScript guide covers everything from basic syntax to advanced concepts and best practices. Like Python, JavaScript is a versatile language that supports multiple programming paradigms. The key to mastering JavaScript is understanding its unique features like prototypal inheritance, closures, asynchronous programming, and the event-driven nature of web development.

Start with the basics and gradually work your way through the more advanced concepts. Practice by building small projects and gradually increase complexity as you become more comfortable with the language.
