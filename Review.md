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
