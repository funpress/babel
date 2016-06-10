---
title: Setting up Babel
---

Since the JavaScript community has no single build tool, framework, platform,
etc., Babel has official integrations for all of the major tooling. Everything
from Gulp to Browserify, from Ember to Meteor, no matter what your setup looks
like there is probably an official integration.

For the purposes of this handbook, we're just going to cover the built-in ways
of setting up Babel, but you can also visit the interactive
[setup page](http://babeljs.io/docs/setup) for all of the integrations.

> **Note:** This guide is going to refer to command line tools like `node` and
> `npm`. Before continuing any further you should be comfortable with these
> tools.

## `babel-cli`

Babel's CLI is a simple way to compile files with Babel from the command line.

Let's first install it globally to learn the basics.

```sh
$ npm install --global babel-cli
```

We can compile our first file like so:

```sh
$ babel my-file.js
```

This will dump the compiled output directly into your terminal. To write it to
a file we'll specify an `--out-file` or `-o`.

```sh
$ babel example.js --out-file compiled.js
# or
$ babel example.js -o compiled.js
```

If we want to compile a whole directory into a new directory we can do so using
`--out-dir` or `-d`.

```sh
$ babel src --out-dir lib
# or
$ babel src -d lib
```

### Running Babel CLI from within a project

While you _can_ install Babel CLI globally on your machine, it's much better
to install it **locally** project by project.

There are two primary reasons for this.

  1. Different projects on the same machine can depend on different versions of
     Babel allowing you to update one at a time.
  2. It means you do not have an implicit dependency on the environment you are
     working in. Making your project far more portable and easier to setup.

We can install Babel CLI locally by running:

```sh
$ npm install --save-dev babel-cli
```

> **Note:** Since it's generally a bad idea to run Babel globally you may want
> to uninstall the global copy by running:
>
> ```sh
> $ npm uninstall --global babel-cli
> ```

After that finishes installing, your `package.json` file should look like this:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "devDependencies": {
    "babel-cli": "^6.0.0"
  }
}
```

Now instead of running Babel directly from the command line we're going to put
our commands in **npm scripts** which will use our local version.

Simply add a `"scripts"` field to your `package.json` and put the babel command
inside there as `build`.

```diff
  {
    "name": "my-project",
    "version": "1.0.0",
+   "scripts": {
+     "build": "babel src -d lib"
+   },
    "devDependencies": {
      "babel-cli": "^6.0.0"
    }
  }
```

Now from our terminal we can run:

```js
npm run build
```

This will run Babel the same way as before, only now we are using a local copy.

## `babel-register`

The next most common method of running Babel is through `babel-register`. This
option will allow you to run Babel just by requiring files, which may integrate
with your setup better.

Note that this is not meant for production use. It's considered bad practice to
deploy code that gets compiled this way. It is far better to compile ahead of
time before deploying. However this works quite well for build scripts or other
things that you run locally.

First let's create an `index.js` file in our project.

```js
console.log("Hello world!");
```

If we were to run this with `node index.js` this wouldn't be compiled with
Babel. So instead of doing that, we'll setup `babel-register`.

First install `babel-register`.

```sh
$ npm install --save-dev babel-register
```

Next, create a `register.js` file in the project and write the following code:

```js
require("babel-register");
require("./index.js");
```

What this does is *registers* Babel in Node's module system and begins compiling
every file that is `require`'d.

Now, instead of running `node index.js` we can use `register.js` instead.

```sh
$ node register.js
```

> **Note:** You can't register Babel in the same file that you want to compile.
> As node is executing the file before Babel has a chance to compile it.
>
> ```js
> require("babel-register");
> // not compiled:
> console.log("Hello world!");
> ```

## `babel-node`

If you are just running some code via the `node` CLI the easiest way to
integrate Babel might be to use the `babel-node` CLI which largely is just a
drop in replacement for the `node` CLI.

Note that this is not meant for production use. It's considered bad practice to
deploy code that gets compiled this way. It is far better to compile ahead of
time before deploying. However this works quite well for build scripts or other
things that you run locally.

First make sure that you have `babel-cli` installed.

```sh
$ npm install --save-dev babel-cli
```

> **Note:** If you are wondering why we are installing this locally, please read
> the [Running Babel CLI from within a project](#running-babel-cli--from-within-a-project)
> section above.

Then replace wherever you are running `node` with `babel-node`.

If you are using npm `scripts` you can simply do:

```diff
  {
    "scripts": {
-     "script-name": "node script.js"
+     "script-name": "babel-node script.js"
    }
  }
```

Otherwise you'll need to write out the path to `babel-node` itself.

```diff
- node script.js
+ ./node_modules/.bin/babel-node script.js
```

> Tip: You can also use [`npm-run`](https://www.npmjs.com/package/npm-run).

## `babel-core`

If you need to use Babel programmatically for some reason, you can use the
`babel-core` package itself.

First install `babel-core`.

```sh
$ npm install babel-core
```

```js
var babel = require("babel-core");
```

If you have a string of JavaScript you can compile it directly using
`babel.transform`.

```js
babel.transform("code();", options);
// => { code, map, ast }
```

If you are working with files you can use either the asynchronous api:

```js
babel.transformFile("filename.js", options, function(err, result) {
  result; // => { code, map, ast }
});
```

Or the synchronous api:

```js
babel.transformFileSync("filename.js", options);
// => { code, map, ast }
```

If you already have a Babel AST for whatever reason you may transform from the
AST directly.

```js
babel.transformFromAst(ast, code, options);
// => { code, map, ast }
```

For all of the above methods, `options` refers to
http://babeljs.io/docs/usage/options/.
