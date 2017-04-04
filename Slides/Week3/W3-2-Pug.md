# PUG
  * templating engine used for server-side templating
  * example: 
  ```jade
    div
      h1 My heading
      ul
        li Some list item
        li Some other list item
      p This here is a paragraph.
  ```
  * result:
  ```html
  <div>
    <h1>My heading</h1>
    <ul>
      <li>Some list item</li>
      <li>Some other list item</li>
    </ul>
    <p>This here is a paragraph.</p>
  </div>
  ```

## Use in Express.js
  * [Instructions](https://expressjs.com/en/guide/using-template-engines.html)
  * Excercise
    * Create a simple web page using instructions above
  
## Features
  * [Doctype](https://pugjs.org/language/doctype.html)
  * [Attributes](https://pugjs.org/language/attributes.html)
  * [Includes](https://pugjs.org/language/includes.html)
  * Excercise
    * Modify the template of the previous excercise so that the result is [valid HTML](https://validator.w3.org/)
  * [Template inheritance](https://pugjs.org/language/inheritance.html)
  * [Interpolation](Interpolation)
  * [Iteration](https://pugjs.org/language/iteration.html)
  * Excercise
      * List the cats in your Mongo database using Pug
      * Resulted HTML might be something like this
      ```html
      <table>
          <tr>
              <th>name</th>
              <th>age</th>
              <th>gender</th>
              <th>weight</th>
          </tr>
          <tr>
              <td>Viski</td>
              <td>4</td>
              <td>male</td>
              <td>4</td>
          </tr>
          <tr>
              <td>Kerttu</td>
              <td>4</td>
              <td>male</td>
              <td>5.5</td>
          </tr>
      </table>
      ```
  
