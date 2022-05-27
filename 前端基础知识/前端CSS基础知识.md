## 伪类

> CSS **伪类** 是添加到选择器的关键字，指定要选择的元素的特殊状态。例如，[`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover) 可被用于在用户将鼠标悬停在按钮上时改变按钮的颜色。



## ：root

> **`:root`** 这个 CSS [伪类](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)匹配文档树的根元素。对于 HTML 来说，**`:root`** 表示 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/html) 元素，除了[优先级](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity)更高之外，与 `html` 选择器相同。



## 用法

### 在声明全局 [CSS 变量](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)时 **`:root`** 会很有用：

```css
:root {
  --main-color: hotpink;
  --pane-padding: 5px 42px;
}
```



## 通用选择器（Universal selector）

#### 通用选择（*）是最终的王牌。它允许选择在一个页面中的所有元素。由于给每个元素应用同样的规则几乎没有什么实际价值，更常见的做法是与其他选择器结合使用。

> 重要提示：使用通用选择时小心。因为它适用于所有的元素，在大型网页利用它可以对性能有明显的影响：网页可以显示比预期要慢。不会有太多的情况下，您想使用此选择。

```css
* {
  box-sizing: border-box;
}
```



## box-sizing

#### **`box-sizing`** 属性定义了 [user agent](https://developer.mozilla.org/zh-CN/docs/Glossary/User_agent)  （用户代理是代表一个人的计算机程序）应该如何计算一个元素的总宽度和总高度。

#### 在 [CSS 盒子模型](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)的默认定义里，你对一个元素所设置的 [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 与 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height) 只会应用到这个元素的内容区。如果这个元素有任何的 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 或 [`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding) ，绘制到屏幕上时的盒子宽度和高度会加上设置的边框和内边距值。这意味着当你调整一个元素的宽度和高度时需要时刻注意到这个元素的边框和内边距。当我们实现响应式布局时，这个特点尤其烦人。

##### box-sizing 属性可以被用来调整这些表现:

- ##### `content-box` 是默认值。如果你设置一个元素的宽为100px，那么这个元素的内容区会有100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。

- ##### `border-box` 告诉浏览器：你想要设置的边框和内边距的值是包含在width内的。也就是说，如果你将一个元素的width设为100px，那么这100px会包含它的border和padding，内容区的实际宽度是width减去(border + padding)的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。

```css
border-box`不包含`margin
```



## position(https://www.ruanyifeng.com/blog/2019/11/css-position.html)

#### `position`属性用来指定一个元素在网页上的位置，一共有5种定位方式，即`position`属性主要有五个值。

- #### `static`

> ##### `static`是`position`属性的默认值



- #### `relative`

> ##### `relative`表示，相对于默认位置（即`static`时的位置）进行偏移，即定位基点是元素的默认位置。它必须搭配`top`、`bottom`、`left`、`right`这四个属性一起使用，用来指定偏移的方向和距离。

![](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/zMBaDT.jpg)



- #### `fixed`

> ###### `fixed`表示，相对于视口（viewport，浏览器窗口）进行偏移，即定位基点是浏览器窗口。这会导致元素的位置不随页面滚动而变化，好像固定在网页上一样。
>
> 

- #### `absolute`

> ###### `absolute`表示，相对于上级元素（一般是父元素）进行偏移，即定位基点是父元素。
>
> ###### 它有一个重要的限制条件：定位基点（一般是父元素）不能是`static`定位，否则定位基点就会变成整个网页的根元素`html`。另外，`absolute`定位也必须搭配`top`、`bottom`、`left`、`right`这四个属性一起使用。
>
> ###### `absolute`定位的元素会被"正常页面流"忽略，即在"正常页面流"中，该元素所占空间为零，周边元素不受影响。



- #### `sticky`

> ###### `sticky`跟前面四个属性值都不一样，它会产生动态效果，很像`relative`和`fixed`的结合：一些时候是`relative`定位（定位基点是自身默认位置），另一些时候自动变成`fixed`定位（定位基点是视口）。
>
> ###### `sticky`生效的前提是，必须搭配`top`、`bottom`、`left`、`right`这四个属性一起使用，不能省略，否则等同于`relative`定位，不产生"动态固定"的效果。原因是这四个属性用来定义"偏移距离"，浏览器把它当作`sticky`的生效门槛。



# padding

#### **`padding`** [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) [简写属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Shorthand_properties)控制元素所有四条边的[内边距区域](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model#padding-area)。不同个数的值代表的含义：

```css
/* 应用于所有边 */
padding: 1em;

/* 上边下边 | 左边右边 */
padding: 5% 10%;

/* 上边 | 左边右边 | 下边 */
padding: 1em 2em 2em;

/* 上边 | 右边 | 下边 | 左边 */
padding: 5px 1em 0 2em;

/* 全局值 */
padding: inherit;
padding: initial;
padding: unset;
```



## [设置居中的方案](https://juejin.cn/post/6844903560879013901)

```css
///设置垂直居中例子
.wrapper {

  position: absolute;

  top: 50%;

  left: 0;

  right: 0;

  transform: translateY(-50%);

}
```



# display

#### **`display`** 属性可以设置元素的内部和外部显示类型 *display types*。部分取值

1. #### [`display: none`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display#display_none)  将 `display` 设置为 `none` 会将元素从 [可访问性树 *accessibility tree*](https://developer.mozilla.org/zh-CN/docs/Learn/Accessibility/What_is_accessibility#accessibility_apis) 中移除。这会导致该元素及其所有子代元素不再被屏幕阅读技术 *screen reading technology* 访问

2. #### [`display: block`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display-outside)  这个值会生成一个块级元素盒子，同时在该元素之前和之后打断（换行）。简单来说就是，这个值会将该元素变成[块级元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Block-level_elements)



# @keyframes

> #### 关键帧 **`@keyframes`** [at-rule](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule) 规则通过在动画序列中定义关键帧（或waypoints）的样式来控制CSS动画序列中的中间步骤。和 [转换 transition](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transitions) 相比，关键帧 keyframes 可以控制动画序列的中间步骤。要使用关键帧, 先创建一个带名称的 `@keyframes` 规则，以便后续使用 [`animation-name`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-name) 属性将动画同其关键帧声明匹配。



# 防止页面加载图片时发生“抖动”

> #### 有些图片宽度确定，高度自适应，当图片未加载完成时，高度为 0，加载完成后高度撑开，在这个过程中会引起页面闪烁抖动



### 解决方法：

1. #### 默认情况下`padding`的百分比数值以父级元素的宽度作为参考

2. #### 获取图片的高宽比，如 100*20 的图片就是 20%

3. #### 将父元素宽度设置成图片最终宽度，高度不设置，利用`padding-top`填充高度，如`padding-top: 20%`



### 代码

> #### 加入希望一个图片宽度为 100%，高度自适应，图片像素为 1920x576

```html
<div class="img-box">
  <img src="xxx.png" />
</div>
```

```css
.img-box {
  position: relative;
  top: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
  padding-top: 30%; /* 主要是这行，数值为图片的高宽比 如576/1920=0.3 */
}
.img-box img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```



# img标签

### `<img />` 是行内元素还是块级元素？

- `<img />` 标签没有独占一行，所以是行内元素，这没啥问题

### 既然是行内元素为什么能够设置宽高呢？

#### <img /> 标签属于替换元素  

- 浏览器根据元素的标签和属性，来决定元素的具体显示内容
- 例如浏览器会根据 `<img>` 标签的src属性的值来读取图片信息并显示出来，而如果查看(X)HTML代码，则看不到图片的实际内容；又例如根据 `<input>` 标签的type属性来决定是显示输入框，还是单选按钮等
- (X)HTML中的 `<img>、<input>、<textarea>、<select>、<object>` 都是替换元素。这些元素往往没有实际的内容，即是一个空元素
- 如：`<img src="tigger.jpg"/>`、`<input type="submit" name="Submit" value="提交"/>`
- **可替换元素的性质同设置了display:inline-block的元素一致**



> #### 需要设置<img  />长宽 margin padding等一些css属性时 需要设置  display: block; 不然不会生效



# outline

> #### [CSS](https://developer.mozilla.org/en-US/docs/CSS) 的 `outline` 属性是在一条声明中设置多个轮廓属性的[简写属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Shorthand_properties) ， 例如 [`outline-style`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-style), [`outline-width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-width) 和 [`outline-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-color)。 

### [border 和 outline](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline#border_和_outline)

> #### [border](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 和 outline 很类似，但有如下区别：
>
> - ##### outline不占据空间，绘制于元素内容周围。
>
> - ##### 根据规范，outline通常是矩形，但也可以是非矩形的。

> #### **注意：**对于很多元素来说，如果没有设置样式，轮廓是不可见的。因为样式（style）的默认值是 `none`。但 `input` 元素是例外，其样式默认值由浏览器决定。



# overflow-y

当一个块级元素（div元素、p元素之类的）的内容在垂直方向发生溢出时，

[CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS)属性`overflow-y `决定如何处理溢出的内容。

隐藏溢出内容（hidden），或者显示滚动条（scroll），或者直接显示溢出内容（visible），或者让浏览器来处理（auto）。



# [block，inline和inline-block概念和区别 ](https://www.cnblogs.com/KeithWang/p/3139517.html)

## 总体概念

1. block和inline这两个概念是简略的说法，完整确切的说应该是 block-level elements (块级元素) 和 inline elements (内联元素)。block元素通常被现实为独立的一块，会单独换一行；inline元素则前后不会产生换行，一系列inline元素都在一行内显示，直到该行排满。
2. 大体来说HTML元素各有其自身的布局级别（block元素还是inline元素）：
   - 常见的块级元素有 DIV, FORM, TABLE, P, PRE, H1~H6, DL, OL, UL 等。
   - 常见的内联元素有 SPAN, A, STRONG, EM, LABEL, INPUT, SELECT, TEXTAREA, IMG, BR 等。
3. block元素可以包含block元素和inline元素；但inline元素只能包含inline元素。要注意的是这个是个大概的说法，每个特定的元素能包含的元素也是特定的，所以具体到个别元素上，这条规律是不适用的。比如 P 元素，只能包含inline元素，而不能包含block元素。
4. 一般来说，可以通过display:inline和display:block的设置，改变元素的布局级别。

## block，inline和inlinke-block细节对比

- display:block

1. 1. block元素会独占一行，多个block元素会各自新起一行。默认情况下，block元素宽度自动填满其父元素宽度。
   2. block元素可以设置width,height属性。块级元素即使设置了宽度,仍然是独占一行。
   3. block元素可以设置margin和padding属性。

- display:inline

1. 1. inline元素不会独占一行，多个相邻的行内元素会排列在同一行里，直到一行排列不下，才会新换一行，其宽度随元素的内容而变化。
   2. inline元素设置width,height属性无效。
   3. inline元素的margin和padding属性，水平方向的padding-left, padding-right, margin-left, margin-right都产生边距效果；但竖直方向的padding-top, padding-bottom, margin-top, margin-bottom不会产生边距效果。

- display:inline-block

1. 1. 简单来说就是将对象呈现为inline对象，但是对象的内容作为block对象呈现。之后的内联对象会被排列在同一行内。比如我们可以给一个link（a元素）inline-block属性值，使其既具有block的宽度高度特性又具有inline的同行特性。

## 补充说明

- 一般我们会用display:block，display:inline或者display:inline-block来调整元素的布局级别，其实display的参数远远不止这三种，仅仅是比较常用而已。
- IE（低版本IE）本来是不支持inline-block的，所以在IE中对内联元素使用display:inline-block，理论上IE是不识别的，但使用display:inline-block在IE下会触发layout，从而使内联元素拥有了display:inline-block属性的表象。





## 实现Flex布局下横向滚动

![如图](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/cHsFjs.png)

### 要实现此布局下图片可以横向滚动可以采用下列css代码 关键属性 flex-shrink: 0;

```scss
关键属性: flex-shrink: 0; 
使用flex布局可以轻松的让元素水平排列, 但是flex默认是在父元素内压缩子元素以达到能够显示所有元素的目的.此时如果想要做水平滚动,就得设置子元素不被压缩.于是flex-shrink设置为0即可.

       &__images {

​        line-height: 0.4rem;

​        margin-right: 0.16rem;

​        overflow-x: auto;

​        overflow-y: hidden;

​        display: flex;

​        .list__item__products__image {

​          width: 0.4rem;

​          height: 0.4rem;

​          margin-right: 0.12rem;

​          flex-shrink: 0;

​        }

​      }
```



## 设置图片水平居中

### 首先明确img标签是行内元素(inline)，不是块元素(block)

```
    .icon-image {

​      display: block

​      margin: auto

​      width: 1.1rem

​      height: 1.1rem

​    }
```



## calc()

### calc() 此 [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 函数允许在声明 CSS 属性值时执行一些计算。

```
/* property: calc(expression) */
width: calc(100% - 80px);
```



在使用width 平分布局下 如果存在边框纳闷就需要calc()计算 如下图

![](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/hHbwmz.png)



```stylus
  .city-list {

​    overflow: hidden

​    width: 100%

​    .city-item {

​      width: calc((100% - 4px) / 4)

​      line-height: 0.9rem

​      float: left

​      text-align: center

​      border-right: 1px solid $lineColor

​      border-bottom: 1px solid $lineColor

​    }

  }
```



## flex布局子元素宽度超出父元素问题

[🔗](https://juejin.cn/post/6974356682574921765)



#### 最近在做项目中，使用`flex`布局遇到了个老问题：当`flex子元素`里的子元素的宽度过大，超出`flex父元素`时，设置`flex:1`并不能限制`flex子元素`的尺寸

html

```html
<div class="wrap">
    <div class="left"></div>
    <div class="right">
        <div class="right-content">
            adasdasdasdadasdasdasdasdasadasdasdasdadasdasadasdasdasdadasdasdasdasdasadasdasdasdadasdas
        </div>
    </div>
</div>
复制代码
```

css

```css
.wrap {
    width: 300px;
    height: 100px;
    margin: 30px 0;
    display: flex;
    border: 1px solid red;
    .left {
        flex: 0 0 100px;
        background: lightgray;
    }
    .right {
        flex: 1;
        background: lightblue;
        &-content {
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
            background: lightyellow;
        }
    }
}
复制代码
```

可以看到，右侧的文本超出部分没有隐藏，而且超出了`.wrap`的宽度。之前也遇到过这种情况，没太注意就给忘了，看来有必要总结一下；

## 问题分析

- `.right`的宽度该如何计算？

正常情况下的元素宽度，如果设置有具体的值，那就是设置的值；如果没有设置，那就是该元素内容区占据的宽度。上面的例子可以看到，`.right`并没有设置`width`属性，所以`.right`是由`.right-content`撑开。又由于是`flex`盒子子元素，设置了`flex: 1`属性，所以:

```
.right宽度 = .right内容占据宽度(即.right-content宽度) + flex: 1属性分配的宽度
复制代码
```

- `.right`盒子已经设置`flex: 1`；为什么宽度还会超出父元素？

这是使用`flex`布局很常见的一个误区：给子元素设置了`flex`属性，很自然的就认为，它会按比例分配父元素的宽度。因为大多时候恰好是这样，其实并非如此。我们再来好好理解一下`flex：1`的含义(`flex`是`flex-grow`、`flex-shrink`和`flex-basis`的缩写，这里就不做介绍。[详情点这里](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fflex)）

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cff817b045784d139d0bca6527ea424c~tplv-k3u1fbpfcp-watermark.awebp) 可以看出，`flex：1`并不是决定子元素宽度的因素，它只是规定了，如果父元素有多余空间，以怎样的比例去分配剩余空间，并不会对子元素原本就占据的空间做处理。所以，当元素原本的宽度就超过父元素宽度时，子元素内容就会超出。

## 解决方案

1. 限制子元素原本宽度，`.right`设置`width`属性

修改`.right`元素`css`如下，

```css
.wrap {
    ...
    .right {
        width: 0; //新增
        flex: 1;
        background: lightblue;
        &-content {
            ...
        }
    }
}
复制代码
```

原理：强行设置`.right`原本宽度为0，让`.right`盒模型宽度完全由`flex: 1`这个属性来分配。

chrome浏览器效果完美：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3bb233322188449fa0d28e8f91b50de0~tplv-k3u1fbpfcp-watermark.awebp)

但是在firefox浏览器时，即使设置`width: 0`，也不会生效，子元素还是超出；同事提出可以设置`min-width: 0`，试了一下，果然可以。虽然还没搞明白设置`min-width: 0`的原理是什么，但是真香。

1. `.right`设置`overflow`属性不为`visible`

设置`width: 0`可行的前提是:`.right-content`元素宽度继承父元素`.right`。如果当`.right-content`元素设置了自己的宽度时，方法1就不能满足了，如下所示：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6eb0e0ab0496475bbc9405254d62a0a9~tplv-k3u1fbpfcp-watermark.awebp)

设置`.right-content`元素`css`如下，子元素依然会超出

```css
.wrap {
    ...
    .right {
        width: 0; //新增
        flex: 1;
        background: lightblue;
        &-content {
            width: 300px; //新增
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
            background: lightyellow;
        }
    }
}
复制代码
```

这时候就回到了基本的css问题，子元素内容超出如何展示，给`.right`设置`overflow`搞定

正常`css`如下，让

```css
.wrap {
    ...
    .right {
        // width: 0;
        flex: 1;
        background: lightblue;
        overflow: auto; //新增
        &-content {
            width: 300px; //新增
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
            background: lightyellow;
        }
    }
}
复制代码
```

## 总结

1. 设置`min-width：0`可以解决当`flex子元素`的子元素大小为`auto`的情况；
2. 设置`overflow`不为`visible`可以解决所有情况下的麻烦；


作者：JackTian
链接：https://juejin.cn/post/6974356682574921765
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

