# First steps into security

## HTTPS

* [HTTPS](https://en.wikipedia.org/wiki/HTTPS)
  * HTTP over [TLS/SSL](https://en.wikipedia.org/wiki/Transport_Layer_Security)
  * _authentication_ of the visited website 
  * protection of the _privacy_ and _integrity_ of the exchanged data

### express (localhost)

* Generate keys and certificate with [openssl](https://www.openssl.org/) (in real life, you would need to get certified by a third party, e.g. [Let’s Encrypt](https://letsencrypt.org/))

```shell
$ openssl genrsa -out ssl-key.pem 1024
$ openssl req -new -key ssl-key.pem -out certrequest.csr
$ openssl x509 -req -in certrequest.csr -signkey ssl-key.pem -out ssl-cert.pem
```

* Put the keys and certificate in the app folder

```javascript
const express = require('express');
const https = require('https');
const fs = require('fs');

const sslkey = fs.readFileSync('ssl-key.pem');
const sslcert = fs.readFileSync('ssl-cert.pem')

const options = {
      key: sslkey,
      cert: sslcert
};

const app = express();

https.createServer(options, app).listen(3000);

app.get('/', (req, res) => {
  res.send('Hello Secure World!');
});
```

* Eventually, force redirection from HTTP to HTTPS

```javascript
const http = require('http');

http.createServer((req, res) => {
      res.writeHead(301, { 'Location': 'https://localhost:3000' + req.url });
      res.end();
}).listen(8080);
```

### express (jelastic)

* A valid certificate (courtesy of helpdesk) for jelastic.metropolia.fi can be used for all its subdomains
  * Well, nothing to do, navigate to your app with https://
* Eventually, force the redirection from HTTP to HTTPS

```javascript
const express = require('express');
const app = express();

app.enable('trust proxy');

// Add a handler to inspect the req.secure flag (see 
// http://expressjs.com/api#req.secure). This allows us 
// to know whether the request was via http or https.
// https://github.com/aerwin/https-redirect-demo/blob/master/server.js
app.use ((req, res, next) => {
  if (req.secure) {
    // request was via https, so do no special handling
    next();
  } else {
    // request was via http, so redirect to https
    res.redirect('https://' + req.headers.host + req.url);
  }
});

app.listen(3000);
```

---

## Authentication

* Process of validating the user is who she claims to be
* Usually done online by user providing username and password
* For APIs tokens are usually used because:
   * Single user can have multiple tokens
   * If one token is exposed, it can be easily disabled
   * Tokens can have explicit access restrictions and thus prevent possible damages on exposure


### express

* Implementing security-related things yourself is generally not smart
* Still, for the sake of example, authentication can be implemented as Express.js middleware:
```javascript
app.use((req, res, next) => {
    if (req.query.token === 'SECRET_TOKEN_TOKEN') {
        next();
    }
    else {
        res.status(401).send('Please sign in.');
    }
});
```

### [Passport.js](http://passportjs.org/)

* De facto standard authentication solution for Express.js applications
* Flexible & does not mount routes
* Supports OAuth (Facebook, Twitter, etc.) & OpenID 
* Has over 300 login strategies available

#### Username and password authentication

* Install:
```shell
$ npm install --save passport
```

* Register the strategy:
```javascript
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
passport.use(new LocalStrategy(
    (username, password, done) => {
        if (username !== process.env.username || password !== process.env.password) {
            done(null, false, {message: 'Incorrect credentials.'});
            return;
        }
        return done(null, {});
    }
));
app.use(passport.initialize());
```

* Login route:
```javascript
app.post('/login', 
  passport.authenticate('local', { failureRedirect: '/login' }),
  (req, res) => {
    res.redirect('/');
  });
```


#### Token-based authentication

* Ready strategy for token-base authentication is available with `passport-localapikey` module:
```javascript
passport.use(new LocalAPIKeyStrategy(
  (apikey, done) => {
    if (apikey !== process.env.TOKEN) { return done(null, false); }
    return done(null, {});
  }
));
```

#### Authentication errors

* If authentication credentials are bad, `done(null, false)` should be called
* If there happens an error, `done(error)` should be called

---

## Authorization

* After user is identified, it needs to be determined whether user has access to certain resource
* Often much more complicated than authentication


### [acl](https://www.npmjs.com/package/acl)
* Access Control List (ACL) module for authorization
* Role-based, hierarchical access control
* Supports Redis, MongoDB and in-memory backends with also 3rd-party backends for firebase, knex, etc.

