---
layout: post
title: "[HL-14] A question about ES6 Promise"
categories: 
    - frontend
tags: 
    - ES6
    - Promise
published: true
---

Recently, I met an issue when using ES6 Promise. This leads me to do more research about `Promise` and try to fully understand this concept in `JavaScript`. A demo code is shown as follows: 

<script async src="//jsfiddle.net/hengfengli/p9xr0j3z/11/embed/js/"></script>

The output: 

```sh
"outer-outer-outer begins"
"outer-outer begins"
"outer-outer ends"
"outer-outer-outer continuing"
"outer-outer-outer ends"
--- after 3 seconds ---
"outer begins!"
"outer ends!"
"response!"
```

So, I have several questions about the above output: 

* Q1: why are `"outer-outer begins"` and `"outer-outer ends"` executed in declaring time instead of calling time? 
* Q2: why does `"response!"` appear after `"outer ends!"`? 

# Q1. The executor is called immediately

When we create a `Promise`, we pass an executor with two function arguments, `resolve` and `reject`. From [Mozilla's definition](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), the executor will immediately be called even before returning the created `Promise` object. Therefore, it is not difficult to understand why Q1 happens in the example. 

Besides, I tried to comment the code that uses `Promise`: 

<script async src="//jsfiddle.net/hengfengli/p9xr0j3z/12/embed/"></script>

I get the following output: 

```sh
"outer-outer-outer begins"
"outer-outer begins"
"outer-outer ends"
"outer-outer-outer continuing"
"outer-outer-outer ends"
--- after 3 seconds ---
"outer begins!"
"outer ends!"
```

The async code inside the executor will also be executed. But why `resolve` is not called? Because the executor doesn't know the details of `resolve` yet. 

# Q2. Resolve the Promise

From above two examples, we can see that `resolve('response!')` will be executed when calling `.then()` on the created `Promise` object. `Promise` is resolved when `resolve` in the executor is called. 

Inside the executor, the call of `resolve` tells the `Promise` object that this promise has been resolved. 

At the declaring time, the executor doesn't know the details of `resolve`, so its goals are to figure out can this promise be resolved? What arguments should be passed to the `resolve` function? 

The time to know these info can happen in the future. When we know that the promise is resolved (`resolve` is called), the function in `.then()` will be executed. 

Note: Remember that `.then()` is async. Don't expect it to execute in a sync way. 

# References

* [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* [https://developers.google.com/web/fundamentals/getting-started/primers/promises](https://developers.google.com/web/fundamentals/getting-started/primers/promises)
* [https://scotch.io/tutorials/javascript-promises-for-dummies](https://scotch.io/tutorials/javascript-promises-for-dummies)

* [https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)