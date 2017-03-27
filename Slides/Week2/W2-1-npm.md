# NPM

### What is npm
  * lets find out from [docs.npm.com](https://docs.npmjs.com/getting-started/what-is-npm)
  
### Starting a project
  * `npm init` creates [package.json](https://docs.npmjs.com/getting-started/using-a-package.json)

### Installing packages (modules)
  * `npm install package-name` installs package locally
    * --save for adding package automatically to dependencies in package.json
    * --save-dev for adding package automatically to dev-dependencies in package.json
  * `npm install -g package-name` installs package globally
    * use this for command line tools
#### Exercise
  * create new folder 'npm-test'
  * start npm project
  * install _eslint_ globally
    * after installing use _eslint_ in your project `eslint --init`
    * edit .eslintrc.js: 
    ```javascript 
    module.exports = {
        "extends": "google",
        "parserOptions": {
            "ecmaVersion": 6
        }
    };
    ```
  * install moment.js locally to dependencies.
  * create a new JS file: _index.js_
  * log current date and time to console in human readable form (use moment.js)
  * import modules to index.js: `const my-module = require('my-module');`
  * how to run: `node index.js`

### Automatig tasks
  * [npm scripts](https://docs.npmjs.com/misc/scripts)

#### Exercise
  * install [_supervisor_](https://github.com/petruisfan/node-supervisor) globally 
  * in package.json create a 'start' script which watches index.js and automatically restarts node when the file is updated
  
### Updating
  * Update  npm: `npm install -g npm`
  * update all outdated local packages `npm update`
  * update all outdated global packages `npm update -g`
  
  