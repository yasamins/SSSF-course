class: center, middle

## ECMAScript 2015 (6th edition)
##### Promise & fetch
#### TX00CR77-3001 / Spring 2017
#### Patrick Ausderau

---

# Contents

1. `Promise` 
1. `fetch()`

---

# Promise

* A `Promise` object is use for asynchronous task
* A `Promise` represents a value which may be available now, or in the future, or never
* When creating a Promise, a function is given as parameter that will be called immediately
  * that function takes `resolve` and `reject` parameters
  * once that function completes, either calls the `resolve` function to resolve the promise or `rejects` it if an error occurred
  * If an exception is thrown in the function, the promise is rejected (the return value of the function is ignored)

```javascript
const myPromise = new Promise((resolve, reject) => {
  /* ...usually some asynchronous task... */
  //on succes...
  resolve('Yes :)');
  //on failure...
  reject('NO NO NO!');
}
```

---

# Promise then() and catch()

* A pending promise can either be fulfilled with a value, or rejected with a reason (error)
  * When any of these happen, the associated handlers queued up by a promise's `then` method are called
  * the Promise `then()` and `catch()` methods return promises so they can be chained

![chaining promise](img/promises.png)
<span style="font-size: small;">Mozilla Contributors CC-BY-SA 2.5</span>

---

# Promise then() 

* The Promise `then` method take one or two parameters
  * A function called when the Promise is fulfilled. This function has one argument, the fulfillment value
  * Optionally a function called when the Promise is rejected. This function has one argument, the rejection reason

```javascript
myPromise.then((successMsg) => {
  console.log('Really: ' + successMsg);
}, (errorMsg) => {
  console.log('So sad :( ' + errorMsg);
});
```

---

# Promise catch()

* The `catch` method returns a Promise and deals with rejected cases only, so takes only one parameter function

```javascript
myPromise.then((successMsg) => {
  console.log('Really: ' + successMsg);
  //but something bad...
  throw 'oh, no :(';
}).catch((e) => {
  console.log(e); 
}).then(() => {
  console.log('after a catch the chain is restored');
});
```

---

# Promise race()

* The Promise `race` takes an array (or other iterable) of promises and return a promise as soon as one of the promises in the iterable resolves or rejects 

```javascript
const p1 = new Promise((resolve, reject) => { 
  setTimeout(resolve, 500, 'one'); 
});
const p2 = new Promise((resolve, reject) => { 
  setTimeout(resolve, 100, 'two'); 
});

Promise.race([p1, p2]).then((value) => {
  console.log(value); // with a huge probability "two"
});
```

---

# Promise all()

* The Promise `all` method returns a single Promise that resolves when all of the promises in the iterable argument have resolved or rejects. Returns
  * if all have resolved, an array of all the resolved values in the same order as defined in the iterable
  * if some have rejected, the reason of the first promise that rejects

```javascript
const p1 = new Promise((resolve, reject) => { 
  setTimeout(resolve, 500, 'one'); 
});
const p2 = new Promise((resolve, reject) => { 
  setTimeout(resolve, 100, 'two'); 
});

Promise.all([p1, p2]).then((values) => {
  console.log(values); // ["one", "two"]
}, reason => {
  console.log(reason); // or reject?
});
```

---

# Fetch

* The Fetch API provides an interface for fetching resources locally or over the network (similar to `XMLHttpRequest`) and provides 
  * `GlobalFetch.fetch()` method used to fetch a resource
  * `Headers` which represents response/request headers, allowing to query them and take different actions depending on the results
  * `Request` which represents a resource request
  * `Response` which represents the response to a request
  * `Body` that provides methods relating to the body of the response/request, allowing to declare what its content type is and how it should be handled

---

# fetch()

* The fetch() method starts the process of fetching a resource from the network. This returns a promise that resolves to the Response object representing the response to your request
  * A input attribute, either a string with the direct URL or a `Request` object
  * Optionally, an options object which contains settings for the request, like
     * `method: POST`
     * `headers:` that will contains `Headers` object (e.g. to set `Content-Type` to `text/plain`)
     * `mode:` The mode you want to use for the request, e.g., `cors`, `no-cors`, or `same-origin`
     * [etc.](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters) 

---

# fetch()

Example:

```javascript 
const myImage = document.querySelector('img');
const myRequest = new Request('http://placekitten.com/768/720');

fetch(myRequest).then((response) => { 
  if(response.ok) {
    return response.blob();
  }
  throw new Error('Network response was not ok.');
}).then((myBlob) => {
  const objectURL = URL.createObjectURL(myBlob);
  myImage.src = objectURL;
}).catch(function(error) {
  console.log('Problem :( ' + error.message);
});
```

---

# Header

* The Headers interface of the Fetch API allows to perform various actions on HTTP request and response headers

```javascript
const myHeaders = new Headers({
  'Content-Type': 'text/plain',
  'Content-Length': content.length.toString(),
});

const myInit = { method: 'GET',
               headers: myHeaders,
               cache: 'default' };
 
fetch('https://www.test.org', myInit).then((response) => { /* ... */ });
```
---

# Request, Response and Body

* The `Request` interface of the Fetch API represents a resource request
  * it contains properties such as `method`, `url`, `headers`, [etc.](https://developer.mozilla.org/en-US/docs/Web/API/Request#Properties)
* The `Response` interface of the Fetch API represents the response to a request
  * it contains properties such as `headers`, `ok` (status code in range 200-299), `redirected`, [etc.](https://developer.mozilla.org/en-US/docs/Web/API/Response#Properties)  
* While you can create `Request` and `Response` objects, it is more likely to get them as the result of another API operation, e.g. as a `fetch()` result
* Both implements the `Body` mixin
  * it provides methods such as `text()`, `json()`, `blob()`, [etc.](https://developer.mozilla.org/en-US/docs/Web/API/Body#Methods)

```javascript

const  myRequest = new Request('cats.json');

fetch(myRequest).then((response) => { 
  return response.json(); 
}).then((json) => {
  for(let i = 0; i < json.cats.length; i++) {
      /* ... */
  }
});

```

---

# Sources

  * [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
  * [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
