## position 属性

position: fixed;



https://developer.mozilla.org/zh-CN/docs/Web/CSS/position

## MDN讲解

### 定位类型

- **定位元素（positioned element）**是其[计算后](https://developer.mozilla.org/zh-CN/docs/Web/CSS/computed_value)位置属性为 `relative`, `absolute`, `fixed `或 `sticky` 的一个元素。
- **相对定位元素（****relatively positioned element****）**是[计算后](https://developer.mozilla.org/zh-CN/docs/Web/CSS/computed_value)位置属性为 `relative `的元素。
- **绝对定位元素（absolutely positioned element）**是[计算后](https://developer.mozilla.org/zh-CN/docs/Web/CSS/computed_value)位置属性为 `absolute` 或 `fixed` 的元素。
- **粘性定位元素****（****stickily positioned element****）**是[计算后](https://developer.mozilla.org/zh-CN/docs/Web/CSS/computed_value)位置属性为 `sticky` 的元素。





### 取值

- `static`

  该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 `top`, `right`, `bottom`, `left` 和 `z-index `属性无效。

- `relative`

  该关键字下，元素先放置在未添加定位时的位置，再**<u>在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）</u>**。position:relative 对 table-*-group, table-row, table-column, table-cell, table-caption 元素无效。

- `absolute`

  不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。绝对定位的元素可以设置外边距（margins），且不会与其他边距合并。

- `fixed`

  不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。打印时，元素会出现在的每页的固定位置。`fixed` 属性会创建新的层叠上下文。当元素祖先的 `transform`, `perspective` 或 `filter` 属性非 `none` 时，容器由视口改为该祖先。

- `sticky` 

  盒位置根据正常流计算(这称为正常流动中的位置)，然后相对于该元素在流中的 flow root（BFC）和 containing block（最近的块级祖先元素）定位。在所有情况下（即便被定位元素为 `table 时`），该元素定位均不对后续元素造成影响。当元素 B 被粘性定位时，后续元素的位置仍按照 B 未定位时的位置来确定。`position: sticky `对 `table` 元素的效果与 `position: relative `相同。



* 相对定位 relative

**<u>相对定位的元素是在文档中的正常位置偏移给定的值，但是不影响其他元素的偏移</u>**。下面的例子中，注意未应用定位的其它元素是按照 "Two" 在正常位置的情况下进行布局的。

```html
<div class="box" id="one">One</div>
<div class="box" id="two">Two</div>
<div class="box" id="three">Three</div>
<div class="box" id="four">Four</div>
.box {
  display: inline-block;
  width: 100px;
  height: 100px;
  background: red;
  color: white;
}

#two {
  position: relative;
  top: 20px;
  left: 20px;
  background: blue;
}
```

![image-20191211181511405](https://tva1.sinaimg.cn/large/006tNbRwly1g9sy7s3sgyj312o0eewev.jpg)





* 绝对定位 - absolute

**<u>相对定位的元素并未脱离文档流，而绝对定位的元素则脱离了文档流</u>**。在布置文档流中其它元素时，绝对定位元素不占据空间。绝对定位元素相对于*最近的非 `static` 祖先元素*定位。当这样的祖先元素不存在时，则相对于ICB（inital container block, 初始包含块）











***



***



### static --是默认值



### relative

`relative` 表现的和 `static` 一样，除非你添加了一些额外的属性。

在一个相对定位（position属性的值为relative）的元素上设置 `top` 、 `right` 、 `bottom` 和 `left` 属性会使其偏离其正常位置。其他的元素的位置则不会受该元素的影响发生位置改变来弥补它偏离后剩下的空隙。

```css
.relative1 {
  position: relative;
}
.relative2 {
  position: relative;
  top: -20px;
  left: 20px;
  background-color: white;
  width: 500px;
}
```

![image-20191211172000394](/Users/bamlin/Library/Application Support/typora-user-images/image-20191211172000394.png)



### fixed -- 固定定位，相对于 视窗，常用于 页脚

一个固定定位（position属性的值为fixed）元素会相对于视窗来定位，这意味着即便页面滚动，它还是会停留在相同的位置。和 `relative` 一样， `top` 、 `right` 、 `bottom` 和 `left` 属性都可用。

我相信你已经注意到页面右下角的固定定位元素。你现在可以仔细看看它，这里有它所使用的CSS：

```css
.fixed {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 200px;
  background-color: white;
}
```

一个固定定位元素不会保留它原本在页面应有的空隙（脱离文档流）。

令人惊讶地是移动浏览器对 fixed 的支持很差。[这里有相应的解决方案](http://bradfrostweb.com/blog/mobile/fixed-position/). // 方案好像没啥用



### absolute -- 绝对定位

`absolute` 是最棘手的position值。 `absolute` 与 `fixed` 的表现类似，但是它不是相对于视窗而是相对于*最近的“positioned”祖先元素*。**<u>如果绝对定位（position属性的值为absolute）的元素没有“positioned”祖先元素，那么它是相对于文档的 body 元素，并且它会随着页面滚动而移动</u>**。记住一个“positioned”元素是指 position 值不是 `static` 的元素。 父元素必须不是 

这里有一个简单的例子：

```css
.relative {
  position: relative;
  width: 600px;
  height: 400px;
}
.absolute {
  position: absolute;
  top: 120px;
  right: 0;
  width: 300px;
  height: 200px;
}
```

