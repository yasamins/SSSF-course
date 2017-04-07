class: center, middle

## ECMAScript 2015 (6th edition)
##### const, let, arrow function and some new features of ES6
#### TX00CR77-3001 / Spring 2017
#### Patrick Ausderau

<!---
to get the slide nicely, clone/pull the repo and drag and drop this .md file
to https://remarkjs.com/remarkise 
giving github url to remarkise throw same-origin policy exception
-->

---

# Contents

1. `const` 
1. `let`
2. arrow function
1. New features in ECMAScript 2015 (6th edition)

---

# Const

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

# Let

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

# Arrow function

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

# Arrow function

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

# ES6 class/object

* With `class` declaration
* `constructor` and getter `get` setter `set`
* Inheritance `extends` and access to parent class with `super`
* `static` class members 

```javascript
"use strict";
class PrefixerClassES6 {
  constructor(prefix) {
    this.prefix = prefix;
  }
  prefixArray(arr) {
    return arr.map(x => this.prefix + x); 
  }
  get something() {return this.prefix + "Test";}
}

const prefixerES6 = new PrefixerClassES6("Hei ");
console.log(prefixerES6.prefixArray(["Mike", "Carla"]));
console.log(prefixerES6.something);
```

---

# Default, rest and spread parameters

* default values for function parameters
```javascript
"use strict";
const defParam = (x, y = 42, z = 2) => x + y + z;
console.log(defParam(6)); //50
console.log(defParam(3, 1)); //6
```

* Aggregation of remaining arguments
```javascript
const restParam = (x, y, ...a) => {return (x + y) * a.length;};
console.log(restParam(1, 2, "foo", "bar", "baz", 42)); //12
```

* Spreading of elements of an iterable collection (like array or string) into both literal elements and individual function parameters
```javascript
const params = ["foo", "bar", 42];
const spreadConst = [1, 2, ...params];
console.log(restParam(...spreadConst)); //9
const str = "foobar";
console.log([...str]); //[ 'f', 'o', 'o', 'b', 'a', 'r' ]
```

---

# New features in ECMAScript 2015 (6th edition)

* Scoped constant `const` and variable `let`
* Function
  * Default Parameter Values
  * Spread and Rest parameters
  * Arrow functions
      * Lexical set `this`
* Class/Object
  * Property shorthand 
  * Method notation in object property definitions
  * Class definition
  * Constructor, getter/setter
  * Static 

---

# New features (will continue on Wed and Thu (or today?))

* New Built-In Methods
   * Object Property Assignment
   * Array Element Finding
   * String Repeating
   * String Searching
   * Number Type Checking
   * Number Safety Checking
   * Number Comparison
   * Number Truncation
   * Number Sign Determination
* Internationalization & Localization
   * Collation
   * Number Formatting
   * Currency Formatting
   * Date/Time Formatting

---

# New features (will continue on Wed and Thu (or today?))

* Generator `function*` (iterator)
* Class/Object Inheritance (multiple)
* Promise
* Template Literals (backtick ` string and raw string access)
* Safe binary and octal & Unicode within strings and regular expressions
* for-of iterator
* Map/Set & WeakMap/WeakSet 
* Typed Arrays 
* Regular Expression Sticky Matching
* Destructuring Assignment 
* Modules Export/Import
* Meta-Programming:   Proxying and Reflection
* Symbol Type
* Typed Arrays

---

# Sources

* [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) V.S. [JavaScript](https://en.wikipedia.org/wiki/JavaScript)
* [Syntax (wikipedia)](https://en.wikipedia.org/wiki/JavaScript_syntax)
* [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) e.g.
  * [const](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/const)
  * [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) V.S. [var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
  * [arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
  * etc.
* [ES6 features](http://es6-features.org/)
* [exploring js es6 arrow function this](http://exploringjs.com/es6/ch_arrow-functions.html)
* [Power of ES6](http://charlesbking.com/power_of_es6/)

