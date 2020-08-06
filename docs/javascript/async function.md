## async 异步function



**async function** 用来定义一个返回 [`AsyncFunction`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/AsyncFunction) 对象的异步函数。异步函数是指通过事件循环异步执行的函数，它会通过一个隐式的 [`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 返回其结果。如果你在代码中使用了异步函数，就会发现它的语法和结构会更像是标准的同步函数。

返回的`Promise`对象会运行执行(resolve)异步函数的返回结果，或者运行拒绝(reject)——如果异步函数抛出异常的话。

```js
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('resolved');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = await resolveAfter2Seconds();
  console.log(result);
  // expected output: 'resolved'
}

asyncCall(); // 先 calling ，后 resolved
```





`await` 操作符用于等待一个[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 对象。它只能在异步函数 [`async function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 中使用。

返回值：返回 Promise 对象的处理结果。如果等待的不是 Promise 对象，则返回该值本身。

描述

await 表达式会暂停当前 [`async function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 的执行，等待 Promise 处理完成。若 Promise 正常处理(fulfilled)，其回调的resolve函数参数作为 await 表达式的值，继续执行 [`async function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function)。

若 Promise 处理异常(rejected)，await 表达式会把 Promise 的异常原因抛出。

另外，如果 await 操作符后的表达式的值不是一个 Promise，则返回该值本身。













```js
var resolveAfter2Seconds = function() {
  console.log("starting slow promise");
  return new Promise(resolve => {
    setTimeout(function() {
      resolve("slow");
      console.log("slow promise is done");
    }, 2000);
  });
};

var resolveAfter1Second = function() {
  console.log("starting fast promise");
  return new Promise(resolve => {
    setTimeout(function() {
      resolve("fast");
      console.log("fast promise is done");
    }, 1000);
  });
};

var sequentialStart = async function() {
  console.log('==SEQUENTIAL START==');

  // 1. Execution gets here almost instantly
  const slow = await resolveAfter2Seconds();
  console.log(slow); // 2. this runs 2 seconds after 1.

  const fast = await resolveAfter1Second();
  console.log(fast); // 3. this runs 3 seconds after 1.
}

//	在 concurrentStart中，两个计时器被同时创建，然后执行await。这两个计时器同时运行，这意味着程序完成运行只需要2秒，而不是3秒,即最慢的计时器的时间。
var concurrentStart = async function() {
  console.log('==CONCURRENT START with await==');
  const slow = resolveAfter2Seconds(); // starts timer immediately
  const fast = resolveAfter1Second(); // starts timer immediately

  // 1. Execution gets here almost instantly
  console.log(await slow); // 2. this runs 2 seconds after 1.
  console.log(await fast); // 3. this runs 2 seconds after 1., immediately after 2., since fast is already resolved
}

var concurrentPromise = function() {
  console.log('==CONCURRENT START with Promise.all==');
  return Promise.all([resolveAfter2Seconds(), resolveAfter1Second()]).then((messages) => {
    console.log(messages[0]); // slow
    console.log(messages[1]); // fast
  });
}

var parallel = async function() {
  console.log('==PARALLEL with await Promise.all==');
  
  // Start 2 "jobs" in parallel and wait for both of them to complete
  await Promise.all([
      (async()=>console.log(await resolveAfter2Seconds()))(),
      (async()=>console.log(await resolveAfter1Second()))()
  ]);
}

// This function does not handle errors. See warning below!
var parallelPromise = function() {
  console.log('==PARALLEL with Promise.then==');
  resolveAfter2Seconds().then((message)=>console.log(message));
  resolveAfter1Second().then((message)=>console.log(message));
}

sequentialStart(); // after 2 seconds, logs "slow", then after 1 more second, "fast"

// wait above to finish
setTimeout(concurrentStart, 4000); // after 2 seconds, logs "slow" and then "fast"

// wait again
setTimeout(concurrentPromise, 7000); // same as concurrentStart

// wait again
setTimeout(parallel, 10000); // truly parallel: after 1 second, logs "fast", then after 1 more second, "slow"

// wait again
setTimeout(parallelPromise, 13000); // same as parallel
```



#### `wait` and parallelism(并行)

在`sequentialStart`中，程序在第一个`await`停留了2秒，然后又在第二个`await`停留了1秒。直到第一个计时器结束后，第二个计时器才被创建。程序需要3秒执行完毕。


在 `concurrentStart`中，两个计时器被同时创建，然后执行`await`。这两个计时器同时运行，这意味着程序完成运行只需要2秒，而不是3秒,即最慢的计时器的时间。

但是 `await `仍旧是顺序执行的，第二个 `await` 还是得等待第一个执行完。在这个例子中，这使得先运行结束的输出出现在最慢的输出之后。

**<u>如果你希望并行执行两个或更多的任务，你必须像在`parallel`中一样使用`await Promise.all([job1(), job2()])`。</u>**



#### `async`/`await和`Promise#then对比以及错误处理

大多数异步函数也可以使用Promises编写。但是，在错误处理方面，`async`函数更容易捕获异常错误

上面例子中的`concurrentStart`函数和`concurrentPromise`函数在功能上都是等效的。在`concurrentStart`函数中，如果任一`await`ed调用失败，它将自动捕获异常，异步函数执行中断，并通过隐式返回Promise将错误传递给调用者。

在Promise例子中这种情况同样会发生，该函数必须负责返回一个捕获函数完成的`Promise`。在`concurrentPromise`函数中，这意味着它从`Promise.all([]).then()`返回一个Promise。事实上，在此示例的先前版本忘记了这样做！

但是，`async`函数仍有可能然可能错误地忽略错误。
以`parallel`异步函数为例。 如果它没有等待`await`（或返回）`Promise.all([])`调用的结果，则不会传播任何错误。
虽然`parallelPromise`函数示例看起来很简单，但它根本不会处理错误！ 这样做需要一个类似于`return ``Promise.all([])`处理方式。







### `使用async`函数重写 promise 链



返回 [`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)的 API 将会产生一个 promise 链，它将函数肢解成许多部分。例如下面的代码：

```js
function getProcessedData(url) {
  return downloadData(url) // 返回一个 promise 对象
    .catch(e => {
      return downloadFallbackData(url)  // 返回一个 promise 对象
    })
    .then(v => {
      return processDataInWorker(v); // 返回一个 promise 对象
    });
}
```

可以重写为单个`async`函数：

```js
async function getProcessedData(url) {
  let v;
  try {
    v = await downloadData(url); 
  } catch (e) {
    v = await downloadFallbackData(url);
  }
  return processDataInWorker(v);
}
```

注意，在上述示例中，`return` 语句中没有 `await` 操作符，因为 `async function` 的返回值将被隐式地传递给 `Promise.resolve`。



**`return await promiseValue;` 与 `return promiseValue;的比较`**

返回值`隐式的传递给`[`Promise.resolve`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)，并不意味着`return await promiseValue;和return promiseValue;`在功能上相同。

看下下面重写的上面代码，在`processDataInWorker`抛出异常时返回了null：

```
async function getProcessedData(url) {
  let v;
  try {
    v = await downloadData(url);
  } catch(e) {
    v = await downloadFallbackData(url);
  }
  try {
    return await processDataInWorker(v); // 注意 `return await` 和单独 `return` 的比较
  } catch (e) {
    return null;
  }
}
```

简单地写上`return processDataInworker(v);将导致在processDataInWorker(v)`出错时function返回值为[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)`而不是`返回null。`return foo;`和`return await foo;`，有一些细微的差异:`return foo;`不管`foo`是promise还是rejects都将会直接返回`foo。相反地，`如果`foo`是一个[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)，`return await foo;`将等待`foo`执行(resolve)或拒绝(reject)，如果是拒绝，将会在返回前抛出异常。















