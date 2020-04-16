---
title: "JavaScript Cheatsheet"
excerpt: "My cheatsheet for JavaScript"
categories: [JavaScript]
toc: true
---

## Naming conventions

- local variables: `camelCase`
- global variables: `UPPERCASE_WITH_UNDERSCORES`
- class names: `TitleCase`

## Definitions

- **Data Type**: There are 5 primitive data types:

  - `String`
  - `Number`
  - `Boolean`
  - `null`
  - `undefined`

- **Primitive**: an instance of a data type.

  - For example, for `var str1 = 'this is a string'`, `str1` is an instance of the `String` data type.

- **Literal**: the actual value either a `String`, `Number` or `Boolean`.

  - For example, for `var str1 = 'this is a string'`, `'this is a string'` is an instance of the `String` data type.

- **Objects**: provide access to built-in properties and methods for a value.
  - Only `String`, `Number` or `Boolean` have complementary objects.

## Miscellaneous notes

- ECMAScript is the official specification

- it is implemented in different environments +web, server, etc) as JS

- JS is written in C++ in Chrome's V8 engine and MDN's firefox

* transpiling - create new syntax from old

* polyfill - create new APIs from old

- use `` (backticks) for interpolation: `Hello ${ Name }`
