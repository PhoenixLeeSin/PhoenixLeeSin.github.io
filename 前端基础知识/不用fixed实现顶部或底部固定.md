### 问题

最近工作中遇到ios手机页面头部`fixed`定位，随着页面的滚动头部偶尔跟着页面滚动，当滚动停止页面头部恢复原来的位置。

### 分析原因

此时页面是`body`产生的滚动条，猜测可能是页面滚动，导致定位失效。解决方案就很明显了，就是定位元素跟滚动容器隔离。怎么隔离呢？没错就是**区域滚动**，不要让`body`产生滚动条。

### 解决方案

- 绝对定位
- 纵向flex布局

#### 绝对定位方案

页面`头部` 、`内容`、`底部`通过绝对定位固定，`内容`区域产生滚动条。 缺点是需要通过计算确定`内容`区域的位置；页面固定头部布局调整时，需要重新计算高度。

#### 纵向flex布局

首先让外容器100%填充屏幕，最外围包裹容器flex布局、方向`flex-direction: column`。

`内容`区域`flex: 1`弹性伸缩。

优点是不用计算头部、底部的高度，计算相对简单些；适应页面布局调整。

html 结构

```
...
<body class="flex">
  <div class="header" id="header">
    <div id="placeholder"></div>
    <h3>弹性盒子固定头部</h3>
  </div>
  <div class="main" id="main">
    ...
  </div>
  <div class="footer">
    底部
  </div>
 ...
复制代码
```

css 代码如下

```
.flex {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 100%;
  display: flex;
  flex-direction: column;
}

.header {
  height: .88rem;
  background-color: crimson;
}

.main {
  flex: 1;
  background-color: #fff;
  overflow-x: hidden;
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch;
}

.footer {
  height: .88rem;
  line-height: .88rem;
  background-color: blue;
  color: #fff;
  text-align: center;
  font-size: .34rem;
}
复制代码
```

### 兼容性测试

- ios设备完美兼容、滚动流畅
- 安卓设备自带浏览正常显示

无奈的是我司安卓设备App  webview打开，滚动出现空白屏、元素被挤压。需进一步调研，是不是安卓webview问题。 小米机型、华为出现上述问题，oppo表现正常。

### 参考文献

[移动web滚动篇](https://link.juejin.cn?target=http%3A%2F%2Fwww.alloyteam.com%2F2017%2F04%2Fsecrets-of-mobile-web-scroll-bars-and-drop-refresh%2F)


作者：liansky
链接：https://juejin.cn/post/6844903588871815181
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。