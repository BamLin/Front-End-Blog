### Object.prototype.constructor

<u>返回创建实例对象的 [`Object`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object) 构造函数的引用。</u>

注意，此属性的值是对函数本身的引用，而不是一个包含函数名称的字符串。对原始类型来说，如`1`，`true`和`"test"`，该值只可读。



所有对象都会从它的原型上继承一个 `constructor` 属性：

```js
var o = {};
o.constructor === Object; // true

var o = new Object;
o.constructor === Object; // true

var a = [];
a.constructor === Array; // true

var a = new Array;
a.constructor === Array // true

var n = new Number(3);
n.constructor === Number; // true
```



### 打印一个对象的构造函数



以下示例创建一个原型，`Tree`，以及该类型的对象，即`theTree`。 然后打印`theTree`对象的`constructor`属性。

```js
function Tree(name) {
   this.name = name;
}

var theTree = new Tree("Redwood");
console.log( "theTree.constructor is " + theTree.constructor );
```

打印输出：

```js
theTree.constructor is function Tree(name) {
    this.name = name;
}
```