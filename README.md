<h1 align="center">
  <!-- Logo -->
  <img src="https://raw.githubusercontent.com/DylanPiercey/set-dom/master/logo.png" alt="Set-DOM"/>

  <br/>

  <!-- Stability -->
  <a href="https://nodejs.org/api/documentation.html#documentation_stability_index">
    <img src="https://img.shields.io/badge/stability-stable-brightgreen.svg?style=flat-square" alt="API stability"/>
  </a>
  <!-- Standard -->
  <a href="https://github.com/feross/standard">
    <img src="https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat-square" alt="Standard"/>
  </a>
  <!-- NPM version -->
  <a href="https://npmjs.org/package/set-dom">
    <img src="https://img.shields.io/npm/v/set-dom.svg?style=flat-square" alt="NPM version"/>
  </a>

  <!-- File size -->
  <a href="https://github.com/DylanPiercey/set-dom/blob/master/dist/set-dom.js">
    <img src="https://badge-size.herokuapp.com/DylanPiercey/set-dom/master/dist/set-dom.js?style=flat-square" alt="File size"/>
  </a>
  <!-- Downloads -->
  <a href="https://npmjs.org/package/set-dom">
    <img src="https://img.shields.io/npm/dm/set-dom.svg?style=flat-square" alt="Downloads"/>
  </a>
  <!-- Gitter chat -->
  <a href="https://gitter.im/DylanPiercey/set-dom">
    <img src="https://img.shields.io/gitter/room/DylanPiercey/set-dom.svg?style=flat-square" alt="Gitter Chat"/>
  </a>
</h1>

A lightweight library to update DOM and persist state.
IE: React diffing with html instead of JSX (bring your own templating language).

# Why
JSX is great but there are so many other nice alternatives.
React is great but it's clunky and opinionated.

This is inspired by [diffhtml](https://github.com/tbranyen/diffhtml), [morphdom](https://github.com/patrick-steele-idem/morphdom) and my knowledge from [tusk](https://github.com/DylanPiercey/tusk). I set out to create a no nonsense "dom to dom" diffing algorithm that was fast and compact.

### Features
* ~800 bytes min/gzip.
* Minimal API.
* Keyed html elements (`data-key` or `id` to shuffle around nodes).
* Use whatever you want to generate html.

# Installation

#### Npm
```console
npm install set-dom
```

#### [Download](https://raw.githubusercontent.com/DylanPiercey/set-dom/master/dist/set-dom.js)
```html
<script type="text/javascript" src="set-dom.js"></script>
<script>
    define(['set-dom'], function (setDOM) {...}); // AMD
    window.setDOM; // Global set-dom if no module system in place.
</script>
```

# Example


```javascript
const setDOM = require("set-dom");

// We will use handlebars for our example.
const hbs = require("handlebars");
const homePage = hbs.compile(`
    <html>
        <head>
            <title>My App</title>
            <meta name="description" content="Rill Application">
        </head>
        <body>
            <div class="app" data-key="home-page">
                {{title}}
                {{#each frameworks}}
                    <div data-key={{name}}>
                        {{name}} is pretty cool.
                    </div>
                {{/each}}
            </div>
            <script src="/app.js"/>
        </body>
    </html>
`);

// You can replace the entire page with your new html (only updates changed elements).
setDOM(document, homePage({
    title: "Hello World.",
    frameworks: [
        { name: "React" },
        { name: "Angular" },
        { name: "Ember" },
        { name: "Backbone" },
        { name: "Everything" }
    ]
}));

// Or update individual elements.
setDOM(myElement, myHTML);
```

# API
+ **setDOM(HTMLEntity, html|HTMLEntity)** : Updates existing DOM to new DOM in as few operations as possible.

---

# Advanced Tips

## Keyed Elements
Just like React (although slightly different) `set-dom` supports keyed nodes.
To help the diffing algorithm reposition your elements be sure to provide a `data-key` or `id` attribute on nodes inside a map. This is optional but key for performance when re-ordering/modifying lists.

Another key difference from React is that `set-dom` simply can't tell when you are rendering an entirely different component. As such it is good practice to use `data-key` when you know that most of the html will be discarded (like when rendering an entirely different page) to skip the diffing process entirely.

## Ignored Elements
Sometimes it is required to simply escape the whole diffing paradigm and do all of the manual dom work yourself. With `set-dom` it is easy to include these types of elements in the page using a special `data-ignore` attribute.

Any elements that have a `data-ignore` will **NEVER** be diffed. The only thing `set-dom` will do for you in this case is automatically add and remove the element.

## Overrides
You can also easily override the attributes used for both *keying* and *ignoring* by manually updating the `KEY` and `IGNORE` propeties of `set-dom` like so.

```js
// Change 'data-key' to 'data-my-key'
setDOM.KEY = 'data-my-key'

// Change 'data-ignore' to 'data-my-ignore'
setDOM.IGNORE = 'data-my-ignore'
```

### Benchmarks
Benchmarks are available on the [vdom-benchmark](https://vdom-benchmark.github.io/vdom-benchmark/) website.

### Contributions

* Use `npm test` to run tests.

Please feel free to create a PR!
