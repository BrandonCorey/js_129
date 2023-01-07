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
- More memory efficient than factory functions as methods are stored inside of a prototype object
- Syntax embraces prototypal inhertiance instead of trying to hide it with constructors and classes

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

## Polymorphism ##

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
