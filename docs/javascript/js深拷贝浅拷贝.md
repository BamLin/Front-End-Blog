

***

## 深拷贝 浅拷贝



#### 堆和栈的区别

* 其实 深拷贝 和 浅拷贝的主要区别就是其在 内存中的 存车类型不同。 

* 堆和栈 都是内存中划分出来用来存储的区域 

* 栈（stack）为自动分配的内存空间，它由系统自动释放； -- 在js中是 存放 基本数据类型（undefined、null、布尔值、number、string） 和 

引用类型的变量（引用类型（object）是存放在堆内存中的，变量实际上是一个存放在栈内存的指针，这个指针指向堆内存中的地址） 

* 而堆（heap）则是动态分配的内存，大小不定也不会自动释放。 

* 

#### 基本数据类型 值不可变，引用类型值可变

* 基本数据类型的值是不可变的，动态修改了基本数据类型的值，它的原始值也是不会改变的，例如： 

```javascript
var str = "abc"; 
console.log(str[1]="f"); // f 
console.log(str); // abc 
```

基本数据类型主要是：undefined，boolean，number，string，null。 

javascript中的原始值（undefined、null、布尔值、数字和字符串）与对象（包括数组和函数）有着根本区别。原始值是不可更改的：任何方法都无法更改（或“突变”）一个原始值。 

***<u>字符串中所有的方法看上去返回了一个修改后的字符串，实际上返回的是一个新的字符串值。</u>*** 

* so， 基本类型的比较是 值的比较，只要值相等认为他们是相等的 

这一点其实开始我是比较迷惑的，总是感觉 js 是一个灵活的语言，任何值应该都是可变的，真是图样图森破，我们通常情况下都是对一个变量重新赋值，而不是改变基本数据类型的值。就如上述引用所说的那样，在 js 中没有方法是可以改变布尔值和数字的。倒是有很多操作字符串的方法，但是这些方法都是返回一个新的字符串，并没有改变其原有的数据。 

作者：sunshine小小倩 

链接：https://juejin.im/post/59ac1c4ef265da248e75892b 

来源：掘金 

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。 

#### 基本数据类型存放在栈中 

* 存放在栈内存中的简单数据段，数据大小确定，内存空间大小可以分配，是直接按值存放的，所以可以直接访问。 

\#### 引用类型 存放在 堆中 

引用类型（object）是存放在堆内存中的，变量实际上是一个存放在栈内存的指针，这个指针指向堆内存中的地址。每个空间大小不一样，要根据情况开进行特定的分配， 

* 引用类型值可变， （跟前面的 基本数据类型不一致 ） 

```javascript
var a = [1,2,3]; 
a[1] = 5; 
console.log(a[1]); // 5 
```



![](https://tva1.sinaimg.cn/large/006y8mN6gy1g95s8efo7wj31380tutf9.jpg)

 

* **<u>引用类型的比较是 引用的比较</u>** 

每次我们对 js中的 引用类型进行操作的时候，都是操作其 对象的引用（保存在 栈内存中的指针），<u>**所以比较两个引用类型，是看其的引用是否指向 同一个对象。**</u> 



![](https://tva1.sinaimg.cn/large/006y8mN6gy1g95us629ddj30u00vhdmv.jpg) 



```javascript
const a = [1,2] 
const b = [1,2] 
a === b // false a、b在内存中的位置不一样，也就是 a、b指向的不是同一个对象，所以不相等 
const c = a 
a === c // true 
```





***



## 小总结

<u>**栈（stack）为自动分配的内存空间，它由系统自动释放；而堆（heap）则是动态分配的内存，大小不定也不会自动释放。**</u>

****

<u>**栈 -- 基本数据类型（ + 引用类型的指针 即变量） -- 变不了 -- 系统回收 ---- 基本类型的比较： 看值是否相等，不能看地址，因为每个值都是新开辟地址**</u> 

<u>**堆 -- 引用数据类型（的值，其指针 即变量在 栈中） -- 自由变 -- 不自动回收 -- 引用类型的比较：看是否指向同一个对象，即堆中的地址**</u> 

引用类型（object）是存放在堆内存中的，变量实际上是一个存放在栈内存的指针，这个指针指向堆内存中的地址。每个空间大小不一样，要根据情况开进行特定的分配 



## 传值 与 传址

了解了基本数据类型与引用类型的区别之后，我们就应该能明白传值与传址的区别了。 

* **<u>进行赋值操作的时候，基本数据类型的赋值（=）是在内存中新开辟一段栈内存，然后再把再将值赋值到新的栈中</u>**

  所以说，基本类型的赋值的两个变量是两个独立相互不影响的变量。 

* **<u>但是引用类型的赋值是传址。只是改变指针的指向</u>** 

例如，<u>**也就是说引用类型的赋值是对象保存在栈中的地址的赋值，这样的话两个变量就指向同一个对象，*因此两者之间操作互相有影响***</u> 



## 赋值（=） 和 浅拷贝 和 深拷贝 的区别



```javascript
    var obj1 = {
        'name' : 'zhangsan',
        'age' :  '18',
        'language' : [1,[2,3],[4,5]],
    };
    var obj2 = obj1;
    var obj3 = shallowCopy(obj1);
    function shallowCopy(src) {
        var dst = {};
        for (var prop in src) {
          // hasOwnProperty() 方法会返回一个布尔值，
          // 指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。
            if (src.hasOwnProperty(prop)) {
                dst[prop] = src[prop];
            }
        }
        return dst;
    }
// obj1：原始数据
// obj2：赋值操作得到
// obj3：浅拷贝得到
    obj2.name = "lisi";
    obj3.age = "20";
    obj2.language[1] = ["二","三"];
    obj3.language[2] = ["四","五"];
    console.log(obj1);  
    //obj1 = {
    //    'name' : 'lisi',
    //    'age' :  '18',
    //    'language' : [1,["二","三"],["四","五"]],
    //};
    console.log(obj2);
    //obj2 = {
    //    'name' : 'lisi',
    //    'age' :  '18',
    //    'language' : [1,["二","三"],["四","五"]],
    //};
    console.log(obj3);
    //obj3 = {
    //    'name' : 'zhangsan',
    //    'age' :  '20',
    //    'language' : [1,["二","三"],["四","五"]],
    //};

// 
```



<u>然而，我们接下来来看一下改变引用类型会是什么情况呢，我又改变了赋值得到的对象 `obj2` 和浅拷贝得到的 `obj3` 中的 `language` 属性的第二个值和第三个值（`language` 是一个数组，也就是引用类型）。结果见输出，可以看出来，无论是修改赋值得到的对象 `obj2` 和浅拷贝得到的 `obj3` 都会改变原始数据。</u>

**这是因为浅拷贝只复制一层对象的属性，并不包括对象里面的为引用类型的数据。所以就会出现改变浅拷贝得到的 `obj3` 中的引用类型时，会使原始数据得到改变。**

**<u>深拷贝：将 B 对象拷贝到 A 对象中，包括 B 里面的子对象，</u>**

**<u>浅拷贝：将 B 对象拷贝到 A 对象中，但不包括 B 里面的子对象</u>**



| --     | 和原数据是否指向同一对象 | 第一层数据为基本数据类型 | 原数据中包含子对象     |
| :----- | ------------------------ | ------------------------ | ---------------------- |
| 赋值   | 是                       | 改变会使原数据一同改变   | 改变会使原数据一同改变 |
| 浅拷贝 | 否                       | 不会                     | 会                     |
| 深拷贝 | 否                       | 不会                     | 不会                   |





### 深拷贝

<u>**深拷贝是对对象以及对象的所有子对象进行拷贝。**</u>

[掘金上饿了么的小姐姐写的文章，很好，看不懂，继续多看几次](https://juejin.im/post/59ac1c4ef265da248e75892b)

> **Zepto**是一个轻量级的**针对现代高级浏览器的JavaScript库，** 它与jquery**有着类似的api**。 如果你会用jquery，那么你也会用zepto。
>
> https://www.html.cn/doc/zeptojs_api/

***如何进行深拷贝？***

**<u>思路就是递归调用刚刚的浅拷贝，把所有属于对象的属性类型都遍历赋给另一个对象即可</u>**。我们直接来看一下 Zepto 中深拷贝的代码：

```javascript
    // 内部方法：用户合并一个或多个对象到第一个对象
    // 参数：
    // target 目标对象  对象都合并到target里
    // source 合并对象
    // deep 是否执行深度合并
    function extend(target, source, deep) {
        for (key in source)
            if (deep && (isPlainObject(source[key]) || isArray(source[key]))) {
                // source[key] 是对象，而 target[key] 不是对象， 则 target[key] = {} 初始化一下，否则递归会出错的
                if (isPlainObject(source[key]) && !isPlainObject(target[key]))
                    target[key] = {}

                // source[key] 是数组，而 target[key] 不是数组，则 target[key] = [] 初始化一下，否则递归会出错的
                if (isArray(source[key]) && !isArray(target[key]))
                    target[key] = []
                // 执行递归
                extend(target[key], source[key], deep)
            }
            // 不满足以上条件，说明 source[key] 是一般的值类型，直接赋值给 target 就是了
            else if (source[key] !== undefined) target[key] = source[key]
    }

    // Copy all but undefined properties from one or more
    // objects to the `target` object.
    $.extend = function(target){
        var deep, args = slice.call(arguments, 1);

        //第一个参数为boolean值时，表示是否深度合并
        if (typeof target == 'boolean') {
            deep = target;
            //target取第二个参数
            target = args.shift()
        }
        // 遍历后面的参数，都合并到target上
        args.forEach(function(arg){ extend(target, arg, deep) })
        return target
    }
```

在 Zepto 中的 `$.extend` 方法判断的第一个参数传入的是一个布尔值，判断是否进行深拷贝。

在 `$.extend` 方法内部，只有一个形参 target，这个设计你真的很巧妙。
因为形参只有一个，所以 target 就是传入的第一个参数的值，并在函数内部设置一个变量 args 来接收去除第一个参数的其余参数，如果该值是一个布尔类型的值的话，说明要启用深拷贝，就将 deep 设置为 true，并将 target 赋值为 args 的第一个值（也就是真正的 target）。如果该值不是一个布尔类型的话，那么传入的第一个值仍为 target 不需要进行处理，只需要遍历使用 extend 方法就可以。

这里有点绕，但是真的设计的很精妙，建议自己打断点试一下，会有意外收获（玩转 js 的大神请忽略）。

而在 extend 的内部，是拷贝的过程。



### 深拷贝这段我没看懂


作者：sunshine小小倩链接：https://juejin.im/post/59ac1c4ef265da248e75892b来源：掘金著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。