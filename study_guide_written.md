## OOP ##
A programming pattern that uses objects as the basic building blocks of a program instead of local variables and functins
**Helps answer these questinos**
- What are the important concepts in the program?
- What are the properties of a vehicle?
- How do we create vehicles?
- What operations can I perform on a vehicle?
- Where should we add new properties and methods?

## Objects ##
One of the eight fundamental data types in JavaScript

An object is a data structure that can store state and behavior
  - State: Data (properties with values that are not functions i.e primitives, arrays, simple objects etc.)
  - Behavior: Operations (properties with values that are functions
    - These are called *methods*
  - State (data) and behavior (operations) are stored using `properties`

 **Properties**
- Properties have key - value pairs
  - All keys are strings, the keys values can be any data type
- Properties can be accessed using either dot or bracket notation
  - NOTE: Methods cannot be invoked with bracket notation
     - Dot notation: Also called member access notation
- Bracket notation can use any string, dot notation requires valid variable names  
  
```javascript
let obj = {
  num: 1, // state
  
  speak() {
    console.log('hey'); // behavior
  }
}

```
## Methods ##
Properties of an object that have *functions* as values
- Methods can be referred to as an objects *behvarior*
- Methods can be invoked using dot-notation

```javascript
// Using obj object from above
obj.speak(); // example of dot notation
```

## Encapsulation ##
Encapsulation refers to the bundling of data and the operations that work on that data together in a single entity (i.e an object)
  - Packaging related state and behavior together
  - **Note** In other languages, encapsulation also refers to the seperation of public and private data, but this is not supported in JS
    - In these cases, an object exposes specific data and operations (referd to as the public interface) for other objects, while keeping other state and behavior inaccessible to other objects

```javascript
// encapsulating data and operations for this student into a single object
let student = {
  name: 'Brandon',
  year: 12,
  grades: [91, 87, 93],
  
  getGPA() {
    let gradesEntered = this.grades.length;
    let total = this.grades.reduce((sum, grade) => sum + grade);
    
    return total / gradesEntered;
  }
}
```

## Collaborator Objects ##
Collaborator Objects refer to objects referenced by an object's `properties`
- These can be simple objects, or any other objects like arrays and dates

```javascript
let school = {
  name: "Umass Amherst",
  state: "MA",
  
}

let student = {
  name: 'Brandon',
  year: 12,
  grades: [91, 87, 93], // collaborator object
  school: school,
}

student.school.name; // 'Umass Amherst"
```

## Object Factories ##
A function used to create objects

**Advantages of Factory functions**
- Can create many objects of a specific type more quickly
- Less code duplication
- More dynamic (can modify certain properties of object while keeping rest the same)
 
**Disadvantages**
- Every object created has a copy of all the same methods (redundant)
- It is impossible to determine if an object came from a factory
  - `instanceOf` will not work since the instances of the factory do not inherit from it
  - `constructor` will not work because the constructor of the returned object will be `Object`
```javascript
function createComputer(cpu, gpu) {
  return {
    cpu,
    gpu,
  }
}

let computer = createComputer('R7 5800X3D', 'RTX 3080');
```

## Prototypal Inhertience ## 
In JS, objects can inherit properties and behavior from one another
  - JavaScript uses **_prototypal inheritance_**
  - The object that you inherit properies and methods form is called the **prototype**

**Delegation**
- An inheriting object does not recieve properties from prototype
- Instead, it delegates (trusts and redirects) property and method access it its prototype
  - Can use `hasOwnProperty` to check if an object has a proeprty or is inheriting it

**[[Prototype]]**
- Every object has a hidden `[[Prototype]]` property that points to its prototype object
- The default prototype for all objects in JS is `Object.prototype`
  - This means all objects in JS have access to the methods of this object, unless explicitly given a prototype of `null`.

```javascript
let personProto = {
  greet() {
    console.log('hello');
  }
}

let person = Object.create(personProto);
person.greet(); // "hello"
```


## Higher Order Functions ##
Has one of the following characteristics
- Takes a function as an argument
- Returns a function

**Benefits**
- Lets programmer use powerful and flexible abstractions
  - Typically this leads to more declarative code

```javascript
let arr = [1, 2, 3];

arr.forEach(num => console.log(num)); // forEach is a higher order function as it takes the anonymous function as an argument
```

## The Global Object ##
JavaScript creates a global object when it begins to run
- It serves as the implicit execution context for function invocations
- In node, this is the object `global`
- In the browser, this is the object `window`
  - Has built in propeties like `infinity` and `parseInt`
  - Undeclared variables are added as properties to the global object

```javascript
newProp = 1;
global.newProp; // 1

function Test() {
  this.anotherProp = 2;
}

Test();
global.anotherProp; // 2;
```
## Execution Context ##
The environment in which a function executes
- Also refered to as the value of `this`
- Determined by how the function is called, not where it is lexically defined

Two ways to set context when calling a function or method
- Explicit (bind, apply, call)
  - Methods like `call` allow you to pass an explicit context to a function to have it executed with
  - NOTE: call and apply call functions, bind returns a function bound to a context
- Implicit (method invocation, regular function call, new keyword)
  - Use of method invocation of an object's own method is called *method execution context*
```javascript

let obj = { name: 'joe', test: function() {console.log(this.name);} }
obj.test(); // implicit, context is obj

let test = obj.test;
test(); // implicit, context is global (regular function call with parenthesis)

let newObj = { name: 'brandon' }
obj.test.call(newObj) // explicit, context is newObj
```
### call, apply, bind ###
**call**
- Calls a function with an explicit execution context passed in as the first argument
  - Additional arguments for the function being called can be passed as subsequent arguments

**apply**
- Works similarly to call, but accepts an array of arguments after `this`

**bind**
- Returns a new function permnanently bound to an exeuction context
- Context cannot be changed, even with `call` and `apply`
  - Original function is not bound to anything explicity, `bind` always returns a new function!
```javascript
let vehicle = {
  engineOn: false,
  
  start() {
    this.engineOn = true;
  },
  
  stop() {
    this.engineOn = false;
  }
};

let truck = {
  year: 2009,
  mpg: 15,
  engineOn: false,
};

vehicle.start.call(truck);
truck.engineOn; // true

vehicle.stop.call(truck);
truck.engineOn; // false

let turnOnTruck = vehicle.start.bind(truck);
turnOnTruck();
truck.engineOn // true
```
## Context Loss ##

### Method copied from Object ###
- When the reference to an object method is stored in a variable and the variable is used to call the method
  - This strips the exeuction context, makes `this` point to the global object
```javascript
let student = {
  name: 'brandon',
  study() {
    console.log(`${this.name} is studying!`;
  }
}

let study = student.study;
study(); // 'undefined is studying!' (looks for name property is global object)
```

### Nested function not using surrounding context ###
- When a nested function (typically inside of a method) is called normally and loses its surrounding context
  - NOTE: This does not apply to arrow functions which retain their surrounding scope even when called regularly
**Solutions**
- Arrow functions --> preserve surrounding scope when called regularly
- `call` --> pass context of object to function call
- `apply` --> pass context of object to function call
- `bind` --> use `bind` method on function (at enclosing curly brace) to store in variable, then call function with that variable
- store object `this` in variable, use it to specify context
```javascript
let student = {
  name: 'brandon',
  study() {
    function print() {console.log(`${this.name} is studying!`);}
    
    print();
  }
}

student.study(); 'undefined is studying!'
```
```javascript
let student = {
  name: 'brandon',
  study() {
    function print() {console.log(`${this.name} is studying!`);}
      
    print.call(student);
  }
};

student.study(); 'brandon is studying!'
```
```javascript
let student = {
  name: 'brandon',
  study() {
    let print = function() {console.log(`${this.name} is studying!`);}.bind(student);
    print();
  }
};

student.study(); 'brandon is studying!'
```
```javascript
let student = {
  name: 'brandon',
  study() {
    let that = this;
    function print() {console.log(`${that.name} is studying!`);}
    print();
  }
};
student.study(); 'brandon is studying!'
```

### Function as argument losing surrounding context ###
- Passing functions as arguments can strip them of context if they are invoked regularly within the function body
- This issue is solved similarly to the nested functions issue

```javascript
let student = {
  name: 'brandon',
  grades: [87, 91, 93],
  
  printGrades() {
    this.grades.forEach(function(grade) {console.log(`${this.name}'s grade: ${grade}`)});
  }
}

// unefined's grade: 87
// unefined's grade: 91
// unefined's grade: 93
```
```javascript
let student = {
  name: 'brandon',
  grades: [87, 91, 93],
  
  printGrades() {
    this.grades.forEach(function(grade) {
    console.log(`${this.name}'s grade: ${grade}`)
    }.bind(this));
  }
}
```
## Constructors ##
A function that returns an object with some key distinctins
- Called with `new` keyword
- Use `this` to define object properties
- Don't need explicit return value
  - If an explicit return value is used:
  - Primitve return values will be ignored, the `new` object will be returned anyway
  - Objects will overwride the `new` object and be returned instead

### `new` ###
When used with a function
- Creates new object
- Sets prototype of new object to object referenced by constructors `prototype` property
- Sets the execution context for the function body to point to the newly creaed object
- Invokes the function
- Returns the new object after the function invocation finihses
NOTE: Cannot use with arrow functions since they do not have `prototype` property
- Also cannot call on methods that use concise syntax

## Constructors with prototypes (Psudeo-classical) ##
- A creation pattern that does two things:
  - Uses constructors to act as object factories of a specific type
  - Uses the `prototype` object of the constructor to share methods with instances of the constructor through inheritance 
    - We say that the instances delegate method calls to the prototype (if they don't have their own version of the method)
 
**Benfefit**
- The main benefit here is creating a method once and storing it in a single object
- This removes code redudancy and reduces the strain on memory of duplicating methods
  - This is in direct contrast to how methods are shared using factory functions
- The `new` keyword handles a number of processes automatically, such as creating a new object and setting its prototype 
- Can check if an instance is of a particular type using:
  - `constructor` property
  - `instanceOf` keyword 

```javascript
function Student(name, year, grades) {
  this.name = name;
  this.year = year;
  this.grades = grades;
}

Student.prototype.averageGrade = function() { // Store methods in prototype object instead of on newly created object directly
  let sum = this.grades.reduce((a, b) => a + b, 0);
  return sum / this.grades.length;
}

let brandon = new Student('brandon', 12, [85, 89, 91]);
brandon.averageGrade(); // 88.333
```

## Static and Instance Properties and Methods ##
### Instance Properties ###
Properties of an instance
- These may be defined in the constructor (and added to the instance at invocation)
- They may also be defined directly on the object
```javascript
brandon.year; // 12 (This is an instance property)
Student.year; // undefined (This is not an instance property)
```
### Instance Methods ###
Methods are also properties, but instance properties with method values are called *Instance Methods*
- Instance methods also include, and typically refer to, **methods stored in the prototype object**
  - This is because methods are not typically stored directly on instances (these are instance methods also however)
```javascript
brandon.averageGrade(); // This is an instance method (it is stored in [[Prototype]] of brandon);
```
### Static Properties ###
Properties defined and accessed directly on the constructor
- These belong to the type, rather than an instance of the type

```javascript
function Cat(name, breed, weight) {
  this.name = name;
  this.breed = breed;
  this.weight = weight;
}
Cat.species = 'Felis Catus' // This is a static property
Cat.description = function() {
  console.log(`The species ${this.species} is also known as the house cat.`); // static method
}
```
### Static Methods ###
Static properties that have functions as their values. These are defined simiarly to instance methods, but on the constructor again
```javascript
function Cat(name, breed, weight) {
  this.name = name;
  this.breed = brade;
  this.weight = weight;
}

Cat.getSpecies = function() {
  console.log('Felis Catus');
}
```
## Built-in Constructors ##
There are a number of built in constructors and prototpes in JavaScript, such as:

### Array ###
`Array` constructor is used with `new` keyword to create arrays
- Takes arguments of array elements
- If only one argument provided, creates array with that number of empty elements
- Array.prototype stores of all the methods that every array inherits
- NOTE: Array is a constructor that can be used without `new` keyword and functions the same
```javascript
let arr = new Array(5).fill(null);
console.log(Array.isArray(arr)); // true
console.log(Object.getPrototypeOf(arr) === Array.prototype); // true
```

### Object ###
`Object` constructor is used with `new` keyword to create objects
- `new` is not required with Object either, but it is recommended
- All objects created inherit from Object.prototype
- There are lots of useful static methods for Object such as:
  - `Object.assign`
  - `Object.create`
  - `Object.keys`
  - `Object.values`

Since other constructors like Array, Function, Date are also objeects, they also inherit from Object.prototype
```javascript
let obj = new Object();
obj.a = 1;
obj.hasOwnProperty('a'); // true
let arr = new Array(1, 2, 3); // [1, 2, 3];
arr.hasOwnProperty('1'); // true
```

### Date ###
The `Date` constructor is used with the `new` keyword and cretes Date objects
- When passed zero arguments, returns current date
- Can pass the constructor a string that represents a date and the constructor will try to coerce it into a date 
- All dates inherit from Date.prototype
- Useful prototype methods like
  - `Date.prototype.toString`
  - `Date.prototype.getFullYear` --> returns full year as number i.e 2023
  - `Date.prototype.getDay` --> returns day of week as number 0 - 6
  - `Date.prototype.getMonth` --> returns month as Number 0 - 11
  - `Date.prototype.getDate` --> returns day of month as number 0 - 31
```javascript
let birthday = new Date('11-01-1998');
let dayOfWeek = birthday.getDay();
let year = birthday.getFullYear();
dayOfWeek; // 0
year; // 1998
```
### String ###
Creates String objects
- These are different from string primitives
- String primitives are wrapped in String objects temporarily when a String.prototype method is called
- The String.prototype method returns a new modified string based on the method used
- String used without `new` will coerce a non string type to a string primitive
```javascript
let str = new String('hello'); // These are string objects
let str1 = new String('hello');
typeof str1; // object
str === str1 // false
let num = 5;
let strNum = String(num); // '5'
typeof strNum // string
```

### Boolean and Numbner ###
Similar to String, when called with `new`, they create `Number` and `Boolean` objects
- Without `new`, they coerce their argument to either a Number or a boolean
- They have primitive and object forms, with JS wrapping the primitives in objects temporarily to access methdods and properties of the constructors and their prototype objects
```javascript
let five = new Number(5);
five === 5; // false (five is an object, 5 is a primitive)
Number('5'); // 5 (coerces string to number)

let falseObj = new Boolean(false); // new booleam obkect
falseObj === false; // false, falseObj is a boolean object, false is a primitive
Boolean(0) === false; // true, 0 is coerced to false primitive
```
## ES6 Classes ##
Syntactic sugar for constuctor and prototype psuedo-classical pattern
