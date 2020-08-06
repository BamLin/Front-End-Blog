### JS 的执行队列

##### 支付宝前端 入门者手册

[https://www.yuque.com/fe9/basic/mvx83k](https://www.yuque.com/fe9/basic/mvx83k)

轮转时间片： 时间片轮转调度是一种最古老，最简单，最公平且使用最广的算法，又称RR调度。每个进程被分配一个时间段，称作它的时间片，即该进程允许运行的时间。

假设要执行两个任务

任务 A 和任务 B。

js引擎会把任务 A 和任务 B 切成以毫秒或者微秒为单位的片段。

把这些片段按照随机的方式进行排列（这个也叫争抢时间片）。

在把这些排列好的片段一个一个往JS引擎里面去送。

在JS引擎以一个一个片段为基准单位去执行这个片段，把任务 A 和任务 B 按照片段是执行完。



***



### 一、什么是事件队列？

```javascript
console.log('start');
setTimeout(()=>{
	console.log('A');
},1000);
console.log('end');
//start
//end
//A
------
console.log('start');
setTimeout(()=>{
	console.log('A');
},0); // 1000变为0
console.log('end');
//start
//end
//A
```



复制代码可以看出，我们将settimeout第二个参数设置为0后，'A' 也总是会在 'end' 之后输出。所以究竟发生了什么？

这是因为 <u>**setTimeout 的回调函数只是会被添加至事件队列，而不是立即执行**</u>。由于当前的任务没有执行结束，所以 setTimeout 任务不会执行，<u>**直到输出了 'end' 之后，当前任务执行完毕，执行栈为空，这时事件队列才会把 setTimeout 回调函数压入执行栈执行**</u>。

### 二、Promise的含义和基本用法？

所谓<u>**Promise，简单说就是一个容器**</u>，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

从<u>**语法**</u>上说，<u>**Promise 是一个对象，从它可以获取异步操作的消息**</u>。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

写一个小demo看一下Promise的运行机制：

```js
let promise = new Promise(function(resolve, reject) {
	console.log('Promise');
	resolve();
});
promise.then(function() {
	console.log('resolved.');
});
console.log('Hi!');
// Promise
// Hi!
// resolved
```

上面代码中，Promise 新建后立即执行，所以首先输出的是Promise。然后，then方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以resolved最后输出。



### 三、Macrotasks宏任务（慢） 和 Microtasks微任务（快）



**<u>主线程执行完成该任务后,又会检查microtasks队列并完成里面的所有任务后,再执行macrotask（setTimeout等）的任务。</u>**

<u>Macrotasks和Microtasks 都属于上述的异步任务中的一种，他们分别有如下API：</u>

* <u>**macrotasks宏任务**</u>: setTimeout, setInterval, setImmediate, I/O, UI rendering

* <u>**microtasks微任务**</u>: process.nextTick, Promise, MutationObserver **<u>--- 优先进入 执行栈</u>**

```js
console.log(1);
setTimeout(function(){
	console.log(2);
}, 0);
Promise.resolve().then(function(){
	console.log(3);
}).then(function(){
	console.log(4);
});
//1
//3
//4
//2
```

Promise的函数代码的异步任务会优先于setTimeout的延时为0的任务先执行。

### 四、分析本题目

```javascript
setTimeout(()=>{
    console.log('A');
},0);
var obj={
    func:function () {
        setTimeout(function () {
            console.log('B')
        },0);
        return new Promise(function (resolve) {
            console.log('C');
            resolve();
        })
    }
};
obj.func().then(function () {
    // then -》 promise 是一个容器，语法上是一个对象，可以获得异步操作的消息，是被加入到 microtasks 中的，比macrotask快 
    console.log('D')
});
console.log('E');
```



1、首先 setTimeout A 被加入到事件队列中 ==> 此时macrotasks中有[‘A’]；

2、obj.func()执行时，setTimeout B 被加入到事件队列中 ==> 此时macrotasks中有[‘A’，‘B’]；

3、接着return一个Promise对象，Promise 新建后立即执行 执行console.log('C'); 控制台首次打印‘C’;

4、然后，then方法指定的回调函数，被加入到microtasks队列，将在当前脚本所有同步任务执行完才会执行。 ==> 此时microtasks中有[‘D’]；

5、然后继续执行当前脚本的同步任务，故控制台第二次输出‘E’；

6、此时所有同步任务执行完毕，如上所述先检查microtasks队列，完成其中所有任务，故控制台第三次输出‘D’；

7、最后再执行macrotask的任务，并且按照入队列的时间顺序，控制台第四次输出‘A’，控制台第五次输出‘B’。



***



与浏览器交互决定了js 只有一个 主线程来处理所有的任务，是一门 **<u>单线程的非阻塞的脚本语言</u>**。 -- 因为要进行 各种dom操作，避免一个添加，另一个删除了。

非阻塞 是 当代码执行一项 异步任务（无法立即返回结果，需要花一定的时间才能返回的任务，如 i/o 事件）<u>（ 回顾：io事件属于 macrotasks 宏任务 ，跟settimeout是一个分类的， then -> promise 是 Microtasks 微任务 ，快于 macrotasks ）</u> 的时候，主线程会 挂起 （pending）这个任务，然后 在 异步任务 返回结果 的时候再根据一定规则去执行相应的回调。

****

单线程在 保证了执行顺序的同时也限制了 js 的效率，因此开发出了 web worker技术，近似于 多线程， 但有 很多限制。（没有改变js单线程的本质）

eg.

* 所有新线程都受 主线程的完全控制，不能独立执行 -- 意味着这些 ”线程“ 实际上属于 主线程的子线程。

* 子线程没有执行io操作的权限，只能分担主线程一些诸如计算等任务。

***

***



### js特点：非阻塞 -- js引擎 event loop（事件循环）

##### 注：虽然nodejs中的也存在与传统浏览器环境下的相似的事件循环。然而两者间却有着诸多不同，故把两者分开，单独解释。

***

### 浏览器环境下 js引擎的事件循环机制

#### 1、执行栈 与 事件队列

当javascript代码执行的时候会将不同的变量存于内存中的不同位置：堆（heap）和栈（stack）中来加以区分。

其中，

**堆里存放着一些对象。**

**栈中则存放着一些基础类型变量以及对象的指针。**

但是我们这里说的执行栈和上面这个栈的意义却有些不同。

从图片可知，一个方法执行会向执行栈中加入这个方法的执行环境，在这个执行环境中还可以调用其他方法，甚至是自己，其结果不过是在执行栈中再添加一个执行环境。

这个过程可以是无限进行下去的，除非发生了栈溢出，即超过了所能使用内存的最大值。

以上的过程说的都是同步代码的执行。那么当一个异步代码（如发送ajax请求数据）执行后会如何呢？前文提过，js的另一大特点是非阻塞，实现这一点的关键在于下面要说的这项机制——事件队列（Task Queue）。

js引擎遇到一个异步事件后并不会一直等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务。当一个异步事件返回结果后，js会将这个事件加入与当前执行栈不同的另一个队列，我们称之为事件队列。被放入事件队列不会立刻执行其回调，而是等待当前执行栈中的所有任务都执行完毕， 主线程处于闲置状态时，主线程会去查找事件队列是否有任务。如果有，那么主线程会从中取出排在第一位的事件，并把这个事件对应的回调放入执行栈中，然后执行其中的同步代码...，如此反复，这样就形成了一个无限的循环。这就是这个过程被称为“事件循环（Event Loop）”的原因。

。。。。。。。

。。。。。。。

### node环境下的事件循环机制

#### 1、与浏览器环境有何不同？

在 node 中，事件循环表现出的状态与浏览器中大致相同。

不同的是 node 中有一套自己的模型。node中事件循环的实现是依靠 libuv引擎。 node 选择 chrome v8 引擎作为js解释器，v8引擎将 js代码 分析后 去调用对应的 node api，而 这些 api 最后由 libuv引擎 驱动，执行对应的任务，并把不同的事件放在不同的队列中等待 主线程 执行。

因此 实际上，node中的事件循环存在于 libuv引擎中。

#### 2、事件循环模型

下面是一个libuv引擎中的事件循环的模型:

┌───────────────────────┐

┌─>│ timers │

│ └──────────┬────────────┘

│ ┌──────────┴────────────┐

│ │ I/O callbacks │

│ └──────────┬────────────┘

│ ┌──────────┴────────────┐

│ │ idle, prepare │

│ └──────────┬────────────┘ ┌───────────────┐

│ ┌──────────┴────────────┐ │ incoming: │

│ │ poll │ <──connections─── │

│ └──────────┬────────────┘ │ data, etc. │

│ ┌──────────┴────────────┐ └───────────────┘

│ │ check │

│ └──────────┬────────────┘

│ ┌──────────┴────────────┐

└──┤ close callbacks │

└───────────────────────┘

注：模型中的每一个方块代表事件循环的一个阶段

这个模型是node官网上的一篇文章中给出的，我下面的解释也都来源于这篇文章。我会在文末把文章地址贴出来，有兴趣的朋友可以亲自与看看原文。

从上面这个模型中，我们可以大致分析出node中的事件循环的顺序：

```
外部输入数据-->轮询阶段(poll)-->检查阶段(check)-->关闭事件回调阶段(close callback)-->定时器检测阶段(timer)-->I/O事件回调阶段(I/O callbacks)-->闲置阶段(idle, prepare)-->轮询阶段...
```

#### 3、事件循环各阶段详解

从上面这个模型中，我们可以大致分析出node中的事件循环的顺序：

外部输入数据-->轮询阶段(poll)-->检查阶段(check)-->关闭事件回调阶段(close callback)-->定时器检测阶段(timer)-->I/O事件回调阶段(I/O callbacks)-->闲置阶段(idle, prepare)-->轮询阶段...

以上各阶段的名称是根据我个人理解的翻译，为了避免错误和歧义，下面解释的时候会用英文来表示这些阶段。

这些阶段大致的功能如下：

timers: 这个阶段执行定时器队列中的回调如 setTimeout() 和 setInterval()。

I/O callbacks: 这个阶段执行几乎所有的回调。但是不包括close事件，定时器和setImmediate()的回调。

idle, prepare: 这个阶段仅在内部使用，可以不必理会。

poll: 等待新的I/O事件，node在一些特殊情况下会阻塞在这里。

check: setImmediate()的回调会在这个阶段执行。

close callbacks: 例如socket.on('close', ...)这种close事件的回调。

下面我们来按照代码第一次进入libuv引擎后的顺序来详细解说这些阶段：

