#Flexible Box 弹性布局



## 阮一峰 flex教程



http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html -- 语法篇

http://www.ruanyifeng.com/blog/2015/07/flex-examples.html -- 实例篇



### 一、Flex布局 -- what

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

**<u>注意，设为 Flex 布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效。</u>**

* 任何一个容器指定为 flex布局
* 行内元素也可以 flex 布局

```css
.box{
  display: inline-flex;
}
```

* webkit内核的浏览器，必须加上 -webkit 前缀

```css
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
```





### 二、基本概念

采用 flex布局的元素，称为 flex容器，其所有子元素 自动成为 容器成员



* 容器默认存在两根轴：水平的主轴（main axis）和 垂直的交叉轴（cross axis）
* 主轴的开始位置（与边框的交叉点）-- main start，结束位置--main end
* 交叉轴 开始位置 -- cross start，结束位置 -- cross end

* 项目默认沿着 主轴排列。单个项目占据的主轴空间叫做 main size，占据的交叉轴空间叫做 cross size



### 三、容器的属性

- flex-direction -- 决定 主轴的方向（即项目的排列方向）
- flex-wrap -- 默认情况下，项目都排列在 轴线上，如果一条轴线排不下，要 如何？换行
- flex-flow -- 是上面两个属性的简写形式，默认值为 row nowrap
- justify-content -- 定义项目在主轴上的对齐方式！！！（最常用）
- align-items -- 定义项目在交叉轴上如何对齐（应该会有用处，感觉很炫～～）
- align-content -- 定义了多个轴线的对齐方式，若只有一根轴线，该属性不起作用



todo～～～ 例子～～～～



### 四、项目的属性

- order -- 定义项目的排列顺序。数值越小，排列越靠前，默认为0
- flex-grow -- 定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
- flex-shrink -- 定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
- flex-basis -- 在分配了多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间，默认值是auto，即项目的本来大小。
- flex -- 是 flex-grow、flex-shrink和flex-basis的简写，默认值为 0 1 auto，后两个属性可选。 建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。
- align-self -- 允许单个项目有与 其他项目不一样的对齐方式，可覆盖 align-items属性。默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，等同于 stretch。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```





## MDN flex

https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-content

https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout

https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex





## 常见布局



* #### 圣杯布局

http://www.ruanyifeng.com/blog/2015/07/flex-examples.html









## element布局



















































