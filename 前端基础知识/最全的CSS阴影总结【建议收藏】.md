# 基础入门

## 基础实例

为了方便平时可以直接取用，所以写在最前面，下面有详细的属性解析；

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9962894576be47b2aa81087f1a91cd96~tplv-k3u1fbpfcp-watermark.awebp)

```css
/*四周外阴影*/
box-shadow: 0 0 3px 3px #ccc;
/*四周内阴影*/
box-shadow:inset 0px 0px 5px 1px #000; 

/*左*/
box-shadow: -7px 0 5px -5px #333;
/*右*/
box-shadow:  7px 0 5px -5px #333;
/*上*/
box-shadow: 0 -7px  5px -5px #333;
/*下*/
box-shadow:  0 7px 5px -5px #333;

/*右下*/
box-shadow: 3px 3px 1px 1px #666;
/*右上*/
box-shadow: 3px -3px 1px 1px #666;
/*左下*/
box-shadow: -3px 3px 1px 1px #666;
/*左上*/
box-shadow: -3px -3px 1px 1px #666;
复制代码
```

## box-shadow语法：

```
              x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色  |  阴影向内
box-shadow:  offset-x | offset-y | blur-radius | spread-radius | color |  inset;
复制代码
```

**注：**

1）只写两个值, 那么这两个值将会被当作 offset-x和offset-y；

2）写了第三个值, 那么第三个值将会被当作blur-radius；

3）写了第四个值, 那么第四个值将会被当作spread-radius；

4）可选，inset关键字。

5）可选，color值。

```
div {
  width: 150px;
  height: 150px;
  background-color: #fff;

  box-shadow: 120px 80px 40px 20px #0ff;
}
复制代码
```

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/577738b79726441c8c2adf1127885de5~tplv-k3u1fbpfcp-watermark.awebp)

## box-shadow属性分析

### Offset-x

offset-x 属性沿 X 轴向左和向右移动阴影

![giphy.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b1ffee09d3b3448fb6f7b17b19c21b54~tplv-k3u1fbpfcp-watermark.awebp)

示例：

```
 box-shadow: 20px 0px rgba(0,0,0,0.5);

 box-shadow: -20px 0px rgba(0,0,0,0.5);
复制代码
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a90c9c9b7ccd4ec78c49f91130fb9df2~tplv-k3u1fbpfcp-watermark.awebp)

> 阴影的水平偏移量，正值表示阴影将在框的右侧，负偏移量将阴影在框的左侧。

### offset-y

offset-y 属性沿 Y 轴上下移动阴影

![giphy (1).gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b0982e760544c6b8dbf339a8897037c~tplv-k3u1fbpfcp-watermark.awebp) 示例：

```
 box-shadow: 0px 20px rgba(0,0,0,0.5);
 
 box-shadow: 0px -20px rgba(0,0,0,0.5);
复制代码
```

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9fc8a25cd7584dd19491457bb6f19123~tplv-k3u1fbpfcp-watermark.awebp)

> 阴影的垂直偏移，负值表示框阴影将在框上方，正值表示阴影将在框下方。

### blur-radius模糊半径

blur-radius 属性会模糊按钮周围的颜色

![giphy (2).gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fcef77a4985f4ee8b517cef2b5322e21~tplv-k3u1fbpfcp-watermark.awebp) 示例：

```
box-shadow: 0 0 50px rgba(0,0,0,0.8);
复制代码
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1759331e3f0a400aa0c9f874a7268684~tplv-k3u1fbpfcp-watermark.awebp)

> 模糊半径（可选），如果设置为 0 阴影会更清晰，数字越大，它就越模糊。 不能为负值。

### spread-radius

这个值在我们的按钮周围散布我们的阴影

![giphy (3).gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aedfe1d8fffb48a8a65057e9828fc46b~tplv-k3u1fbpfcp-watermark.awebp)

示例：

```
  box-shadow: 0 0 0 50px rgba(0,0,0,0.5);
复制代码
```

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fbf24f6153694b1d86d9aaf0798c425f~tplv-k3u1fbpfcp-watermark.awebp)

> 传播半径（可选），正值增加阴影的大小，负值减小大小。默认值为 0（阴影与模糊大小相同）。

## text-shadow文本阴影

文字的阴影用text-shadow属性实现，它的用法和box-shadow非常相似。

唯一的区别是text-shadow没有spread值也没有inset关键字。

它仅适用于文本节点。

**语法**：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/97517af0f21f4d86afa71d7bf5d18692~tplv-k3u1fbpfcp-watermark.awebp)

**示例**：

```
 /*测试一*/
 text-shadow:2px 3px 6px #000;
 
 /*测试二*/
 text-shadow:2px 3px 1px #000;
复制代码
```

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e95d4103caa442e0aaf8142e4d3dc3e3~tplv-k3u1fbpfcp-watermark.awebp)

# 晋阶用法

## 多重阴影

一个元素可以使用多个阴影，多个阴影之间用逗号分隔；

**示例一**：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7000a4cf50734c6fb43f356fea146e24~tplv-k3u1fbpfcp-watermark.awebp)

```
 box-shadow: 5px 5px 10px #FF00FF, -7px -4px 4px #FF9966;
复制代码
```

**示例二**：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/541b47de09f5494e9e4ccb7b68a723dc~tplv-k3u1fbpfcp-watermark.awebp)

```
box-shadow:-10px 0 10px red, 10px 0 10px blue,0 -10px 10px yellow,0 10px 10px green;
复制代码
```

> 注：
> 1）当给同一个元素使用多个阴影属性时，需要注意它的顺序，*最先写*的阴影将显示*在最顶层*;
> 2)、如果前面的阴影模糊值小于后面的阴影模糊值，那么前面的显示在后面之上，如果前面阴影的模糊值大于后面的阴影模糊值，那么前面的阴影将遮住后面的阴影效果。

## drop-shadow阴影

**语法**:

```
 drop-shadow(offset-x offset-y blur-radius spread-radius color)
复制代码
```

和 box-shadow 属性类似。但是 box-shadow 属性在元素的整个框后面创建一个矩形阴影, 而 drop-shadow() 过滤器则是创建一个**符合图像本身形状**(alpha通道)的阴影。

**示例一**：

注意三角形的位置，使用`box-shadow`，三角形的位置是没有阴影的；但是使用` drop-shadow`,则有； ![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5c390652100341e6999e9dedaf6c3341~tplv-k3u1fbpfcp-watermark.awebp)

```
.bubble-box {
    width: 150px;
    margin: 40px; padding: 50px;
    background-color: #66CCFF;
    position: relative;
    font-size: 24px;
}
.triangle {
    position: absolute;
    left: -40px;
    width: 0; height: 0;
    overflow: hidden;
    border: 20px solid transparent;
    border-right-color: #66CCFF;
}
.box-shadow {
    box-shadow: 5px 5px 10px black;
}
.drop-shadow {
    filter: drop-shadow(5px 5px 10px black);
}
******************************
<div class="bubble-box box-shadow">
    <i class="triangle"></i>
    box-shadow
</div>
<div class="bubble-box drop-shadow">
    <i class="triangle"></i>
    filter: drop-shadow
</div>
复制代码
```

**示例二**：

假如有一张 T 恤的图片，如果想要给这件衣服加一些阴影，不同阴影的效果如下；

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b3f40e192f6345ee922fea1eefe4453c~tplv-k3u1fbpfcp-watermark.awebp)

可以看到如果使用`box-shadow`，则阴影在周围的盒子上；

而使用`drop-shadow`，则阴影是在 T 恤周围；

------

> 引用参考:
>  [Learn the CSS Box-Shadow Property by Coding a Beautiful Button ](https://link.juejin.cn?target=https%3A%2F%2Fwww.freecodecamp.org%2Fnews%2Fcss-box-shadow-property-with-examples%2F)
> [MDN Web Docs box-shadow](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fbox-shadow)
> [Creating Glow Effects with CSS](https://link.juejin.cn?target=https%3A%2F%2Fcodersblock.com%2Fblog%2Fcreating-glow-effects-with-css%2F)

小可爱看完就点个赞再走吧！🤞🤞🤞


作者：Axjy
链接：https://juejin.cn/post/6982595708796796965
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。