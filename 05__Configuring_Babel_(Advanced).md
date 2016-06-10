---
title: Configuring Babel (Advanced)
---

Most people can get by using Babel with just the built-in presets, but Babel
exposes much finer-grained power than that.

## Manually specifying plugins

Babel presets are simply collections of pre-configured plugins, if you want to
do something differently you manually specify plugins. This works almost exactly
the same way as presets.

First install a plugin:

```sh
$ npm install --save-dev babel-plugin-transform-es2015-classes
```

Then add the `plugins` field to your `.babelrc`.

```diff
  {
+   "plugins": [
+     "transform-es2015-classes"
+   ]
  }
```

This gives you much finer grained control over the exact transforms you are
running.

For a full list of official plugins see the
[Babel Plugins page](http://babeljs.io/docs/plugins/).

Also take a look at all the plugins that have been
[built by the community](https://www.npmjs.com/search?q=babel-plugin). If you
would like to learn how to write your own plugin read the
[Babel Plugin Handbook](plugin-handbook.md).

## Plugin options

Many plugins also have options to configure them to behave differently. For
example, many transforms have a "loose" mode which drops some spec behavior in
favor of simpler and more performant generated code.

To add options to a plugin, simply make the following change:

```diff
  {
    "plugins": [
-     "transform-es2015-classes"
+     ["transform-es2015-classes", { "loose": true }]
    ]
  }
```

> I'll be working on updates to the plugin documentation to detail every option
> in the coming weeks.
> [Follow me for updates](https://twitter.com/thejameskyle).

## Customizing Babel based on environment

Babel plugins solve many different tasks. Many of them are development tools
that can help you debugging your code or integrate with tools. There are also
a lot of plugins that are meant for optimizing your code in production.

For this reason, it is common to want Babel configuration based on the
environment. You can do this easily with your `.babelrc` file.

```diff
  {
    "presets": ["es2015"],
    "plugins": [],
+   "env": {
+     "development": {
+       "plugins": [...]
+     },
+     "production": {
+       "plugins": [...]
+     }
    }
  }
```

Babel will enable configuration inside of `env` based on the current
environment.

The current environment will use `process.env.BABEL_ENV`. When `BABEL_ENV` is
not available, it will fallback to `NODE_ENV`, and if that is not available it
will default to `"development"`.

**Unix**

```sh
$ BABEL_ENV=production [COMMAND]
$ NODE_ENV=production [COMMAND]
```

**Windows**

```sh
$ SET BABEL_ENV=production
$ [COMMAND]
```

> **Note:** `[COMMAND]` is whatever you use to run Babel (ie. `babel`,
> `babel-node`, or maybe just `node` if you are using the register hook).
>
> **Tip:** If you want your command to work across unix and windows platforms
> then use [`cross-env`](https://www.npmjs.com/package/cross-env).

## Making your own preset

Manually specifying plugins? Plugin options? Environment-based settings? All
this configuration might seem like a ton of repetition for all of your projects.

For this reason, we encourage the community to create their own presets. This
could be a preset for the specific
[node version](https://github.com/leebenson/babel-preset-node5) you are running,
or maybe a preset for your
[entire](https://github.com/cloudflare/babel-preset-cf)
[company](https://github.com/airbnb/babel-preset-airbnb).

It's easy to create a preset. Say you have this `.babelrc` file:

```js
{
  "presets": [
    "es2015",
    "react"
  ],
  "plugins": [
    "transform-flow-strip-types"
  ]
}
```

All you need to do is create a new project following the naming convention
`babel-preset-*` (please be responsible with this namespace), and create two
files.

First, create a new `package.json` file with the necessary `dependencies` for
your preset.

```js
{
  "name": "babel-preset-my-awesome-preset",
  "version": "1.0.0",
  "author": "James Kyle <me@thejameskyle.com>",
  "dependencies": {
    "babel-preset-es2015": "^6.3.13",
    "babel-preset-react": "^6.3.13",
    "babel-plugin-transform-flow-strip-types": "^6.3.15"
  }
}
```

Then create an `index.js` file that exports the contents of your `.babelrc`
file, replacing plugin/preset strings with `require` calls.

```js
module.exports = {
  presets: [
    require("babel-preset-es2015"),
    require("babel-preset-react")
  ],
  plugins: [
    require("babel-plugin-transform-flow-strip-types")
  ]
};
```

Then simply publish this to npm and you can use it like you would any preset.
