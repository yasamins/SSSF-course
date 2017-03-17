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
for (let i = 0; i < 10; i++) {
    console.log(i); //0,1,2,...
}

console.log(i); //ReferenceError

for (var j = 0; j < 10; j++) {
    console.log(j); //0,1,2,...
}

console.log(j); //10
```	    
