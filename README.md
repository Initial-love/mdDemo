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

```en.js
  //en.js is locales onload resources that is key-value json data and uesd in the following demo.
 {
  "Hello": "Hello",
  "Hello %s, how are you today?": "Hello %s, how are you today?",
  "weekend": "weekend",
  "Hello %s, how are you today? How was your %s.": "Hello %s, how are you today? How was your %s.",
  "Hi": "Hi",
  "Howdy": "Howdy",
  "%s cat": {
    "one": "%s cat",
    "other": "%s cats"
  },
  "There is one monkey in the %%s": {
    "one": "There is one monkey in the %%s",
    "other": "There are %d monkeys in the %%s"
  },
  "tree": "tree"
}

```

```js
// Load Module and Instantiate
var i18n = new (require('i18n-2'))({
  // setup some locales - other locales default to the first locale
  locales: ['en', 'de'] //  English is default language
});

// Use it however you wish
console.log(i18n.__("Hello")); // return a string "Hello"
```

## API 

`Polyglot.t()` also provides interpolation. Pass an object with key-value pairs of interpolation arguments as the second parameter.

```js
polyglot.extend({
  "hello_name": "Hola, %{name}."
});

polyglot.t("hello_name", {name: "DeNiro"}); // "Hola,DeNiro."
```

Polyglot also supports nested phrase objects.

```js
polyglot.extend({
  "nav": {
      "hello": "Hello",
      "hello_name": "Hello, %{name}",
      "sidebar": {
          "welcome": "Welcome"
      }
   }
});

polyglot.t("nav.sidebar.welcome"); // "Welcome"
```

`__(string, [...])` Translates a string according to the current locale. Also supports sprintf syntax, allowing you to replace text, using the `node-sprintf` module. 

```javascript
var greeting = i18n.__('Hello %s, how are you today?', 'Marcus'); // greeting is "Hello Marcus, how are you today?"
greeting = i18n.__('Hello %s, how are you today? How was your %s?', 'Marcus', i18n.__('weekend')); // greeting is "Hello Marcus, how are you today? How was your weekend?"
```

You can also use nested object :
```json
{
  "foo": {
      "bar": "ok"
  }
}
```
And use not dot notation to acccess the value :
``` javascript 
i18n.__('foo.bar'); // return a string "ok"


