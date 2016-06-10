---
title: Babel and other tools
---

Babel is pretty straight forward to setup once you get the hang of it, but it
can be rather difficult navigating how to set it up with other tools. However,
we try to work closely with other projects in order to make the experience as
easy as possible.

## Static analysis tools

Newer standards bring a lot of new syntax to the language and static analysis
tools are just starting to take advantage of it.

### Linting

One of the most popular tools for linting is [ESLint](http://eslint.org),
because of this we maintain an official
[`babel-eslint`](https://github.com/babel/babel-eslint) integration.

First install `eslint` and `babel-eslint`.

```sh
$ npm install --save-dev eslint babel-eslint
```

> **Note:** `babel-eslint` compatibility with Babel 6 is currently in a
> pre-release version. Install the
> [latest](https://github.com/babel/babel-eslint/releases) 5.0 beta in order to
> use it with Babel 6.

Next create or use the existing `.eslintrc` file in your project and set the
`parser` as `babel-eslint`.

```diff
  {
+   "parser": "babel-eslint",
    "rules": {
      ...
    }
  }
```

Now add a `lint` task to your npm `package.json` scripts:

```diff
  {
    "name": "my-module",
    "scripts": {
+     "lint": "eslint my-files.js"
    },
    "devDependencies": {
      "babel-eslint": "...",
      "eslint": "..."
    }
  }
```

Then just run the task and you will be all setup.

```sh
$ npm run lint
```

For more information consult the
[`babel-eslint`](https://github.com/babel/babel-eslint) or
[`eslint`](http://eslint.org) documentation.

### Code Style

JSCS is an extremely popular tool for taking linting a step further into
checking the style of the code itself. A core maintainer of both the Babel and
JSCS projects ([@hzoo](https://github.com/hzoo)) maintains an official
integration with JSCS.

Even better, this integration now lives within JSCS itself under the `--esnext`
option. So integrating Babel is as easy as:

```
$ jscs . --esnext
```

From the cli, or adding the `esnext` option to your `.jscsrc` file.

```diff
  {
    "preset": "airbnb",
+   "esnext": true
  }
```

For more information consult the
[`babel-jscs`](https://github.com/jscs-dev/babel-jscs) or
[`jscs`](http://jscs.info) documentation.

<!--
### Code Coverage

> [WIP]
-->

### Documentation

Using Babel, ES2015, and Flow you can infer a lot about your code. Using
[documentation.js](http://documentation.js.org) you can generate detailed API
documentation very easily.

Documentation.js uses Babel behind the scenes to support all of the latest
syntax including Flow annotations in order to declare the types in your code.

## Frameworks

All of the major JavaScript frameworks are now focused on aligning their APIs
around the future of the language. Because of this, there has been a lot of work
going into the tooling.

Frameworks have the opportunity not just to use Babel but to extend it in ways
that improve their users' experience.

### React

React has dramatically changed their API to align with ES2015 classes
([Read about the updated API here](https://babeljs.io/blog/2015/06/07/react-on-es6-plus)).
Even further, React relies on Babel to compile it's JSX syntax, deprecating it's
own custom tooling in favor of Babel. You can start by setting up the
`babel-preset-react` package following the
[instructions above](#babel-preset-react).

The React community took Babel and ran with it. There are now a number of
transforms
[built by the community](https://www.npmjs.com/search?q=babel-plugin+react).

Most notably the
[`babel-plugin-react-transform`](https://github.com/gaearon/babel-plugin-react-transform)
plugin which combined with a number of
[React-specific transforms](https://github.com/gaearon/babel-plugin-react-transform#transforms)
can enable things like *hot module reloading* and other debugging utilities.

<!--
### Ember

> [WIP]
-->

## Text Editors and IDEs

Introducing ES2015, JSX, and Flow syntax with Babel can be helpful, but if your
text editor doesn't support it then it can be a really bad experience. For this
reason you will want to setup your text editor or IDE with a Babel plugin.

- [Sublime Text](https://github.com/babel/babel-sublime)
- [Atom](https://atom.io/packages/language-babel)
- [Vim](https://github.com/jbgutierrez/vim-babel)
- [WebStorm](https://babeljs.io/docs/setup/#webstorm)
