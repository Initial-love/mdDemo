Similarities and differences between Polyglot.js and I18n-2.js
=======

*Polyglot.js and I18n-2.js can achieve the function of translation.<br>
*Ployglot.js and I18n-2.js possess lightweight simple translation module with dynamic json storage.<br>
*Ployglot.js and I18n-2.js used API which are the most commonly is written below.<br>
*For specific function functions, please refer to the following address. Ployglot.js: (http://airbnb.github.com/polyglot.js), I18n-2.js: (http://github.com/jeresig/i18n-node-2)<br>
*Ployglot.js and I18n-2.js have many similarities, but in terms of specific implementation, they are different from the storage of stroage.

## Installation 

Run the following:
```
npm install node-polyglot
npm install i18n-2
```
## Simple Example

### polyglot Demo

#### Output key corresponding to value

```js
var polyglot = new Polyglot();

ployglot.extend({
  "hello": "Hello"
});

ployglot.t("hello");  // return a string "Hello"
```

### I18n-2 Demo

#### Output key corresponding to value

```en.js
  //en.js is locales onload resources that is key-value json data and uesd in the following all demos.
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

### Polyglot-API

`Polyglot.t()` also provides interpolation. Pass an object with key-value pairs of interpolation arguments as the second parameter.

#### Replacement of variables and deep key values

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

#### The realization of single plural in translation below.
For pluralizing "car" in English, Polyglot assumes you have a phrase of the form:

```js
polyglot.extend({
  "num_cars": "%{smart_count} car |||| %{smart_count} cars",
});
```

In English (and German, Spanish, Italian, and a few others) there are only two plural forms: singular and not-singular.

Some languages get a bit more complicated. In Czech, there are three separate forms: 1, 2 through 4, and 5 and up. Russian is even crazier.

`polyglot.t()` will choose the appropriate phrase based
on the provided `smart_count` option, whose value is a number.

```js
polyglot.t("num_cars", {smart_count: 0}); // "0 cars"

polyglot.t("num_cars", {smart_count: 1}); // "1 car"

polyglot.t("num_cars", {smart_count: 2}); // "2 cars"
```

As a shortcut, you can also pass a number to the second parameter:

```js
polyglot.t("num_cars", 2); // "2 cars"
```

#### Change translation order

Some languages cannot be said to translate in the normal order, so the string corresponding to the value adjusts the normal translation order. For instance:

```json
{
  changeDemo: 'change %{b}, %{a}, %{c}'
}
```

Tip: %{a or b or c} it can be repeated. 

```js
const jsonData = {
  a: "aaa",
  b: "bbb",
  c: "ccc
};

polyglot.t("changeDemo", jsonData); // change bbb, aaa, ccc
```

### I18n-2-API

`__(string, [...])` Translates a string according to the current locale. Also supports sprintf syntax, allowing you to replace text, using the `node-sprintf` module. 

#### Replacement of variables and deep key values

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
```

#### The realization of single plural in translation below.

`__n(one, other, count, [...])`

Different plural forms are supported as a response to `count`:
```javascript
var singular = i18n.__n('%s cat', '%s cats', 1); // "1 cat"
var plural = i18n.__n('%s cat', '%s cats', 3); // "3 cats"
```
As with `__(...)` these could be nested:
```javascript
var singular = i18n.__n('There is one monkey in the %%s', 'There are %d monkeys in the %%s', 1, 'tree'); // "There is one monkey in the tree"
var plural = i18n.__n('There is one monkey in the %%s', 'There are %d monkeys in the %%s', 3, 'tree'); // "There are 3 monkeys in the tree"
```

You may also use in your locale object/file :
```json
{
    "catEat": {
        "one": "%d cat eat the %s",
        "other": "%d cats eat the %s"
    }
}
```
and use `__n()` as you would use `__()` (directly, or from locale) :

```javascript
var singular = i18n.__n('catEat', 1, 'mouse'); // "1 cat eat the mouse"
var plural = i18n.__n('catEat', 10, 'mouse'); // "10 cats eat the mouse"
```

#### Change translation order

Some languages cannot be said to translate in the normal order, so the string corresponding to the value adjusts the normal translation order. For instance:

```json
{
  "changeDemo": "changeDemo %2$s, %1$s, %3$s",
}
```

Tip: For more format matching, please refer to [sprintf module];

```js
var str = ["str_a", "str_b", "str_c"];
// str[index] it can be repeated
var changeDemo = i18n.__n('changeDemo', 1, str[0], str[1], str[2]); // "changeDemo str_b, str_a, str_b"
```

