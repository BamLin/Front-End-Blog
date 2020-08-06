

## call 、 apply 、 bind





### call

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call



`call()` 方法使用一个指定的 `this` 值和单独给出的一个或多个参数来调用一个函数。



**注意：**该方法的语法和作用与 [`apply()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法类似，只有一个区别，就是 `call()` 方法接受的是**一个参数列表**，而 `apply()` 方法接受的是**一个包含多个参数的数组**。

* 使用 `call` 方法调用函数并且指定上下文的 '`this`'

```js
// 在下面的例子中，当调用 greet 方法的时候，该方法的this值会绑定到 obj 对象。
function greet() {
  var reply = [this.animal, 'typically sleep between', this.sleepDuration].join(' ');
  console.log(reply);
}

var obj = {
  animal: 'cats', sleepDuration: '12 and 16 hours'
};

greet.call(obj);  // cats typically sleep between 12 and 16 hours
```



* 使用  `call` ` 方法调用函数并且不指定第一个参数（`argument`）

```js
// 在下面的例子中，我们调用了 `display` 方法，但并没有传递它的第一个参数。如果没有传递第一个参数，`this` 的值将会被绑定为全局对象。

var sData = 'Wisen';

function display() {
  console.log('sData value is %s ', this.sData);
}

display.call();  // sData value is Wisen
```

**注意：**在严格模式下，`this` 的值将会是 `undefined`。见下文。

```js
'use strict';

var sData = 'Wisen';

function display() {
  console.log('sData value is %s ', this.sData);
}

display.call(); // Cannot read the property of 'sData' of undefined
```





### apply











### bind