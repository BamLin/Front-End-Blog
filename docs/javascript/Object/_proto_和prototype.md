



### `__proto__`和`prototype`

不错的文章：

> https://github.com/creeperyang/blog/issues/9
>
> https://juejin.im/post/6844903656744189959    这篇要容易看得多



#### 知识补充

https://juejin.im/post/6844903880333983751. bind apply call **<u>*手写*</u>**

##### bind函数

> 创建一个新的函数，bind()被调用的时候，新函数的this被指定为bind()的第一个参数，而其余参数将作为新函数的参数，供调用时使用

```js
bind() 最简单的用法是创建一个函数，不论怎么调用，这个函数都有同样的 this 值。JavaScript新手经常犯的一个错误是将一个方法从对象中拿出来，然后再调用，期望方法中的 this 是原来的对象（比如在回调中传入这个方法）。如果不做特殊处理的话，一般会丢失原来的对象。基于这个函数，用原始的对象创建一个绑定函数，巧妙地解决了这个问题：

this.x = 9;    // 在浏览器中，this 指向全局的 "window" 对象
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX();   
// 返回 9 - 因为函数是在全局作用域中调用的

// 创建一个新函数，把 'this' 绑定到 module 对象
// 新手可能会将全局变量 x 与 module 的属性 x 混淆
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```



```js
function func () {}
console.log(func.prototype) // Object {}
var fn = func.bind(this)
console.log(fn.prototype)  // undefined ?

// https://segmentfault.com/q/1010000007599191/a-1020000016597540

从我的角度来理解，函数都有 prototype 属性，是因为有作为构造函数的可能性，作为构造函数，它的 this 会在 new fn() 中指向新实例。

而 bind 的函数在调用前 this 已经指向了某个对象。如果拿这个 bind 的函数作为构造函数，改变了 this 的指向，那么 bind 本身的意义不也就不存在了么？所以 bind 函数就没有作为构造函数的可能性，那么他也没有必要拥有 prototype 属性。
```



#####  call apply 作用基本相同

> call和apply两个方法的作用基本相同，它们都是为了改变某个函数**执行时的上下文**（context）而建立的， 他的真正强大之处就是能够扩充函数赖以运行的作用域。通俗一点讲，就是改变函数体内部**this 的指向**。
>
> **call 和 apply 的区别：call 方法接收的是一个参数列表，apply 方法接收的是一个包含多个参数的数组。**

* call 接收的一个参数列表

```js
window.color = "red";
var o = {color: "blue"};
function sayColor(){
	alert(this.color);
}
sayColor();//red
sayColor.call(this);//red，把函数体sayColor内部的this，绑到当前环境（作用域）(这段代码所处的环境)
sayColor.call(window);//red，把函数体sayColor内部的this，绑到window（全局作用域）
// 注意：如果call方法没有参数，或者参数为null或undefined，则等同于指向全局对象。
sayColor.call(o);//blue
```

* apply 接收的是一个包含多个参数的数组



* 手写 call、apply 函数

https://juejin.im/post/6844903880333983751

https://juejin.im/post/6856419501777846279

```js
// 手写 call	
var foo = {
	  count: 1
	};
	function bar() {
	  console.log(this.count);
	}
	bar.myCall(foo); // 1
--------------------------------------------------------------------
	Function.prototype.myCall = function(context) {
	  // 取得传入的对象（执行上下文），比如上文的foo对象，这里的context就相当于上文的foo
	  // 不传第一个参数，默认是window,
	  var context = context || window;
	  // 给context添加一个属性，这时的this指向调用myCall的函数，比如上文的bar函数
	  context.fn = this;//这里的context.fn就相当于上文的bar函数
	  // 通过展开运算符和解构赋值取出context后面的参数，上文的例子没有传入参数列表
	  var args = [...arguments].slice(1);
	  // 执行函数（相当于上文的bar(...args)）
	  var result = context.fn(...args);
	  // 删除函数
	  delete context.fn;
	  return result;
	};


// 手写 apply
var foo = {
    count: 1
};
function bar() {
    console.log(this.count);
}
bar.myApply(foo); // 1
--------------------------------------------------------------------
Function.prototype.myApply = function(context) {
      var context = context || window;
      context.fn = this;
      var result;
      // 判断第二个参数是否存在，也就是context后面有没有一个数组
      // 如果存在，则需要展开第二个参数
      if (arguments[1]) {
        result = context.fn(...arguments[1]);
      } else {
        result = context.fn();
      }
      delete context.fn;
      return result;
}
```

* 手写bind() 函数   ---    function.bind(thisArg[, arg1[, arg2[, ...]]])
* * 返回值：返回一个原函数的拷贝，并拥有指定的 **`this`** 值和初始参数。
  * 参数：
  * bind函数并没有立即执行，仅仅只是绑定（与apply、call不一样，这两个立即执行，虽然三者都是改变调用对象的this指向）

复制并新建一个函数，并在新建的时候就已经为其指定了 this，指定this为传参

在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。



```js
Function.prototype.myBind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  var _this = this // 要复制的函数，myBind的调用者--> this
  var args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    // 使用 new 运算符去构造绑定函数，会改变 this指向，this指向当前实例； 在构造函数里用new调用，A()中的 this 指向创建的新实例对象 let a = new A()
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}

// 另一种写法  https://juejin.im/post/6856419501777846279
1.bind 的参数可以在绑定和调用的时候分两次传入
2.bindArgs 是绑定时除了第一个参数以外传入的参数，args 是调用时候传入的参数，将二者拼接后一起传入
3.如果使用 new 运算符构造绑定函数，则会改变 this 指向，this 指向当前的实例
4.通过 Fn 链接原型，这样 fBound 就可以通过原型链访问父类 Fn 的属性

Function.prototype.bind = function(context) {    let that = this;    let bindArgs = Array.prototype.slice.call(arguments, 1);    function Fn () {};    function fBound(params) {        let args = Array.prototype.slice.call(arguments) ;        return that.apply(this instanceof fBound ? this : context, bindArgs.concat(args));    }    Fn.prototype = this.prototype;    fBound.prototype = new Fn();    return fBound;}
```

![image-20200804150927399](/Users/bamlin/Library/Application Support/typora-user-images/image-20200804150927399.png)







#### 第二篇的

* **<u>每个函数都有 prototype 属性，除了 Function.prototype.bind()复制新建的函数，该属性指向原型</u>**

* **<u>每个对象都有 -proto-属性，指向了 创建该对象的构造函数的原型</u>**

  * 对象可以通过 -proto- 来寻找不属于该对象的属性，-proto- 将对象连接起来组成了原型链

  * 原型对象及prototype：**<u>构造函数有一个prototype属性，指向实例对象的原型对象</u>**。通过同一个构造函数实例化的多个对象具有相同的原型对象。<u>**经常使用原型对象来实现继承**</u>。

  * ```js
    function Foo(){};
    Foo.prototype.a = 1;
    var f1 = new Foo;
    var f2 = new Foo;
    
    console.log(Foo.prototype.a);//1
    console.log(f1.a);//1
    console.log(f2.a);//1
    ```

  * 任何对象都可以看做是通过 Object() 构造函数的 new 操作实例化的对象，

  Foo.prototype 作为实例对象，构造函数即 Object()，原型对象是 Object.prototype

  ```JS
  console.log(Foo.prototype.__proto__ === Object.prototype);//true
  ```

  * <u>实例对象Foo.prototype本身具有constructor属性，所以它会覆盖继承自原型对象Object.prototype的constructor属性。 ---- constructor：原型对象有一个constructor属性，指向该原型对象对应的构造函数，如果实例对象本身自己没有 constructor属性的话，如 let f1 = new Foo();  f1 可以继承其原型对象 Foo.prototype的constructor属性；；；；</u>

    ​    <u>如果实例对象本身自己有了，那么就会覆盖 其原型对象的 constructor属性</u>

  <u>***非常重要！！！！！！！***</u>

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ghduzzuj14j30n80700va.jpg" alt="image-20200803194308607" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ghdv23oh46j310u0oqqkb.jpg" alt="img" style="zoom:50%;" />

* **<u>函数也是对象，只不过是具有特殊功能的对象而已。任何函数都可以看做是通过Function()构造函数的new操作实例化的结果</u>**

如果把函数Foo当成实例对象的话，其构造函数是Function()，其原型对象是Function.prototype；类似地，函数Object的构造函数也是Function()，其原型对象是Function.prototype。

```js
console.log(Foo.__proto__ === Function.prototype);//true
console.log(Object.__proto__ === Function.prototype);//true
```



* 如果Function.prototype作为实例对象的话，其原型对象是什么呢？和前面一样，所有的对象都可以看成是Object()构造函数的new操作的实例化结果。所以，Function.prototype的原型对象是Object.prototype，其原型函数是Object()。

* ------ 先有 Object.prototype，后有 Function.prototype

  ```js
  console.log(Function.prototype.__proto__ === Object.prototype);//true
  ```

  

总结：

【1】函数(Function也是函数)是new Function的结果，所以函数可以作为实例对象，其构造函数是Function()，原型对象是Function.prototype。

【2】对象(函数也是对象)是new Object的结果，所以对象可以作为实例对象，其构造函数是Object()，原型对象是Object.prototype

【3】Object.prototype的原型对象是null。









###



#### `__proto__`

引用《JavaScript权威指南》的一段描述：

> Every JavaScript object has a second JavaScript object (or null ,
> but this is rare) associated with it. This second object is known as a prototype, and the first object inherits properties from the prototype.

翻译出来就是每个JS对象一定对应一个原型对象，并从原型对象继承属性和方法。好啦，既然有这么一个原型对象，那么对象怎么和它对应的？

**对象`__proto__`属性的值就是它所对应的原型对象：**

```js
var one = {x: 1};
var two = new Object();
one.__proto__ === Object.prototype // true
two.__proto__ === Object.prototype // true
one.toString === one.__proto__.toString // true
```

> 用自己的话 翻译翻译就是：
>
> 1、万物皆对象
>
> 2、变量（对象）的 `__proto__` 属性，它对应的值就是 变量（对象）对应的原型对象，即自己所继承的原型。变量（or变量or实例）的 -proto- 指向继承构造函数的 prototype 属性
>
> eg. var two = new Object(); 
>
> two.__proto__ === Object.prototype // true
>
> 3、只有函数才有 prototype 属性





#### `prototype`

首先来说说`prototype`属性，不像每个对象都有`__proto__`属性来标识自己所继承的原型，**<u>只有函数才有`prototype`属性。</u>**

为什么只有函数才有`prototype`属性？

其实函数在JS中真的很特殊，是所谓的_一等公民_。JS不像其它面向对象的语言，它没有类（`class`，ES6引进了这个关键字，但更多是语法糖）的概念。<u>**JS通过函数来模拟类。**</u>

当你创建函数时，JS会为这个函数自动添加`prototype`属性，~~值是空对象~~ **值是一个有 constructor 属性的对象，不是空对象**。而一旦你把这个函数当作构造函数（`constructor`）调用（即通过`new`关键字调用），那么JS就会帮你创建该构造函数的实例，实例继承构造函数`prototype`的所有属性和方法（实例通过设置自己的`__proto__`指向承构造函数的`prototype`来实现这种继承）。

#### 小结

虽然对不熟悉的人来说还有点绕，但JS正是通过`__proto__`和`prototype`的合作实现了原型链，以及对象的继承。

构造函数，通过`prototype`来存储要共享的属性和方法，也可以设置`prototype`指向现存的对象来继承该对象。

对象的`__proto__`指向自己构造函数的`prototype`。`obj.__proto__.__proto__...`的原型链由此产生，包括我们的操作符`instanceof`正是通过探测`obj.__proto__.__proto__... === Constructor.prototype`来验证`obj`是否是`Constructor`的实例。

回到开头的代码，`two = new Object()`中`Object`是构造函数，所以`two.__proto__`就是`Object.prototype`。至于`one`，ES规范定义对象字面量的原型就是`Object.prototype`。





### 更深一步的探讨 🐔和🥚

我们知道JS是单继承的，`Object.prototype`是原型链的顶端，所有对象从它继承了包括`toString`等等方法和属性。

`Object`本身是构造函数，继承了`Function.prototype`;`Function`也是对象，继承了`Object.prototype`。这里就有一个_鸡和蛋_的问题：

```js
Object instanceof Function // true
Function instanceof Object // true
```

相信经过上面的详细阐述，这张图应该一目了然了。

1. `Function.prototype`和`Function.__proto__`都指向`Function.prototype`，这就是鸡和蛋的问题怎么出现的。
2. `Object.prototype.__proto__ === null`，说明原型链到`Object.prototype`终止。

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghdntd2b1ij30h60lp0v7.jpg)

```js
// 无聊的尝试
// new func().__proto__ --> func.prototype
// func.protptype 就是 func.prototype
// func.__proto__ 为 Function.prototype
function Temp(){};
let temp = new Temp();
console.dir(temp.__proto__) // Temp.prototype --> Object.prototype
console.dir(temp.__proto__.__proto__) // Object.prototype
console.dir(temp.__proto__.__proto__.__proto__) // null
console.dir(temp.prototype)  // undefined

console.debug("=============")
let o1 = new Object()
console.dir(o1.__proto__) // Object.prototype
console.dir(o1.__proto__.__proto__) // null
console.dir(o1.__proto__.__proto__.__proto__) // error
console.dir(o1.prototype) // undefined
```

* 

* **<u>一切皆对象</u>**，方法Function是对象，方法的原型 Function.prototype也是对象
* 原型链的尽头（root）是`Object.prototype`。**所有对象均从`Object.prototype`继承属性。**
* `Function.prototype`和`Function.__proto__`为**同一对象**。
  *  eg. Function.prototype === Number.__proto__ 	// true
  * 这意味着： **`Object`/`Array`/`String`等等构造函数本质上和`Function`一样，均继承于`Function.prototype`**
* `Function.prototype`直接继承root（`Object.prototype`）。
  * 通过这点我们可以弄清 **继承的原型链：`Object.prototype(root)`<---`Function.prototype`<---`Function|Object|Array...`**

* 由以上3点导出我们最后的问题：`Object`和`Function`的鸡和蛋的问题。

必须首先更深入一层去理解`Function.prototype`这个对象，因为它是导致`Function instanceof Object`和`Object instanceof Function`都为`true`的原因。



回归规范，摘录一下

1、`Function.prototype`是个不同于一般函数（对象）的函数（对象）。

- `Function.prototype`像普通函数一样可以调用，但总是返回`undefined`。
- 普通函数实际上是`Function`的实例，即普通函数继承于`Function.prototype`。`func.__proto__ === Function.prototype`。
- `Function.prototype`继承于`Object.prototype`，并且没有`prototype`这个属性。`func.prototype`是普通对象，`Function.prototype.prototype`是`null`。
- 所以，`Function.prototype`其实是个另类的函数，可以独立于/先于`Function`产生。



2、`Object`本身是个（构造）函数，是`Function`的实例，即`Object.__proto__`就是`Function.prototype`。

> 总结：

**最后总结：先有`Object.prototype`（原型链顶端），`Function.prototype`继承`Object.prototype`而产生，最后，`Function`和`Object`和其它构造函数继承`Function.prototype`而产生。**











