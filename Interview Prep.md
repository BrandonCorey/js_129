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
