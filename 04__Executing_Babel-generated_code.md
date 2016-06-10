---
title: Executing Babel-generated code
---

So you've compiled your code with Babel, but this is not the end of the story.

## `babel-polyfill`

Almost all futuristic JavaScript syntax can be compiled with Babel, but the same
is not true for APIs.

For example, the following code has an arrow function that needs to be compiled:

```js
function addAll() {
  return Array.from(arguments).reduce((a, b) => a + b);
}
```

Which turns into this:

```js
function addAll() {
  return Array.from(arguments).reduce(function(a, b) {
    return a + b;
  });
}
```

However, this still won't work everywhere because `Array.from` doesn't exist
in every JavaScript environment.

```
Uncaught TypeError: Array.from is not a function
```

To solve this problem we use something called a
[Polyfill](https://remysharp.com/2010/10/08/what-is-a-polyfill). Simply put, a
polyfill is a piece of code that replicates a native api that does not exist in
the current runtime. Allowing you to use APIs such as `Array.from` before they
are available.

Babel uses the excellent [core-js](https://github.com/zloirock/core-js) as its
polyfill, along with a customized
[regenerator](https://github.com/facebook/regenerator) runtime for getting
generators and async functions working.

To include the Babel polyfill, first install it with npm:

```sh
$ npm install --save babel-polyfill
```

Then simply include the polyfill at the top of any file that requires it:

```js
import "babel-polyfill";
```

## `babel-runtime`

In order to implement details of ECMAScript specs, Babel will use "helper"
methods in order to keep the generated code clean.

Since these helpers can get pretty long, and they get added to the top of every
file you can move them into a single "runtime" which gets required.

Start by installing `babel-plugin-transform-runtime` and `babel-runtime`:

```sh
$ npm install --save-dev babel-plugin-transform-runtime
$ npm install --save babel-runtime
```

Then update your `.babelrc`:

```diff
  {
    "plugins": [
+     "transform-runtime",
      "transform-es2015-classes"
    ]
  }
```

Now Babel will compile code like the following:

```js
class Foo {
  method() {}
}
```

Into this:

```js
import _classCallCheck from "babel-runtime/helpers/classCallCheck";
import _createClass from "babel-runtime/helpers/createClass";

let Foo = function () {
  function Foo() {
    _classCallCheck(this, Foo);
  }

  _createClass(Foo, [{
    key: "method",
    value: function method() {}
  }]);

  return Foo;
}();
```

Rather than putting the `_classCallCheck` and `_createClass` helpers in every
single file where they are needed.
