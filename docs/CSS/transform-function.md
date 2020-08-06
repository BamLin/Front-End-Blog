# transform 坐标空间 奇技淫巧

* https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform

**<u>*CSS`transform`属性允许你旋转，缩放，倾斜或平移给定元素。这是通过修改CSS视觉格式化模型的坐标空间来实现的。*</u>**





### transform: scale( number ) 修改元素大小

<img src="/Users/bamlin/Library/Application Support/typora-user-images/image-20200716120727519.png" alt="image-20200716120727519" style="zoom: 33%;" />



CSS 函数 `scale()` 用于修改元素的大小。可以通过向量形式定义的缩放值来放大或缩小元素，同时可以在不同的方向设置不同的缩放值。

```css
// sx缩放向量的横坐标， sy 纵坐标
scale(sx)

scale(sx, sy)
```

eg.

```html
<p>foo</p>
<p class="transformed">bar</p>
```

```scss
p { 
  width: 50px;
  height: 50px;
  background-color: teal;
}
.transformed {
  /* 等同于变换: scaleX(2) scaleY(2);*/
  transform: scale(2);
  background-color: blue;
}
```



* 最初看到是在 vue讲 过渡&动画中，三种 scale的状态 组成的 CSS 动画，很秀

https://cn.vuejs.org/v2/guide/transitions.html       **<u>*图中的例子没有完全看明白*</u>**



<img src="/Users/bamlin/Library/Application Support/typora-user-images/image-20200716121431312.png" alt="image-20200716121431312" style="zoom:33%;" />