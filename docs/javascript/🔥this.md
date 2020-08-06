





https://zhuanlan.zhihu.com/p/24107744





#### 一些例子 -- 不错



* #### this 与 setTimeout

https://zhuanlan.zhihu.com/p/24107744

```js
var obj = {
  name: 'qiutc',
  foo: function() {
    console.log(this);
  },
  foo2: function() {
    console.log(this);
    setTimeout(this.foo, 1000);
  }
}
obj.foo2();
```

两次打印不一样。

第一次 foo2中的打印的 this 指向 “obj” 这个对象；

第二次 foo1中的打印的 this 指向 全局对象。setTimeout只是一个函数，函数必然有可能需要参数，“this.foo“ 当作一个参数传给了 setTimeout 这个函数，一个 ”function“ 类型的 参数 fun ，  在传入参数的时候，执行了一个 ”fun = this.foo" 的操作，直接将 fun 指向了 this.foo 的引用，执行的时候其实执行的是 fun() （类似前面的例子， var fun = obj.foo;   func(); 只是简单的赋值，被当作一个普通函数调用，与obj无关  ），已经和 `obj` 无关了，它是被当作普通函数直接调用的，因此 `this` 指向全局对象。

解决使用setTimeout的时候这个问题，使用 闭包：

```js
var obj = {
  name: 'qiutc',
  foo: function() {
    console.log(this);
  },
  foo2: function() {
    console.log(this);
    var _this = this;
    setTimeout(function() {
      console.log(this);  // Window
      console.log(_this);  // Object {name: "qiutc"}
    }, 1000);
  }
}
obj.foo2();
```

可以看到直接用 `this` 仍然是 `Window`；因为 `foo2` 中的 `this` 是指向 `obj`，我们可以先用一个变量 `_this` 来储存，然后在回调函数中使用 `_this`，就可以指向当前的这个对象了；

##### setTimeout 的另一个坑

之前说过，如果直接执行回调函数而没有绑定作用域，那么它的 `this` 是指向全局对象(`window`)，在严格模式下会指向 `undefined`，然而在 `setTimeout` 中的回调函数在严格模式下却表现出不同：

```js
'use strict';
function foo() {
  console.log(this);
}
setTimeout(foo, 1);
// window （ 正常来说，严格模式下，函数被直接调用，属性全局调用，this不指向全局对象，而是 undefined ）
```

按理说我们加了严格模式，foo 调用也没有指定 `this`，应该是出来 `undefined`，但是这里仍然出现了全局对象，难道是严格模式失效了吗？

并不，即使在严格模式下，`setTimeout` 方法在调用传入函数的时候，如果这个函数没有指定了的 `this`，那么它会做一个隐式的操作----自动地注入全局上下文，等同于调用 `foo.apply(window)` 而非 `foo()`；

当然，如果我们在传入函数的时候已经指定 `this`，那么就不会被注入全局对象，比如： `setTimeout(foo.bind(obj), 1);`；



PS： apply

**红宝书  5.5.5 函数属性和方法** （第五章 引用类型）

每个函数都包含了两个非继承而来的方法： apply() 和 call()，用途都是在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值。

* 首先,apply()方法接收两个参数:一个是在其中运行函数的作用域,另一个是参数数组。其中,第二个参数可以是 Array 的实例,也可以是arguments 对象。

* call()方法与 apply()方法的作用相同,它们的区别仅在于接收参数的方式不同。对于 call() 方法而言,第一个参数是 this 值没有变化,变化的是其余参数都直接传递给函数。换句话说,在使用call()方法时,传递给函数的参数必须逐个列举出来

**<u>事实上,传递参数并非 apply()和 call()真正的用武之地;它们真正强大的地方是能够扩充函数赖以运行的作用域</u>**。下面来看一个例子。

```js
window.color = "red"; 
var o = { color: "blue" };  
function sayColor(){     
  alert(this.color); 
}  
sayColor();                //red  
sayColor.call(this);       //red 
sayColor.call(window);     //red 
sayColor.call(o);          //blue
```

**<u>使用 call()(或 apply())来扩充作用域的最大好处,就是对象不需要与方法有任何耦合关系。</u>**
在前面例子的第一个版本中, 我们是先将 sayColor()函数放到了对象 o 中, 然后再通过 o 来调用它的; 而在这里重写的例子中,就不需要先前那个多余的步骤了。

* ECMAScript 5 还定义了一个方法: **<u>bind()</u>**。这个方法会<u>**创建一个函数的实例,其 this 值会被绑定到传给 bind()函数的值**</u>。例如:

```js
window.color = "red"; 
var o = { color: "blue" };  
function sayColor(){     
  alert(this.color); 
}  
var objectSayColor = sayColor.bind(o); 
objectSayColor();    //blue
```











