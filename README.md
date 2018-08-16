Similarities and differences between Polyglot.js and I18n-2.js
=======

*Polyglot.js and I18n-2.js can achieve the function of translation.
*Ployglot.js and I18n-2.js possess lightweight simple translation module with dynamic json storage.

## Installation 

Run the following:
```
npm install node-polyglot
npm install i18n-2
```
## Simple Example

### polyglot Demo

```js
var polyglot = new Polyglot();

ployglot.extend({
  "hello": "Hello"
});

ployglot.t("hello");  // return a string "Hello"
```

### I18n-2 Demo

```js
// Load Module and Instantiate
var i18n = new (require('i18n-2'))({
  // setup some locales - other locales default to the first locale
  locales: ['en', 'de'] //  English is default language
});

// Use it however you wish
console.log(i18n.__("Hello")); // return a string "Hello"
```
