

## 字符串相关操作

#### 数字转字符串 

1. 浮点数转换成整型，这个更容易出错，很多人喜欢使用parseint()，其实parseint()是用于将字符串转换成数字，而不是浮点数和整型之间的转换，我们应该使用math.floor()或者math.round()。另外，和第二节的对象查找中的问题不一样，math是内部对象，所以math.floor()其实并没有多少查询方法和调用的时间，速度是最快的。 

2. 把数字转换成字符串，应用"" + 1，虽然看起来比较丑一点，但事实上这个效率是最高的，性能上来说：("" + ) > string() > .tostring() > new string()，尽量使用编译时就能使用的内部操作要比运行时使用的用户操作要快。string()属于内部函数，所以速度很快，而.tostring()要查询原型中的函数，所以速度逊色一些，new string()用于返回一个精确的副本。 

```js 

```

("" + ) > string() > .tostring() > new string() 

``` 

#### json转字符串 -- JSON.parse(a)

#### 字符串转json -- JSON.stringify( a ) 

​```js 

```

/** 定制 订阅信息，不同方法不同订阅 */ 

const tempSendContent = {} 

tempSendContent['id'] = this.uuid() 

tempSendContent['reqType'] = 'sub' // 订阅为 sub ，不订阅为 unsub 

tempSendContent['api'] = 'api/market/v1/control/multiDepth' // 订阅深度 

tempSendContent['quote'] = this.depthBase // 他这里搞混了，quote是计价币，应该是 usdt，但这里要写 btc 

tempSendContent['base'] = this.depthQuote 

tempSendContent['exchangeNames'] = ['HayekEx', 'KuCoin'] // 后面更改 

tempSendContent['accountIds'] = [95] 

tempSendContent['frequency'] = 1000 

const sendContent = JSON.stringify(tempSendContent) 

``` 

#### 字符串转 数字number 

​```js 

parseFloat 

parseFloat("80") 

```

#### 保留几位小数 (四舍五入 & 非四舍五入） 

##### 四舍五入 toFixed(3) 

```js 

var num =2.446242342; 

num = num.toFixed(2); // 输出结果为 2.45 

```

```

##### 不四舍五入 

1. 先把小数变成 整数 

​```js 

```

Math.floor(15.7784514000 * 100) / 100 // 输出结果为 15.77 

``` 

2. 当作字符串，使用正则匹配： 

​```js 

Number(15.7784514000.toString().match(/^d+(?:.d{0,2})?/)) 

// 输出结果为 15.77,不能用于整数如 10 必须写为10.0000 

// 注意：如果是负数，请先转换为正数再计算，最后转回负数 

```

#### 去空格 -- replace() + 正则表达式 

* 去掉所有空格 

```js 

```

var str=" 1223 332 "; 

console.log(str.length) //控制台输出长度为26 

var str_new = str.replace(/s/ig,'') // / s / ig , '' 

console.log(str_new.length) //控制台输出长度为7 

``` 

原理： 

**replace(/s/ig,'')** 

1. s 是匹配任何空白字符，包括空格、制表符、换页符，总之所有空白 

2. / / 包起来是正则表达式的一种语法格式 

3. ig 是 “ignore” && “global” 的合并缩写，表示 “忽略大小写，全文查找”，这里的全文对应的当然是 str 

4. repalce（xxx1，xxx2）是原生的js函数，表示 用xxx2替换掉xxx1 ， 

* 去掉 首尾空格 

​```js 

.replace(/(^s*)|(s*$)/g,'') 

```

* * *

#### js获取map对象的key、value 

```js 

```

var mapObject = { 

id1001: 'xiaoming', 

id1002: 'xiaohong' 

} 

// 适合不知道 key 和value的情况，直接遍历 

for (var key in mapObject){ 

var value = mapObject[key] 

console.log(value) 

} 

// 注意，这里的 key 可以是其他命名 ，另外好像必须是 var来定义，用let会爆红，原因不理解～～～～～ 

``` 

```

* * *

---

#### vue中 轮询器 （不完整） 

```vue 

<template> 

<div > 

{{log}} 

</div> 

</template> 

<script> 

export default { 

name: "TrainingInRotation", 

data(){ 

return { 

log:0, 

timerId:1, // 模拟计时器id，唯一性 

timerObj :{}, // 计时器存储器 

} 

}, 

created(){ 

this.startTraining(); 

}, 

methods: { 

/** 开始轮训*/ 

startTraining() { 

let this_ = this; 

const id = this.timerId++ 

this.timerObj[id] = true 

async function timerFn() { 

if (!this_.timerObj[id]) return 

await this_.getData(); 

setTimeout(timerFn, 1 * 1000) 

} 

timerFn(); 

}, 

/* 

* 停止轮训 

* */ 

stopTime() { 

this.timerObj = {} 

}, 

/**要轮训的代码*/ 

getData(){ 

this.log+=1; 

console.log("this.log:"+this.log); 

} 

}, 

// destroyed不在 method中 

destroyed(){ 

this.stopTime(); 

} 

} 

</script> 

<style scoped> 

</style> 

```

```

关于 [async function的解释](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 

#### js 判断字符串中是否存在 某字符串 

##### indexOf（） （good～～～～） 

​```js 

var str = '123' 

console.log(str.indexOf('3')!= -1 // true 

/* 

indexOf() 方法可以 返回某个指定的字符串值 在字符串中<u>**首次出现**</u>的位置。 

！！！～ 注意，是首次，这个特性可以用在 去重算法中 ～！！！！ 

如果 检索的字符串值 没有出现 ，则返回 -1 

*/ 

```

```

##### search（） 

​```js 

var str = '123' 

consolo.log(str.search('3') != -1 // true 

/* 

search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。 

如果没有找到任何匹配的子串，则返回 -1。 

*/ 

```

#### substring() -- 从字符串中提取固定位置的字符 

```js 

stringObject.substring(start,stop) 

// start 必需。一个非负的整数，规定要提取的子串的第一个字符在 stringObject 中的位置。 

// stop 可选。一个非负的整数，比要提取的子串的最后一个字符在 stringObject 中的位置多 1。 -- 如果省略该参数，那么返回的子串会一直到字符串的结尾。 

```

#### 将时间戳转换为 日期格式 

```js 

var date = new Date(时间戳); //获取一个时间对象 

/** 

1. 下面是获取时间日期的方法，需要什么样的格式自己拼接起来就好了 

2. 更多好用的方法可以在这查到 -> http://www.w3school.com.cn/jsref/jsref_obj_date.asp 

*/ 

date.getFullYear(); // 获取完整的年份(4位,1970) 

date.getMonth(); // 获取月份(0-11,0代表1月,用的时候记得加上1) 

date.getDate(); // 获取日(1-31) 

date.getDay(); // 获取 周几 ，monday。。 

date.getTime(); // 获取时间(从1970.1.1开始的毫秒数) 

date.getHours(); // 获取小时数(0-23) 

date.getMinutes(); // 获取分钟数(0-59) 

date.getSeconds(); // 获取秒数(0-59) 

```

例子： 

```js 

// 比如需要这样的格式 yyyy-MM-dd hh:mm:ss 

var date = new Date(1398250549490); 

Y = date.getFullYear() + '-'; 

M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-'; 

D = date.getDate() + ' '; 

h = date.getHours() + ':'; 

m = date.getMinutes() + ':'; 

s = date.getSeconds(); 

console.log(Y+M+D+h+m+s); //呀麻碟 

// 输出结果：2014-04-23 18:55:49 

```

#### 日期格式 转换为 时间戳 

```js 

// 也很简单 

var strtime = '2014-04-23 18:55:49:123'; 

var date = new Date(strtime); //传入一个时间格式，如果不传入就是获取现在的时间了，这样做不兼容火狐。 

// 可以这样做 

var date = new Date(strtime.replace(/-/g, '/')); 

// 有三种方式获取，在后面会讲到三种方式的区别 

time1 = date.getTime(); 

time2 = date.valueOf(); 

time3 = Date.parse(date); 

/* 

三种获取的区别： 

第一、第二种：会精确到毫秒 

第三种：只能精确到秒，毫秒将用0来代替 

比如上面代码输出的结果(一眼就能看出区别)： 

1398250549123 

1398250549123 

1398250549000 

*/ 

```

* Date()参数形式有7种 

```js 

new Date("month dd,yyyy hh:mm:ss"); 

new Date("month dd,yyyy"); 

new Date("yyyy/MM/dd hh:mm:ss"); 

new Date("yyyy/MM/dd"); 

new Date(yyyy,mth,dd,hh,mm,ss); 

new Date(yyyy,mth,dd); 

new Date(ms); 

比如： 

new Date("September 16,2016 14:15:05"); 

new Date("September 16,2016"); 

new Date("2016/09/16 14:15:05"); 

new Date("2016/09/16"); 

new Date(2016,8,16,14,15,5); // 月份从0～11 

new Date(2016,8,16); 

new Date(1474006780); 

```

#### js获取光标的位置 -- 没看懂？？？？ 

[https://blog.csdn.net/mafan121/article/details/78519348](https://blog.csdn.net/mafan121/article/details/78519348) 

mouseenter -- mdn 

[https://developer.mozilla.org/zh-CN/docs/Web/API/Element/mouseenter_event](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/mouseenter_event) 

现代JavaScript教程 -- 推荐～～～～ 

[https://zh.javascript.info/mousemove-mouseover-mouseout-mouseenter-mouseleave](https://zh.javascript.info/mousemove-mouseover-mouseout-mouseenter-mouseleave) 

***



## 数组



#### shift -- 删除数组第一个元素，并返回该元素的值。次方法改变数组的长度

```javascript
const array1 = [1, 2, 3];
const firstElement = array1.shift();
console.log(array1);
// expected output: Array [2, 3]
console.log(firstElement);
// expected output: 1
```



#### 反转数组

reverse() 

#### 数组中取最大值 

[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/max](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/max) 

```js 

// math。max 

let a = [1,2,3,4,1] 

Math.max(1,2,3,4,1) 

Math.max.apply(null,a) 

function getMaxOfArray(numArray) { 

return Math.max.apply(null, numArray); 

} 

// sort 顺序从小到大，max为最后一个，js中没有python中的-1，所以直接 .length 取最后一位 

a.sort().[a.length] 

// 或者是 算法的样子 

// -- 先假设数组的第一个就是 最大的，然后依次比较第二个数、第三个。。。 

let a = [1,2,3,4,1] 

let max = a[0] 

for (let i = 0+1; i < a.length; i++) { 

if ( max >= a[i]) { 

// continue ??? 

} else { 

max = a[i] 

} 

// 简单的写法是 

// max > a[i] ? max : a[i] 

// 三目运算符，假如为 否的情况，需要不赋值or nan，直接 冒号后面为 空 就行， 

} 

```

#### .filter( function() ) -- 创建一个新数组，用函数来过滤原数组的所有元素 

[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter#%E5%8F%82%E6%95%B0](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter#%E5%8F%82%E6%95%B0) 

```js 

var words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present']; 

const result = words.filter(word => word.length > 6); 

console.log(result); 

// expected output: Array ["exuberant", "destruction", "present"] 

// 语法～～～ 

var newArray = arr.filter(callback(element[, index[, array]])[, thisArg]) 

// callback 

用来测试数组的每个元素的函数。返回 true 表示该元素通过测试，保留该元素，false 则不保留。它接受以下三个参数 

// element 

数组中当前正在处理的元素。 

// index可选 

正在处理的元素在数组中的索引。 

// array可选 

调用了 filter 的数组本身。 

// thisArg可选 

执行 callback 时，用于 this 的值。 

// filter会返回一个新数组 

// 关于循环遍历的问题 

filter 为数组中的每个元素调用一次 callback 函数，并利用所有使得 callback 返回 true 或等价于 true 的值的元素创建一个新数组。callback 只会在已经赋值的索引上被调用，对于那些已经被删除或者从未被赋值的索引不会被调用。那些没有通过 callback 测试的元素会被跳过，不会被包含在新数组中。 

filter 遍历的元素范围在第一次调用 callback 之前就已经确定了。在调用 filter 之后被添加到数组中的元素不会被 filter 遍历到。如果已经存在的元素被改变了，则他们传入 callback 的值是 filter 遍历到它们那一刻的值。被删除或从来未被赋值的元素不会被遍历到。 

```

```js 

// 数组去重的用法 实例 

function unique(arr) { 

var obj = {}; 

return arr.filter(function(item, index, arr){ 

// 参照上面的语法： item是当前正在处理的元素，index当前元素的索引 

return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true) 

}) 

// hasOwnProperty() 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。 

} 

var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}]; 

console.log(unique(arr)) 

//[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}] //所有的都去重了 

```

## 延时函数 

### 调度：setTimeout 和 setInterval 

#### setTimeout( function{}, 4000) -- 将 ***<u>函数</u>*** 的执行推迟到一段时间之后再执行。 

##### 可以用 clearTimeout()来阻止 函数的执行 

```JS 

setTimeout(alert('dasd'),2000) // 无效，立即执行 

setTimeout(function(){alert('dasd')},2000) // 有效，等待时间后执行 

// setTimeout() 方法用于在指定的毫秒数后调用函数或计算表达式，注意，是 函数和表达式的执行 ！！！ 

```

```js 

// 调用 含参数的复杂延时函数 

function haha(a) { 

console.log(a) 

} 

setTimeout( haha, 1000,'qweqweqwe') // c参数放最后 

```

#### setInterval -- 让函数间隔一定时间周期性执行。 

## JavaScript 浮点数精度问题 

* 原因 

这里需要结合具体例子来讲解，比如：十进制的0.65转化为二进制表示为：0.1010011……（跟精度限制有关）。 

十进制小数用二进制通常是用乘二取整法来获得的。 

比如0.65换算成二进制就是：0.65 × 2 = 1.3 取1，留下0.3继续乘二取整；0.3 × 2 = 0.6 取0， 留下0.6继续乘二取整；0.6 × 2 = 1.2 取1，留下0.2继续乘二取整；…… 

一直循环，直到达到精度限制才停止（所以，计算机保存的小数一般会有误差，所以在编程中，要想比较两个小数是否相等，只能比较某个精度范围内是否相等）。 

* 重写 tofix（） 

https://juejin.im/post/5aa1395c6fb9a028df223516 