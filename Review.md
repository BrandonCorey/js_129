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
