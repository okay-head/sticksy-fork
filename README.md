<h1 align="left">Sticksy.js 📌</h1>
<h2>Sticksy clone</h2>
<h3>You're not the one without bugs, it took me a long time to make it work for my project.</h3>

[![npm](https://badge.fury.io/js/sticksy.svg)](https://www.npmjs.com/package/sticksy)
[![gzip size](https://badgen.net/badgesize/gzip/https://unpkg.com/sticksy@latest/dist/sticksy.min.js)](https://cdn.jsdelivr.net/npm/sticksy/dist/sticksy.min.js)
[![hits](https://data.jsdelivr.com/v1/package/npm/sticksy/badge)](https://cdn.jsdelivr.net/npm/sticksy/dist/sticksy.min.js)
[![snyk](https://snyk.io/test/npm/sticksy/badge.svg)](https://snyk.io/test/npm/sticksy)
[![license](https://badgen.net/npm/license/lodash)](https://opensource.org/licenses/MIT)

Sticksy.js is a **zero-dependency** JavaScript library that sticks your elements to the top until they reaching the bottom.
Unlike [**Q2W3 WordPress Plugin**](https://wordpress.org/plugins/q2w3-fixed-widget/), you don't need WordPress, jQuery, or other stuff. **You can use it in any web project.** \
_It's simple and ultra fast. ⚡_

Just import and initialize:

```javascript
var stickyElems = Sticksy.initializeAll('.container > .sticky') // and that's all!
```

> ⚠️ The library **is not** a `position: sticky` polyfill, it makes the sibling elements move down.

<br/>
<p align="center">
  <img alt="Fixed widgets in sidebar" src="https://github.com/kovart/sticksy/raw/assets/demo.gif?raw=true"></img>
</p>

## When do you need Sticksy?

> The basic use-case of the library is to make **fixed widgets** in a sidebar.

The library is especially useful for ads or other items that visitors want to interact with.
Sticky blocks are perceived much better by your visitors than unfixed widgets and therefore they have a significantly higher click-through rate.

_But also you can use it for some design features._

## Features

-   Setup in one line
-   Super performance
-   Zero dependencies
-   Fully written in ES5
-   Can react to DOM changes
-   Small size ~1.8Kb (minified gzip)

## Demo

-   [Basic usage](https://codepen.io/kovart/pen/VReGjN)
-   [Usage with JQuery or Zepto](https://codepen.io/kovart/pen/OqMrvR)
-   [Same selector for multiple items](https://codepen.io/kovart/pen/eXJxQY)

More examples in [`this folder`](./examples).

## Installation

You can simply install the library from CDN, NPM, Yarn or just download it from this repo.

### CDN

Just insert the proper version of the library into your page:

```html
<script src="https://cdn.jsdelivr.net/npm/sticksy@0.2.0/dist/sticksy.min.js"></script>
```

### NPM

```sh
$ npm install sticksy --save
```

### Yarn

```sh
$ yarn add sticksy
```

---

🧱 Import an entire module if you use Webpack, Rollup or other module bundlers:

```js
import 'sticksy'
```

## Usage

Watch an example.

```html
<!-- Container -->
<aside class="sidebar">
    <!-- Non sticky element -->
    <div class="widget"></div>
    <!-- Sticky element -->
    <div class="widget js-sticky-widget"></div>
    <div class="widget"></div>
    <div class="widget"></div>
</aside>
```

_⚠️ The container shouldn't be absolutely positioned as we use absolute position to stuck the elements to the bottom._

Then you should initialize an instance with a **new** keyword (it's important):

```js
var stickyEl = new Sticksy('.js-sticky-widget')
// just for demonstration of state handling
stickyEl.onStateChanged = function (state) {
    if (state === 'fixed') stickyEl.nodeRef.classList.add('widget--fixed')
    else stickyEl.nodeRef.classList.remove('widget--fixed')
}
```

That's all 😎

Also, you can directly pass the target node:

```js
var stickyEl = new Sticksy(document.getElementById('sticky-widget'))
```

#### Via JQuery/Zepto:

```js
var stickyEl = $('.widget.js-sticky-widget').sticksy({ topSpacing: 60, listen: true })
```

---

#### Initialize all sticky elements

You can add the one class to all the target elements and initialize them all in one line:

```html
<aside class="sidebar">
    <div class="widget"></div>
    <!-- Sticky element -->
    <div class="widget js-sticky-widget"></div>
</aside>
<main>
    <!-- Some content here -->
</main>
<aside class="sidebar">
    <!-- Sticky element -->
    <div class="widget js-sticky-widget"></div>
    <div class="widget"></div>
</aside>
```

```js
var stickyElements = Sticksy.initializeAll('.js-sticky-widget')
```

#### Enable reaction to DOM changes

The library can detect changes of the container and its children by using <a href="https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver" target="_blank">MutationObserver</a>. To enable this behavior, you have to specify `listen` option.

```js
var stickyEl = new Sticksy('.js-sticky-widget', {
    listen: true, // Listen for the DOM changes in the container
})
```

> Beware! Since the library uses style attribute to change elements position,
> it ignores changes of width and height properties of style attribute.
> Use CSS classes instead.

---

More examples in [`example folder`](https://github.com/kovart/sticksy/tree/master/examples) and [`this section`](#demo).

## API

The API is as simple as possible.

### Constructor options

```js
var instance = new Sticksy(target[, options]);
```

-   `target` _(String | Element)_ target element or query string
-   `options` _(ContructorOptions)_ all options below are optional
    -   `topSpacing`: _(Number)_ additional top spacing for the top sticky element when it becomes fixed (sticky). Use this option when you have a fixed top panel. `Optional` | `Default: 0`
    -   `listen`: _(Boolean)_ should we recalculate all cached dimensions of the viewport, container and sticky elements on any DOM changes in the container element. `Optional` | `Default: false`
-   **_Returns:_** [`Instance`](#instance-object)

Example:

```js
var stickyEl = new Sticksy('.block.js-sticky-widget', {
    topSpacing: 60, // Specify this when you have a fixed top panel
    listen: true, // Listen for the DOM changes in the container
})
```

### Instance object

### Properties

-   `nodeRef` _(Element)_ a direct reference to the DOM element

```js
stickyEl.onStateChanged = function (state) {
    if (state === 'fixed') stickyEl.nodeRef.classList.add('widget--fixed')
    else stickyEl.nodeRef.classList.remove('widget--fixed')
}
```

### Methods

#### refresh(): void

Recalculate and update the element according to the new state.

```js
stickyEl.refresh()
```

#### hardRefresh(): void

Recalculate all cached dimensions of the viewport, container and sticky elements and update the element according to the new state. Use it for manual refreshing, for example, if you haven't specified `listen` option, but you have to deal with some DOM manipulations.

```js
stickyEl.hardRefresh()
```

#### enable(): void

Enable `'sticky'` effect.

```js
stickyEl.enable()
```

#### disable(): void

Make the element static.

```js
stickyEl.disable()
```

### Events

#### onStateChanged

Triggered when the state of the element has changed.
The state can be: `static`, `fixed` and `stuck`.

```js
stickyEl.onStateChanged = function (state) {
    // your handler here
    if (state === 'fixed') alert('it is fixed!')
}
```

---

### Static methods

#### refreshAll(): void

Call `refresh()` method for the initialized instances.

```js
Sticksy.refreshAll()
```

#### hardRefreshAll(): void

Call `hardRefresh()` method for the initialized instances.

```js
Sticksy.hardRefreshAll()
```

#### enableAll(): void

Call `enable()` method for all the initialized instances.

```js
Sticksy.enableAll()
```

#### disableAll(): void

Call `disable()` method for all the initialized instances.

```js
Sticksy.disableAll()
```

### Helper methods

#### initializeAll(target[, options][, ignorenothingfound])

Find and initialize all the elements with the same options. By default, it doesn't throw an error if nothing found.

-   `target` _(String | Element | Element[] | jQuery)_ target element(s) or query string. `Required`
-   `options` _[(ContructorOptions)](#constructor-options)_ options for the target elements. `Optional`
-   `ignoreNothingFound` _(Boolean)_ should we throw an error if no match is found. `Default: true`

-   **_Returns_:** [`Instance`](#instance-object)

Example:

```js
var stickyElems = Sticksy.initializeAll('.js-sticky-widget', { listen: true }, true)
```

## Performance

Performance is ultra high. ⚡

The library uses the simplest function to calculate the elements state:

```js
Sticksy.prototype._calcState = function (windowOffset) {
    if (windowOffset < this._limits.top) {
        return States.STATIC
    } else if (windowOffset >= this._limits.bottom) {
        return States.STUCK
    }
    return States.FIXED
}
```

The function doesn't have any complicated calculations. It just compares two variables. Not more. \
If the calculated state is **the same** as previous the library **does nothing**.

Cool, right? 😃

## Browser Compatibility

Sticksy.js works in all modern browsers including Internet Explorer 11.\
If you want the library to react to DOM changes and need to support IE10 or below,
you should install [`Mutation Observer Polyfill`](https://github.com/megawac/MutationObserver.js).

Please, open an issue if you have any browser compatibility problems.

## License (MIT)

```
WWWWWW||WWWWWW
 W W W||W W W
      ||
    ( OO )__________
     /  |           \
    /o o|    MIT     \
    \___/||_||__||_|| *
         || ||  || ||
        _||_|| _||_||
       (__|__|(__|__|
```

Sticksy.js is released under the MIT license.\
Copyright (c) 2019-present Artem Kovalchuk
