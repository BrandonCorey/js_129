## Factory Function ##
```javascript
function createPhone(brand, model, color) {
  return {
    brand,
    model,
    color,
    vibrate: false,

    info() {
      console.log(`This is a(n) ${this.color} ${this.brand} ${this.model}`)
    },

    ring() {
      if (this.vibrate) console.log('brrr, brrr, brrr');
      else console.log('RING RING RING');
    },

    silence() {
      this.vibrate = !this.vibrate;
    }
  };
}

let phone = createPhone('Apple', 'iPhone 10', 'white');
```

### Pros ###
- Can create multiple objects of a certain type very quickly
- Reduces code duplication
- Allows for more dynamic objects as values can be passed to properties through function arguments

### Cons ###
- Memory inefficient, copies all methods to all objects
- Impossible to determine the type of an object created using a constructor

## OLOO ##
```javascript
const carPrototype = {
  init(brand, model) {
    this.brand = brand;
    this.model = model;
    this.engineOn = false;
    return this;
  },

  start() {
    this.engineOn = true;
  }
}

const truckPrototype = Object.create(carPrototype);

truckPrototype.tow = function() {
  console.log('towing...');
}

let truck = Object.create(truckPrototype).init('Lincoln', 'Town Car');
```
### Pros ###
- Can create multiple objects of a certain type very quickly
- Reduces code duplication
- More memory efficient than factory functions as methods are stored inside of a prototype object
- Syntax embraces prototypal inhertiance instead of trying to hide it with constructors and classes

### Cons ###
- Similar to factory function, cannot determine the type of an object created using OLOO

## Constructor Prototype ##
```javascript
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Order(name, price, quantity) {
  Product.call(this, name, price);
  this.quantity = quantity;
}

Order.prototype = Object.create(Product.prototype);
Order.prototype.constructor = Order;

Order.prototype.cost = function() {
  return this.quantity * this. price;
}

let order = new Order('RTX 3080', 800, 4);
console.log(order.cost()); // 3200
```

## Pros ##
- Can create multiple objects of a certain type very quickly
- Reduces code duplication
- Able to store methods inside of JS functions built in prototype object
- Able to use `instanceOf` or the prototype object to determine instance's type
- Syntax similar to other OO languages

## Cons ##
- Syntax is a bit more verbose than other creation patterns

## ES6 Classes ##
```javascript
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }
}

class Order extends Product {
  constructor(name, price, quantity) {
    super(name, price);
    this.quantity = quantity;
  }

  cost() {
    return this.quantity * this.price;
  }
}

let order = new Order('RTX 3080', 800, 4);
console.log(order.cost()); // 3200
```
### Pros ###
- Can create multiple objects of a certain type very quickly
- Reduces code duplication
- Able to store methods inside of JS functions built in prototype object
- Able to use `instanceOf` or the prototype object to determine instance's type
- Syntax similar to other OO languages
  - Can extend types very easily with `extend` keyword

### Cons ###
- Must use `new` keyword with class syntax
- Prototype object of class is not enumerable by default

## Mixin ##
```javascript
let meowable = {
  meow() {
    console.log('meoww');
  }
}

let roarable = {
  roar() {
    console.log('ROAR!');
  }
}

class Tiger {}
class Puma {}
class Leopard {}

Object.assign(Tiger.prototype, roarable);
Object.assign(Puma.prototype, meowable);
Object.assign(Leopard.prototype, meowable, roarable);

let leopard = new Leopard();

leopard.roar(); // 'ROAR!'
leopard.meow(); // 'meoww'
```

### Benefits ###
- Allows for modeling for a greater number of relationships
- Acts as a solution for the single inheritance issue of JS

In the above example, without mixins, we would probably creating a roaring cat class anda  meowing cat class. The problem
- Tiger could be a sub type of roaring class
- Puma could be a sub type of meowing class
- Leopard could be a sub type of either but NOT both
  - This is a problem because it needs both behaviors
  - This is why we use mixins

## Polymorphism ##

### Pros ###
Allows us to call methods on objects of different types without worrying about the type of the object

### Duck Typing ###
```javascript
class Programmer {
  constructor() {}

  develop() {
    console.log('Programming...');
  }
}

class Artist {
  constructor() {}

  develop() {
    console.log('Creating art...');
  }
}

class Musician {
  constructor() {}

  develop() {
    console.log('Creating music...');
  }
}


class Game {
  constructor() {
    this.programmer = new Programmer();
    this.Artist = new Artist();
    this.Musician = new Musician();
  }

  develop() {
    const devs = [this.programmer, this.Artist, this.Musician];

    devs.forEach(dev => dev.develop());
  }
}

let game = new Game();
game.develop();
```

### Benefits ###
Allows us to categorize unrelated types together based on similar behavior relative to their types
- The unrelated types will typically have similar properties
  - An example of this is in JS using array methods on string

### Inheritance ###
```javascript
class Gun {
  constructor() {}

  shoot() {
    console.log('pew');
  }
}

class Revolver extends Gun {}

class MachineGun extends Gun {
  shoot() {
    console.log('pew, pew, pew');
  }
}

let gun = new Gun();
let revolver = new Revolver(); // polymorphism through inheritance
let machineGun = new MachineGun(); // polymorphism through inheritance


let guns = [gun, revolver, machineGun];
guns.forEach(gun => gun.shoot());
```

### Benefits ###
This covers two behaviors
- Sub types inheriting methods from super types
- Sub types overriding methods from super types with their own (of the same name)
