---
title: Introduction
---

Babel is a generic multi-purpose compiler for JavaScript. Using Babel you can
use (and create) the next generation of JavaScript, as well as the next
generation of JavaScript tooling.

JavaScript as a language is constantly evolving, with new specs and proposals
coming out with new features all the time. Using Babel will allow you to use
many of these features years before they are available everywhere.

Babel does this by compiling down JavaScript code written with the latest
standards into a version that will work everywhere today. This process is known
as source-to-source compiling, also known as transpiling.

For example, Babel could transform the new ES2015 arrow function
syntax from this:

```js
const square = n => n * n;
```

Into the following:

```js
const square = function square(n) {
  return n * n;
};
```

However, Babel can do much more than this as Babel has support for syntax
extensions such as the JSX syntax for React and Flow syntax support for static
type checking.

Further than that, everything in Babel is simply a plugin and anyone can go out
and create their own plugins using the full power of Babel to do whatever
they want.

*Even further* than that, Babel is broken down into a number of core modules
that anyone can use to build the next generation of JavaScript tooling.

Many people do too, the ecosystem that has sprung up around Babel is massive and
very diverse. Throughout this handbook I'll be covering both how built-in Babel
tools work as well as some useful things from around the community.

> ***For future updates, follow [@thejameskyle](https://twitter.com/thejameskyle)
> on Twitter.***
