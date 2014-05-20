#Argumenter [![Build Status](https://travis-ci.org/trecenti/argumenter.png?branch=master)](https://travis-ci.org/Trecenti/argumenter) [![Build Status](https://snap-ci.com/Trecenti/argumenter/branch/master/build_image)](https://snap-ci.com/Trecenti/argumenter/branch/master/)

A descriptive way to handle function arguments.

##By Arguments Type
```js
var argumenter = require('argumenter');
function myFn(opt_fn, opt_options) {
  var handler = argumenter(myFn);

  handler
    .when([Function, Object], function (fn, options) {
      //do something with fn and options;
    }).when(Object, function (options) {
      //do something with options
    }).when(Function, function (fn) {
      //do something with fn
    });

  return handler.done();
}
```

##By Arguments Length

```js
var argumenter = require('argumenter');
function myFn() {
  var handler = argumenter(myFn);

  handler
    .when(2, function (fn, options) {
      //do something with fn and options;
    }).when(1, function (any) {
      //do something with any
    }).when(0, function () {
      //do something
    });

  return handler.done();
}
```

##Returning the execution
```js
var argumenter = require('argumenter');
function myFn() {
  var handler = argumenter(myFn);

  handler
    .when(2, function (a, b) {
      return a + b;
    }).when(1, function (a) {
      return a;
    }).when(0, function () {
      return 0;
    });

  return handler.done();
}

myFn(1, 1); // 2
myFn(1); // 1
myFn(); // 0
```

##Spreading Array Arguments
```js
var argumenter = require('argumenter');
var context = {};

function myFn(array) {
  var handler = argumenter(myFn);

  handler.spread(0).spread(2)
    .when([Object, Function, Number, Array], function (obj, fn, number, array) {
      //do something with obj, fn, number and array
    });

  return handler.done();
}
myFn([{}, function () {}], 1, [[1,2]]); // will be handled as myFn({}, function() {}, 1, [1, 2])
```

##Context Binding
```js
var argumenter = require('argumenter');
var context = {};

function myFn() {
  var handler = argumenter(myFn);

  handler
    .when(1, function (arg) {
      return this; // this === context
    });

  return handler.done(context);
}
```
