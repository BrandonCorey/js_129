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
- Implicit (own method invocation, regular function call)
  - Use of method invocation of an object's own method is called *method execution context*
```javascript

let obj = { name: 'joe', test: function() {console.log(this.name);} }
obj.test(); // implicit, context is obj

let test = obj.test;
test(); // implicit, context is global (regular function call with parenthesis)

let newObj = { name: 'brandon' }
obj.test.call(newObj) // explicit, context is newObj
```
