<!--

WARNING!! DON'T EDIT THE FILE README.md on the root of the project, that one is a GENERATED FILE!

You should just edit the source file at src/README.md - the one which stars with ## Lazy loading JS modules in the browser

-->

## Lazy loading JS modules in the browser

<img src="img/sloth-zootopia.gif" class="logo"></p>

Tiago Garcia @ [Mobile+Web DevCon](http://mobilewebdevconference.com/san-francisco-2017/) 
<br/>Mar 1st, 2017

---

## Tiago Garcia

<p class="ac-column-logo">
  <img src="img/avatar.jpg" class="avatar ac-logo-big">
</p>

<ul class="ac-column-text">
  <li>JavaScript P.I., dog daddy, vegan rollerblader,<br/>not your typical Brazilian.</li>
  <li>Tech Manager at [Avenue Code](http://www.avenuecode.com)</li>
  <li>Tech Lead at [Macys.com](http://www.macys.com)</li>
  <li>Twitter: [@tiagooo_romero](https://twitter.com/tiagooo_romero)</li>
  <li>Site: *[tiagorg.com](http://tiagorg.com)*</li>
  <li>*[tgarcia@avenuecode.com](mailto:tgarcia@avenuecode.com)*</li>
</ul>

----

## Avenue Code

<p class="ac-column-logo">
  <img src="img/ac-logo-big.png" class="ac-logo-big" />
</p>

<ul class="ac-column-text">
  <li>Offices in San Francisco, São Paulo, Belo Horizonte</li>
  <li>Primary verticals: <br/>Retail & Financial services</li>
  <li>Partners with MuleSoft, Adobe, Chef, Oracle and AWS</li>
  <li>Project Management, Business Analysis, Development, QA, DevOps, Coaching</li>
  <li>*[www.avenuecode.com/careers](https://www.avenuecode.com/careers)*</li>
</ul>

---

# Part I

<img src="img/part-i.jpg" />

---

# Agenda

- *Part I: JavaScript Lazy-loading for dummies*
  - SPA for performance?
  - Lazy-loading 101
  - Do I need Lazy-loading?
  - How to Lazy load?

---

## SPA for performance?

<img src="img/back-in-my-day.jpg" />

----

## SPA for performance?

- Moving you server-side app to SPA would improve the page performance:
  - Requests returning JSON instead of HTML
  - Rendering a new view instead of a page reload
  - Routing and state management on the client-side
- But that may not be enough!
  - Have you ever checked if you application is downloading more stuff that is being actually used?
  - All those performance gains can fall short if you download all your stuff at once!

---

## Lazy-loading 101

- E-commerce A (server-side rendered) has 5 pages: home, browse, product, cart, checkout
  - Each page has *300 KB* HTML + *100 KB* JS
  - The complete flow will download:
    <ul>
      <li>*400 KB* for each page <i class="fa fa-exclamation-triangle icon-warning" aria-hidden="true"></i> </li>
      <li>*2 MB* total <i class="fa fa-exclamation-triangle icon-warning" aria-hidden="true"></i></li>
    </ul>

----

## Lazy-loading 101

- E-commerce B (SPA) has the same 5 pages
  - 1 actual page + 4 views
  - Home page has *300 KB* HTML + *100 KB* JS
  - Each of the views has *50 KB* JSON + *150 KB* JS
  - The complete flow will download:
    <ul>
      <li>*1.2 MB* total (vs *2 MB*) <i class="fa fa-check-circle icon-success" aria-hidden="true"></i></li>
      <li>80% just on home page: *1 MB* (vs *400 KB*) <i class="fa fa-times-circle icon-danger" aria-hidden="true"></i></li>
    </ul>
  - However, if you use Lazy-loading:
    - *400 KB* on home page
    - *200 KB* each view after

----

## What is Lazy-loading?

- *Lazy loading* is a design pattern about deferring the _initialization_ (loading/fetching/allocation) of a _resource_ (code/data/asset) until the point at which it is needed.
- Its main goal is to improve _efficiency_ when a significant amount of resources is not needed at first.
- Lazy-loading is targeted to increase performance and save on memory consumption and processing power.
- It's an [EAA pattern](http://martinfowler.com/eaaCatalog/lazyLoad.html) from Martin Fowler.

---

## Do I need Lazy-loading?

- *Above the fold*
  - what you see first when you open a page
  - part of the *Critical Rendering Path*
  - must be rendered during the page load time
  - thus, it can't be lazy loaded
- *Below the fold*
  - everything else, needs scrolling or user interaction
  - won't be displayed during the page load time
  - it doesn't need to be rendered with the page load
  - thus, it can be lazy loaded

----

## Do I need Lazy-loading?

- Ask yourself: is there any chunk of code/library that only runs:
  - below the fold (as a reviews panel)?
  - after some event (as a button click)?
  - upon a certain condition (as an uncommon widget)?
- If you answered yes, you may profit from lazy load and potentially improve your page performance.
- Just defer the downloading of those chunks of code/libraries until the trigger is executed.

----

## Do I need Lazy-loading?

- Lazy loading isn't recommended for certain scenarios:
  - Supporting network limitations (offline mode)
  - Web-based mobile apps (Web Views)
  - Apps that can't be paused (games)
  - Specific UX requirements (single loading screen)

---

## How to Lazy load?

- DON'T include all your scripts in the page at once.
- DON'T import all your modules on the top of your file.
- Carefully decide WHEN to import or require your modules and libraries:
  - Below the fold (scroll listener)
  - Event callbacks (user interactions / network calls)
  - Conditionally (for uncommon scenarios)
  - After some time (chat overlays)
- This talk is about JS but you can also lazy load images, fonts and CSS.

---

# Part II

<img src="img/part-ii.jpg" />

---

## Agenda

- *Part II: Blazing loading*
  - Modules
  - Lazy-loading in AMD
  - Lazy-loading in CommonJS
  - Lazy-loading in ES2015
  - Webpack 2

---

## Module

- *Structural* design pattern
- Purpose: to define reusable components with private/public attributes and methods
- Pre-condition: a chunk of code with a return point
- Post-condition: a definition that represents that chunk of code as a module

----

## Global object 

- Encapsulation through *closures* -> function scope

```javascript
  var Zoo = (function() {
    var getBarkStyle = function(isHowler) {
      return isHowler? 'woooooow!': 'woof, woof!';
    };
    return {
      Dog: function(name, breed) {
        this.name = name;
        this.bark = getBarkStyle(breed === 'husky');
      },
      Wolf: function(name) {
        this.name = name;
        this.bark = getBarkStyle(true);
      }
    };
  })(); // IIFE
```

```javascript
var myDog = new Zoo.Dog('Sherlock', 'beagle');
console.log(myDog.name + ': ' + myDog.bark); // Sherlock: woof, woof!
```

----

## Revealing Module

- Flexibility to switch items from private to public scope.
```javascript
  let Zoo = (function() {
    let getBarkStyle = isHowler => isHowler ? 'woooooow!' : 'woof, woof!';
    let Dog = function(name, breed) {
      this.name = name;
      this.bark = getBarkStyle(breed === 'husky');
    };
    let Wolf = function(name) {
      this.name = name;
      this.bark = getBarkStyle(true);
    };
    return {
      Dog: Dog,
      Wolf: Wolf
    };
  })(); // IIFE
```

----

## Why to use Modules?

- Spaghetti code is bad for reusability, readability, code organization and is very brittle (side-effects).
- Modules can fix these all if properly implemented.
- A module can be delivered as a dependency for another module.
- Modules can be packaged and deployed separately from each other, mitigating the "butterfly effect".
- Modules bring cohesion up and coupling down.

----

## Module loaders

- On the previous example, *Zoo* is a global var, which is:
  - fragile (any posterior code can modify/redefine it)
  - not scalable (what if you define 100 modules?)
  - counter-productive (you have to manually resolve your dependencies)
- Enter *module loaders*:
  - Container for module registration under aliases
  - Dependency injection
  - Modules loading on demand
- Included with your Module standard of choice!

---

## AMD

- A standard for *asynchronous* modules.
- Pros:
  - Multiple modules can be loaded in parallel.
  - It naturally works on the browser.
  - Easy to lazy-load modules.
- Cons:
  - Complex, easy to create race conditions.
  - Not guaranteed order of execution of modules.
  - Dependencies array gets hard to read easily.

----

## Lazy-loading in AMD

- Export your module interface with *define()*
- Import on the client using *require()*
```javascript
  // dog.js
  define('dog', [], function() {
    var Dog = function(name, breed) {
      this.name = name;
      this.breed = breed;
    };
    Dog.prototype.bark = function() {
      return this.name + ': ' + getBarkStyle(this.breed);
    };
    function getBarkStyle(breed) {
      return breed === 'husky'? 'woooooow!': 'woof, woof!';
    };
    return Dog;
  });
```

----

## Lazy-loading in AMD

- Lazy-loading upon user interaction:

```javascript
// main.js
define('main', [], function() {
  document.getElementById('loadDogButton')
          .addEventListener('click', function(e) {
    // Lazy-loading dog module
    require(['dog'], function(Dog) {
      var dogContainer = document.getElementById('dogContainer');

      var sherlock = new Dog('Sherlock', 'beagle');
      dogContainer.innerHTML += sherlock.bark();

      var whisky = new Dog('Whisky', 'husky');
      dogContainer.innerHTML += '<br/>' + whisky.bark();
    });
  });
});
```

---

## CommonJS

- A standard for *synchronous* modules.
- Pros:
  - Official Node.js and NPM format.
  - Simple and convenient syntax.
  - Guaranteed order of execution of modules.
- Cons:
  - Doesn't naturally work on the browser (there are solutions for that as *Browserify* and *Webpack*).
  - Sequential loading of modules is slower than asynchronous.
  - NPM dependency tree can skyrocket easily.

----

## Lazy-loading in CommonJS

- Export your module interface with *module.exports*
- Import on the client using *require(dependency)*
```javascript
  // dog.js
  var Dog = function(name, breed) {
    this.name = name;
    this.breed = breed;
  };
  Dog.prototype.bark = function() {
    return this.name + ': ' + getBarkStyle(this.breed);
  };
  function getBarkStyle(breed) {
    return breed === 'husky'? 'woooooow!': 'woof, woof!';
  };
  module.exports = Dog;
```

----

## Lazy-loading in CommonJS

- Using [Webpack](https://webpack.js.org)'s *`require.ensure`*

```javascript
// main.js
document.getElementById('loadDogButton')
        .addEventListener('click', function(e) {
  // Lazy-loading dog module
  require.ensure([], function(require) {
    var Dog = require('./dog'),
        dogContainer = document.getElementById('dogContainer');

    var sherlock = new Dog('Sherlock', 'beagle');
    dogContainer.innerHTML += sherlock.bark();

    var whisky = new Dog('Whisky', 'husky');
    dogContainer.innerHTML += '<br/>' + whisky.bark();
  });
});
```

- Read more about [Code Splitting](https://webpack.js.org/guides/code-splitting-require/)

---

## ES2015 Modules

- ES2015 offers native Modules which are quite a bit similar to CommonJS.

```javascript
// dog.js
let getBarkStyle = function(breed) {
  return breed === 'husky'? 'woooooow!': 'woof, woof!';
};

export class Dog {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
  }

  bark() {
    return `${this.name}: ${getBarkStyle(this.breed)}`;
  };
}
```

----

## ES2015 Module Loader

- ES2015 Modules are not yet natively supported by all browsers & Node.js.
- You can use them right now with a transpiler as [Babel.js](https://babeljs.io/).
- Consider a module loader such as [System.js](https://github.com/systemjs/systemjs), which supports AMD, CommonJS, ES2015 and global.
- The Module Loader spec is in the works by WhatWG: [Loader](https://whatwg.github.io/loader/).

----

## System.js

- [System.js](https://github.com/systemjs/systemjs) is a module loader which supports AMD, CommonJS, ES2015 and global scripts.
- Is performs asynchronous module loading using a Promises-based API.
- Promises can be chained and combined.
- *`Promises.all `* can load multiple modules in parallel.
- System.js 0.2.0 ships with [dynamic import() operator](https://github.com/tc39/proposal-dynamic-import).

----

## Lazy-loading in System.js

- Using the [dynamic import() operator](http://www.2ality.com/2017/01/import-operator.html)

```
// main.js
document.getElementById('loadDogButton')
        .addEventListener('click', e => {
  // Lazy-loading dog module
  import('dog').then(Dog => {
    let dogContainer = document.getElementById('dogContainer');

    let sherlock = new Dog('Sherlock', 'beagle');
    dogContainer.innerHTML += sherlock.bark();

    let whisky = new Dog('Whisky', 'husky');
    dogContainer.innerHTML += `<br/>${whisky.bark()}`;
  })).catch(err => {
    console.log("Module loading failed");
  });
});
```

---

## Webpack 2

- Webpack is a module bundler constituted of [Entries, Outputs, Loaders and Plugins](https://webpack.js.org/concepts/).
- [Webpack 2](https://webpack.js.org/) (just released) now offers native *ES2015 modules* and *System.js* support.
- Loads JS modules in AMD, CommonJS and ES2015 (and also TypeScript and CoffeeScript).
- Performs bundling (importing the dependency graph in the right order).
- Lazy-loads JS and CSS (code splitting).
- Performs tree-shaking.

---

## Challenge

- Fork from [github.com/tiagorg/lazy-load-es2015-webpack2-challenge](https://github.com/tiagorg/lazy-load-es2015-webpack2-challenge)
- Take a moment to understand this implementation.
- Completely lazy-load *`Animations`* and its dependencies upon click of the "Merry Xmas" button.
- Completely lazy-load *`Messaging`* after 5s the page has been loaded.
- Verify your lazy-loadable bundles have been generated and are loading properly.
- Use the [dynamic import() operator](http://www.2ality.com/2017/01/import-operator.html).

---

# Conclusion

<img src="img/lazy-load-it.jpg" />

---

# Questions?

<img src="img/questions.jpg" />

---

# Thanks!

- Special thanks to Avenue Code, Mobile+Web DevCon crew and most importantly all the attendees!
- Slides: [tiagorg.com/lab-lazy-loading-es2015-modules](http://tiagorg.com/lab-lazy-loading-es2015-modules)
- Github: [github.com/tiagorg/lab-lazy-loading-es2015-modules](https://github.com/tiagorg/lab-lazy-loading-es2015-modules)
- More talks at [tiagorg.com](http://tiagorg.com)
- Follow me at [@tiagooo_romero](https://twitter.com/tiagooo_romero)