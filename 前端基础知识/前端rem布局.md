# 前端rem布局

## rem是什么？

> #### rem（font size of the root element）是指相对于根元素的字体大小的单位。简单的说它就是一个相对单位。看到rem大家一定会想起em单位，em（font size of the element）是指相对于父元素的字体大小的单位。它们之间其实很相似，只不过一个计算的规则是依赖根元素一个是依赖父元素计算。





```javascript
function remSize() {

​    var deviceWidth = document.documentElement.clientWidth || window.innerWidth;

​    if (deviceWidth >= 750) {

​        deviceWidth = 750

​    }

​    if (deviceWidth <= 320) {

​        deviceWidth = 320

​    }

  //// 设置html 根元素的font-size 为设备宽除以7.5的px 就是1rem; 
  ///  如果你把html的font-size设为20px，前面说过，rem就是html的字体大小，那么1rem = 20px
​    document.documentElement.style.fontSize = (deviceWidth / 7.5) + 'px'

​    document.querySelector('body').style.fontSize = 0.3 + 'rem'

}



remSize();



window.onresize = function() {

​    remSize();

}
```



## 也可以在工程的public文件夹下的index.html下添加如下代码实现不同设备的适配




```html
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<link rel="icon" href="<%= BASE_URL %>favicon.ico">
<title><%= htmlWebpackPlugin.options.title %></title>
  ///适配
<script>
  var width = document.documentElement.clientWidth || document.body.clientWidth;
  var ratio = width / 375;
  var fontSize = ratio * 100;
  document.documentElement.style.fontSize = fontSize + 'px';
</script>
```



### 100px使我们在base.scss下添加的html的fontsize

```scss
html {

  font-size: 100px;

}



body {

  font-size: .12rem;

}

## 
```

