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
function createCar(model, brand, year) {
  return {
    brand,
    model,
    year,
    
    revEngine() {
      console.log('Vrrrrmmmmmmm')
    }
  }
}
```

### Describe the above code snippet ###
The function `createCar` is defined. The function returns an object with properties that have values equal to those of the arguments of the same names passed into the factory function. The returned object also has a `revEngine` method that logs 'Vrrrrmm'.

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
A constructor function named `Student` is defined. The constructor initalizes a new object when the `new` keyword is used to invoke the function. The arguments passed to the function have their values used to create instance properties of the same name on the newly initalized object using the `this` keyword, which points to the new object.
