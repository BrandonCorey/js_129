## Objects ##
One of the eight fundamnetal date types in JavaScript

**In the context of OOP**
- A type of data structure
- Characterized by having properties, which are key-value pairs
  - Each key is a string
  - Each value can be any data type
- Can store both data and operations within its structure
- Properties that have function values are called methods

```javascript
let person = {
  name: 'Brandon',
  age: 23,
  
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}
```
## Object Factories ##
An object oriented design pattern used to create mutliple objects of a specific type
- A function is used to instantiate objects
- The object returned in the function body serves as a sort of template for the object you wish to create
### Benefits ###
- Reduces code duplication, objects can be instantiated with a simple function invocation
- Can create many objects far more efficiently
- Increases dynamism as function arguments can have their values used as object properties
### Disadvantages ###
- Memory inefficient
  - All methods defined in the factory object are copied to all instances of the factory
- Impossible to determine the type of an instance
  - `instanceOf` will not work as factory functions do not show up in the prototype chain of the instance
  - `constructor` property will not return anything useful as factories are just normal functions that do not modify the `constructor` property to point to themselves (will return Object)
  - `Object.getPrototypeOf` does not return anything useful as factories are just normal functions and do not modify the prototype of an instance to point to their `prototype` object
```javascript
function createCar(brand, model, year) {
  return {
    brand,
    model,
    year,
    
    revEngine() {
      console.log('Vrrrrmmmmmmm')
    }
  }
}

let car = createCar('Lincoln', 'Town Car', 2009);
```

### Describe the above code snippet ###
The function `createCar` is defined. The function returns an object with properties that have values equal to those of the arguments of the same names passed into the factory function. The returned object also has a `revEngine` method that logs 'Vrrrrmm'. The function is used to instantiate an object and store the reference in the `car` variable.

## Constructors and Prototypes (Psuedo-classical) ##
An object oriented creation pattern that uses constructor functions and inhertiance to create instances of a specific type
- Constructor functions are used to initialize an object (act similar to object factories)
- Psuedo-classical inheritance allows for delegation of property access to the super type of the instance
- Constructors are called with the `new` keyword, which does a few things:
  - Creates a new object
  - Sets the prototype of the new object to the prototype object of the constructor
  - Sets the context of the constructor function to the new object
  - Executes the constructor function
  - returns the new object
```javascript
function Student(name, degree) {
  this.name = name;
  this.degree = degree;
}

let student = new Student('Brandon Corey', 'Economics');
```
### Describe the above code snippet ###
A constructor function named `Student` is defined. The constructor initalizes a new object when the `new` keyword is used to invoke the function. The arguments passed to the function have their values used to create instance properties of the same name on the newly initalized object using the `this` keyword, which points to the new object. The newly instantiated object then has its reference stored in the `student` variable.

## OLOO ##
An object oriented creation pattern that uses prototypal inheritance to create object instances from prototype objects
- Embraces JavaScripts prototypal inhertiance instead of tring to hide it like Psuedo-classical
- Uses prototypal inheritance which allows for method delegation of instance properties to their prototypes
  - Better for memory efficiency
- Similar to other patterns, it can be used to bulk create objects of the same type
```javascript
const inventoryPrototype = {
  init() {
    return this;
  },
  
  addProduct(product, amount) {
    let stock = this;
    if (amount > 0) {
      stock[product] = (stock[product] || 0) + amount;
    }
  },
  
  removeProduct(product, amount) {
    let stock = this;
    if (stock[product]) stock[product] -= amount;
    if (stock[product] < 0) stock[product] = 0;
  }
}

let inventory = Object.create(inventoryPrototype).init();
```
### Describe the code above ###
A variable `inventoryPrototype` is declared an initialized with an object containing three instance methods. This object follows the OLOO pattern, with an `init` method initalize an object. `addProduct` adds a `product` property to the object with an `amount` value, and `removeItem` removes a specified `amount` from an item. Neither add or remove product return anything. `Object.create` is then used to create an object that inherits from `inventoryPrototype`, and the `init` method is immediately chained to initalize the object.

## ES6 Classes ##
An object oriented creation pattern that uses psuedo-classical inheritance abstracted by "class" syntax more conistant with other OO languages
- Uses prototypes and constructors under the hood
- Classes are just functions
- Instance methods are stored in the prototype object of the class
- New objects are initalized with a `constructor` method inside of the class
  - The `constructor` is automatically invoked when the `new` keyword is used with a class name
  - The `new` keyword MUST be used when invoking a class constructor method
  - The `super` keyword can be used within the constructor to invoke the constructor of the super type
- Instance methods can be defined directly inside of the class
- Class syntax uses the `extends` keyword to create sub-types that inherit from the super
```javascript
class Inventory {
  constructor() {}
  
  addProduct(product, amount) {
    let stock = this;
    
    if (amount > 0) {
      stock[product] = (stock[product] || 0) + amount;
    }
  }
  
  removeProduct(product, amount) {
    let stock = this;
    if (stock[product] && amount > 0) stock[product] -= amount;
    if (stock[product] < 0) stock[product] = 0;
  }
}
```
### Explain the code above ###
An `Inventory` type is defined using ES6 classes. It has three instance methods, `constructor` to initalize an empty inventory object, `addProduct` to add a `product` instance property to an the object, and `removeProduct` to remove an `amount` value from a `product `property. `addProduct` accepts a `product` and an `amount` argument and creates a property with those values if it does not already exist. It also requies that the `amount` is greater than 0. `removeProduct` subtracts an `amount` from an existing property if the value is greater than 0. Neither `addProduct` nor `removeProduct` return anything. 

## Methods and Properties ##
_Properties_ are key value pairs stored within an object. Properties can be accessed using name of the "key"
- Instance properties are properties of an object instance or properties inherited through the prototype chain of the instance
- Static properties are properties defined directly on the constructor of an instance
  - These are not inherited

_Methods_ are properties of an object whose associated value is a function
- Instance methods refer to methods of an instance, or more commonly methods that appear in the prototype chain (usually prototype object) of an instance
- Static methods are methods defined directly on the constructor of an instance
  - These are not inherited
```javascript
function Student(name, degree, grades) {
  this.name = name;
  this.degree = degree;
  this.grades = grades; // <-- These are instance properties
}

Student.prototype.averageGrade = function() { // <-- This is an instance method
  let sum = this.grades.reduce((a, b) => a + b);
  return sum / this.grades.length;
}

Student.info = function() { // <-- This is a static method
  console.log('These are students who have attended university');
}
```
### Describe the difference between instance methods and static methods ###
Instance methods are methods that are defined either directly on an instance, or methods defined on an the prototype object of the constructor of an instance. This means that instance methods can be inherited from the constructor. Static methods on the other hand are defined directly _on_ the constructor, meaning that they pertain to type itself, and not the instances of the type. This also means that they are not inherited by instances of the type as they are not contained within the prototype of the instance.

## Prototypal and Psuedo-classical Inheritance ##
Prototypal inheritance refers to objects delegating property access to other other objects referred to as there *prototypes*
- Prototypal inheritance is the basis for all inhertiance in JavaScript
- Every object has an internal [[Prototype]] property that points to its prototype 
- Inherited properties are not copied to inheriting objects, they are accessed farther up the prototype chain within the object that contains them

Psuedo-classical inheritance is a type of prototypal inhertiance where an object inherits properties from the prototype object of a constructor function
- This behavior is observed in both Constructor/prototype creation patterns and ES6 classes
- Can also refer to sub-typing, where one consturctors prototype object inherits from anothers prototype object
```javascript
// Prototypal
let catPrototype = {
  meow(){
    console.log('Meoww');
  }
}

let cat = Object.create(catPrototype);
cat.meow(); // 'Meoww'
```
```javascript
// Psuedo-classical
function Cat() {}

Cat.prototype.meow = function() {
  console.log('Meoww');
}

let cat = new Cat();
cat.meow(); // 'Meoww'
```
### Explain the two code snippets above ###
In the first snippet, `catPrototype` is declared an initalized to an object with one `meow` method that logs 'Moeww' and returns `undefined`. It is then passed as an argument to `Object.create`, which initalizes an object with `catPrototype` as the prototype. The reference of this object is then stored inside the `cat` variable.

In the second code snipped, a constructor funciton `Cat` is defined that takes no arguments and returns `undefined`. A `meow` method is then defined on prototype object of `Cat` to allow instances of the constructor to inherit the method, which logs 'Meoww' and returns `undefined`. `Cat` is then invoked with the `new` keyword to initialize a new object that inerhits from `Cat.prototype`, and stores the reference to this object in the variable `cat`. 

This code demonstrates the different levels of abstraction between prototypal and psuedo-classical inhertiance. Prototypal inhertiance refers to the delegation of object properties to an object that appears in the object's prototype chain, where as psuedoclassical inhertiance refers to inhertiance from a prototype object of a constructor function. This is still prototypal inhertiance at its core, but refers to the use of it within the context of a specific creation pattern.

## Encapsulation ## 
Encapsulation refers to the idea of bundling data and operations that use that data into a single entity (object)
- One of the three pillars of OOP
- Allows for easier scalabiliy as objects are useful for organizing state and behavior as programs grow in size
- Also refers to the idea of seperation of public and private interface, however this is natively supported in JS
  - Public interface refers to properties of an object that are allowed to be accessed directly
  - Private interface refers to properties of an object that are not directly accessible outside of the object
 ```javascript
let pc = {
  cpu: 'R7 5800X3D',
  gpu: 'RTX 3080',
  mobo: 'B550',
  
 info() {
   console.log(
     `Contains a ${this.cpu} and ${this.gpu} on a ${this.mobo}`
   )
 }
}
 ```
 ### Explain the snippet above ###
 An `pc` variable is declared and initalized with an object containing four properties, one of which is a method. The properties contain info on the CPU, GPU, and motherboard of the pc. The info method prints this informatin, using a template literal to interpolate the names of the respective properties. This demonstrates encapsulation as all of the properties pertain to pc components, and the `info` method is simply prints a descrition of the components of the pc, which is relevant to the state of the object.
 
## Polymorphism ##
Refers to the idea of different object types responding differently to the same method invocation
- Means the programmer doesn't have to care about what type the object calling the method is

**Polymorphism through inhertiance**
Refers to two concepts:
- Sub types inheriting behavior from super types (normal psuedo-classical inhertiance)
- Sub types overriding behavior from the suepr type with their own version of the method

```javascript
// Custom example
class Firearm {
  shoot() {
    console.log('pew');
  }
}

class Glock extends Firearm {}
class AK extends Firearm {
  shoot() {
    console.log('pew pew pew');
  }
}

let glock = new Glock();
let ak = new AK();

[glock, ak].forEach(gun => gun.shoot());
// 'pew'
// 'pew pew pew'
```

**Polymorphism through duck-typing**
A type of polymorphism that allows objects of unrelated types to be categorized together by there use of similar behavior
- Means they have methods that share the same name and act in a similar way in relative to the type of the object
- Can sometimes mean that the unrelated types have similar properties overall
```javascript
class Programmer {
  develop() {
    this.programGame();
  }
  
  programGame() {}
}

class Musician {
  develop() {
    this.createScore();
  }
  
  createScore() {}
}

class Artist {
  develop() {
    this.designArt();
  }
  
  designArt() {}
}

class VideoGame {
  constructor() {
    this.programmer = new Programmer();
    this.musician = new Musician();
    this.artist = new Artist();
  }
  
  getDevs() {
    return [
      this.programmer,
      this.musician,
      this.artist,
    ]
  }
  
  develop() {
    let devs = this.getDevs();
    devs.forEach(dev => dev.develop());
  }
}
```
### Whats the difference between polymorphism through inheritance and duck typing ###
Polymorphism through inheritance refers the concept that allows methods to be inherited from a super type, and overwritten if the sub type needs specific functionality for that method. This allows us to call methods on different sub types without worrying about implemenation details of the method.

Polymorphism through duck typing is similar, however this refers to the invocation of methods of the same name on _unrelated_ types that may share relatively similar functionality that allows each type to be categorized together. Unlike through inheritance, duck typing lets us think of unrelated types as similar strictly through behavior, not through relation within a prototypal chain.

## Collaborator objects ##
These are objects that are referenced by an object property
- These the values of the properties can be either an object literal or a reference to an object
```javascript
let cat = {
  name: 'tiffany',
  color: 'orange',
  
  qualities: ['fluffy', 'slow', 'nice'], // object literal collaborator
}
```
### Describe the snippet above ###
A `cat` variable is declared and initalized to an object literal with three instance properties. The properties `name` and `color` both have string values, and the `qualities` property is a collaborator, referencing an array that contains three string elements

## Single vs multiple inheritance ##
Single inheritance refers to the idea of an object or type only inheriting from a single type, rather than multiple. In JS, the only type of inhertiance is single

Multiple inheritance refers to objects or sub types inheriting from multiple other types. This type of inheritance is found in more classical OO languages

Because JS does not natively support multiple inheritance, design patterns like the _mix-in_ pattern were developed to allow objects to recieve state and behavior from objects that are not their prototype to allow for the modeling of an increase number of relationships

## Mixins vs Inheritance ##
Mixins refer to copying state and behavior from one object ao another, usually using the `Object.assign` method
- This is done as a stand in for multiple inheritance as JS only supports single inheritance
- Differs from inheritance as all state and behavior will be copied to the target object of the mix in, unlike with inhertiance where methods are typically stored in a prototype object and accessed through delegation

```javascript
const roaring = {
  roar() {
    console.log('GRRRRR');
  }
}

const meowing = {
  meow() {
    console.log('meoww');
  }
}

class Tiger {}
class Puma {}
class Leopard {}

Object.assign(Tiger.prototype, roaring);
Object.assign(Puma.prototype, meowing);
Object.assign(Leopard.prototype, roaring, meowing);
```
### Describe the code above ###
A roaring variable is declared and initalized to an object that contains one `roar` method that logs a roar. A `meow` variable is then initalized with an object containing a `emow` method that logs a leow. Both methods return `undefined`. Then, three classes are declared with nothing inside of their body. `Object.assign` is used to use the `roaring` object and `meowing` object as mixins for their relevant classes, which allows `Leopard` to recieve the behavior of both, something not possible through inheritance.

## Methods and Functions ##

### Method invocation ###
Method invocation can be done using multiple different methods
- object-method syntax
- invoking a method through use of a variable
- using `bind`, `apply`, or `call`

The way you invoke a method determines its execution context

### Functin invocation ###
Function invocation can be done using the following methods
- Regular parenthentical function call
- Using `bind`, `apply`, or `call`

The way you invoke a function also determines its execution context

```javascript
function print(string) {
  console.log(string);
}

let obj = {}
obj.print = function(string) { console.log(string); }

print('hello'); // context is global
obj.print('hi'); // context is obj
```

## Higher Order Functions ##
A function that either accepts a function as an argument or returns a function
- Higher order functions are possible in JS because functinos are first class citizens
  - Means that a function can be used anywhere any other value can be used
```javascript
const squareNum = num => num ** 2;

let square = [1, 2, 3].map(squareNum);
console.log(square); // [1, 4, 9];
```
### What is the higher order function in the above code ###
The higher order functino is `map`. This is because map takes a function argument `squareNum`.

 ## The global object ##
 This is an object that stores important information about the global environment that your code is being run
 - In Node, this object is called `global`
 - In the browser, this object is called `window`
 - The global object acts as the execution context for regular function invocations
 - Properties can be added to the global object by using assigning values to "variables" that have not been declared
 ```javascript
 prop = 5
 global.prop; // 5
 
 function globalFunc() {
   this.prop = 3;
 }
 
 globalFunc();
 global.prop; // 3
 ```
 ### How can you check if the context for the function execution is global object? ###
 One way to check if the function invocation used the global object as the execution context is to evalute the expression `this === global`
 
 ## Method and Property lookup sequence ##
 The lookup sequence for properties starts begins with the object that provides the context for the invocation, if not found, the prototype chain is searched
 - If the `null` is reached and the property has not been found, undefine is returned
   - `null` is the prototype object the default prototype `Object.prototyp`
 ```javascript
 function Dog(name, breed, color) {
  this.name = name;
  this.breed = breed;
  this.color = color;
}

Dog.prototype.bark = function () {
  console.log('Woof!!');
}

let dog = new Dog('spot', 'German Shepherd', 'Black');
Object.getPrototypeOf(dog) === Dog.prototype; // true
Object.getPrototypeOf(Dog) === Object.prototype; // true
Object.getPrototpyeOf(Object.prototype === null) // true

dog.name;
// dog (found name)
// 'spot'

dog.bark();
// dog --> Dog.prototype (found bark)
// 'Woof!!'

dog.age;
// dog --> Dog.prototype --> Object.prototype --> null (not found)
// undefined
 ```
