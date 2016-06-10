---
title: Configuring Babel
---

You may have noticed by now that running Babel on its own doesn't seem to do
anything other than copy JavaScript files from one location to another.

This is because we haven't told Babel to do anything yet.

> Since Babel is a general purpose compiler that gets used in a myriad of
> different ways, it doesn't do anything by default. You have to explicitly tell
> Babel what it should be doing.

You can give Babel instructions on what to do by installing **plugins** or
**presets** (groups of plugins).

## `.babelrc`

Before we start telling Babel what to do. We need to create a configuration
file. All you need to do is create a `.babelrc` file at the root of your
project. Start off with it like this:

```js
{
  "presets": [],
  "plugins": []
}
```

This file is how you configure Babel to do what you want.

> **Note:** While you can also pass options to Babel in other ways the
> `.babelrc` file is convention and is the best way.

## `babel-preset-es2015`

Let's start by telling Babel to compile ES2015 (the newest version of the
JavaScript standard, also known as ES6) to ES5 (the version available in most
JavaScript environments today).

We'll do this by installing the "es2015" Babel preset:

```sh
$ npm install --save-dev babel-preset-es2015
```

Next we'll modify our `.babelrc` to include that preset.

```diff
  {
    "presets": [
+     "es2015"
    ],
    "plugins": []
  }
```

## `babel-preset-react`

Setting up React is just as easy. Just install the preset:

```sh
$ npm install --save-dev babel-preset-react
```

Then add the preset to your `.babelrc` file:

```diff
  {
    "presets": [
      "es2015",
+     "react"
    ],
    "plugins": []
  }
```

## `babel-preset-stage-x`

JavaScript also has some proposals that are making their way into the standard
through the TC39's (the technical committee behind the ECMAScript standard)
process.

This process is broken through a 5 stage (0-4) process. As proposals gain more
traction and are more likely to be accepted into the standard they proceed
through the various stages, finally being accepted into the standard at stage 4.

These are bundled in babel as 4 different presets:

- `babel-preset-stage-0`
- `babel-preset-stage-1`
- `babel-preset-stage-2`
- `babel-preset-stage-3`

> Note that there is no stage-4 preset as it is simply the `es2015` preset
> above.

Each of these presets requires the preset for the later stages. i.e.
`babel-preset-stage-1` requires `babel-preset-stage-2` which requires
`babel-preset-stage-3`.

Simply install the stage you are interested in using:

```sh
$ npm install --save-dev babel-preset-stage-2
```

Then you can add it to your `.babelrc` config.

```diff
  {
    "presets": [
      "es2015",
      "react",
+     "stage-2"
    ],
    "plugins": []
  }
```
