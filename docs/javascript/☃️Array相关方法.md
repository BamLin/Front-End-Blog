# Array

### 有个疑问，方法后面加个 prototype，是为何？为什么有的函数没有，有的有？？？



### forEach

```js
const routes = [
  {
    meta:{
      roles: 1
    }
  },
    {
    meta:{
      roles: 2
    }
  },
    {
    meta:{
      roles: 3
    }
  },
    {
    meta:{
      roles: 4
    }
  },
]
routes.forEach(
  route => {
    console.debug(route);
    const temp = {...route}
    console.debug(temp)
  }
)
```







### Array.prototype.filter()

* https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

`**filter()**` 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 

```js
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]

```



* 可以用于 输入框的精准查询/模糊查询

```js
//   el-autocomplete

// 输入查询，后面如果要添加个性化推荐（例如：搜索框。。），可以考虑动态请求后端返回，目前暂时先在前端做
  querySearch(queryString: any, cb: any) {
    const restaurants = this.symbolList;
    const results = queryString
      ? restaurants.filter(restaurants => {
          return (
            restaurants.symbol
              .toLowerCase()
              .indexOf(queryString.toLowerCase()) >= 0
          ); // 找不到 -1，可精确查询，可模糊
        })
      : restaurants;
    console.debug(results);
    // 调用 callback 返回
    cb(results);
  }
```













