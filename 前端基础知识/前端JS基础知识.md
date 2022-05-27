# Element.classList

#### `**Element.classList**` 是一个只读属性，返回一个元素的类属性的实时 [`DOMTokenList`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMTokenList) 集合。虽然 `element.classList` 本身是只读的，但是你可以使用 `add()` 和 `remove()` 方法修改它。

```javascript
const div = document.createElement('div');
div.className = 'foo';

// 初始状态：<div class="foo"></div>
console.log(div.outerHTML);

// 使用 classList API 移除、添加类值
div.classList.remove("foo");
div.classList.add("anotherclass");

// <div class="anotherclass"></div>
console.log(div.outerHTML);

// 如果 visible 类值已存在，则移除它，否则添加它
div.classList.toggle("visible");

// add/remove visible, depending on test conditional, i less than 10
div.classList.toggle("visible", i < 10 );

console.log(div.classList.contains("foo"));

// 添加或移除多个类值
div.classList.add("foo", "bar", "baz");
div.classList.remove("foo", "bar", "baz");

// 使用展开语法添加或移除多个类值
const cls = ["foo", "bar"];
div.classList.add(...cls);
div.classList.remove(...cls);

// 将类值 "foo" 替换成 "bar"
div.classList.replace("foo", "bar");
```



# EventTarget.addEventListener()

> **EventTarget.addEventListener()** 方法将指定的监听器注册到 [`EventTarget`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget) 上，当该对象触发指定的事件时，指定的回调函数就会被执行。 事件目标可以是一个文档上的元素 [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element),[`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)和[`Window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)或者任何其他支持事件的对象 (比如 `XMLHttpRequest`)`。`
>
> #### `addEventListener()`的工作原理是将实现[`EventListener`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventListener)的函数或对象添加到调用它的[`EventTarget`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget)上的指定事件类型的事件侦听器列表中。



```javascript
function closeNavbar(e) {

​    console.log(e);

​    if (document.body.classList.contains('show-nav') &&

​        e.target !== toggle &&

​        !toggle.contains(e.target) &&

​        e.target !== navbar &&

​        !navbar.contains(e.target)) {

​        document.body.classList.toggle('show-nav');

​        document.body.removeEventListener('click', closeNavbar); 

​    } else if (!document.body.classList.contains('show-nav')) {

​        document.body.removeEventListener('click', closeNavbar);

​    }

}
```



## [一维数组与二维数组互相转换的方法](https://segmentfault.com/a/1190000016038640)



### 一维数组转化为二维数组

```
  let baseArray = [1, 2, 3, 4, 5, 6, 7, 8];
  let len = baseArray.length;
  let n = 4; //假设每行显示4个
  let lineNum = len % n === 0 ? len / n : Math.floor( (len / n) + 1 );
  let res = [];
  for (let i = 0; i < lineNum; i++) {
    // slice() 方法返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象。且原始数组不会被修改。
    let temp = baseArray.slice(i*n, i*n+n);
    res.push(temp);
  }
  console.log(res);
```



### 二维数组转化为一维数组

```
const arr=[[1,2,3],[3,4],[5]];
console.log([].concat.apply([],arr));
```

