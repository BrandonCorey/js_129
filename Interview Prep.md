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
  }
}

let phone = createPhone('Apple', 'iPhone 10', 'white');
```

## OLOO ##
```javascript
const inventoryPrototype = {
  init() {
    return this;
  },

  addItem(product, quantity) {
    this[product] = (this[product] || 0) + quantity;
  },

  removeItem(product, quantity) {
    this.addItem(product, -quantity);
  }
}


let inventory = Object.create(inventoryPrototype).init();

inventory.addItem('RTX 3080', 15);
inventory.removeItem('RTX 3080', 10);
```
