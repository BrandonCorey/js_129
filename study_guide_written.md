## Objects ##
- One of the eight fundamental data types in JavaScript
- An object is a data structure that can store state and behavior
  - State: Data (properties with values that are not functions i.e primitives, arrays, simple objects etc.)
  - Behavior: Operations (properties with values that are functions
    - These are called *methods*
  - State (data) and behavior (operations) are stored using `properties`

 ### Properties ###
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
- Properties of an object that have *functions* as values
- Methods can be referred to as an objects *behvarior*
- Methods can be invoked using dot-notation

```javascript
// Using obj object from above
obj.speak(); // example of dot notation
```

## Encapsulation ##
- Encapsulation refers to the bundling of data and the operations that work on that data together in a single entity (i.e an object)
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
- Collaborator Objects refer to objects referenced by object `properties`
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
- A function used to create objects

**Advantages of Factory functions**
- Can create many objects more quickly
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
