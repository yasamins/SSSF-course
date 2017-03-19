# ECMAScript 6 

1. New features in ECMAScript 2015 (6th edition)
1. `const` and `let`
2. arrow function

---

# New features in ECMAScript 2015 (6th edition)

* Scoped constant `const` and variable `let`
* Function
  * Default Parameter Values
  * Spread and Rest parameters
  * Arrow functions
      * Lexical set `this`
  * Generator `function*` (iterator)
* Class/Object
  * 

---

#Const

* The `const` declaration creates a read-only reference to a value that is block-scoped
  *  the variable identifier cannot be reassigned
  *  in case the content is an object, the object itself can still be altered
* Must be initialized at creation
* Cannot share its name with a function or a variable in the same scope

```javascript
"use strict";
const MY_CONST = 7;

if (MY_CONST === 7) {
    const MY_CONST = 21;
    console.log(MY_CONST); //21
}

console.log(MY_CONST); //7
```

---

#Let

* The `let` declaration declares a block-scoped local variable
  * optionally initialized at creation
  * the variable identifier cannot be reassigned, will raise `SyntaxError` exception

```javascript
"use strict";
for (let i = 0; i < 10; i++) {
    console.log(i); //0,1,2,...
}

console.log(i); //ReferenceError

for (var j = 0; j < 10; j++) {
    console.log(j); //0,1,2,...
}

console.log(j); //10
let j = 12; //SyntaxError
```
---

#Arrow function

* The arrow function have a shorter syntax than `function`
* Does not bind  its own `this`, `arguments`, `super`, or `new.target`
* Can not be used as constructor
* Better avoid to use as method functions 

```javascript
"use strict";
const add = (x, y) => {return x + y;};
console.log(add(1, 2));

//no curl brace and implicit return
const add_implicit = (x, y) => x + y;
console.log(add_implicit(2, 3));

//if only one param, parenthesis can be avoided
const one_param = x => x * x;
console.log(one_param(3));

//if no (or multiple) param, must use parenthesis
const no_param = () => {return 42;};
console.log(no_param());
```
---

#Arrow function

* lexical `this`, e.g. in object 

```javascript
"use strict";
function Prefixer(prefix) {
  this.prefix = prefix;
}

//ES5 needs some hack around
Prefixer.prototype.prefixArrayES5 = function(arr) {
  let that = this; // get the right this 
  return arr.map(function (x) {
    return that.prefix + x;
  }); // or }.bind(this) instead of that = this
}

Prefixer.prototype.prefixArrayES6 = function(arr) {
  return arr.map( x => {return this.prefix + x;});
}

const prefixer = new Prefixer("Hi ");
console.log(prefixer.prefixArrayES5(["Joe", "Mary"]));
console.log(prefixer.prefixArrayES6(["Alice" , "Bob"]));
```

---

#ES6 class/object

* With `class` declaration
* `constructor` and getter `get` setter `set`
* Inheritance `extends` and access to parent class with `super`
* `static` class members 

```javascript
class PrefixerClassES6 {
  constructor(prefix) {
    this.prefix = prefix;
  }
  prefixArray(arr) {
    return arr.map(x => this.prefix + x); 
  }
  get something() {return this.prefix;}
}

const prefixerES6 = new PrefixerClassES6("Hei ");
console.log(prefixerES6.prefixArray(["Mike", "Carla"]));
console.log(prefixerES6.something);
```
