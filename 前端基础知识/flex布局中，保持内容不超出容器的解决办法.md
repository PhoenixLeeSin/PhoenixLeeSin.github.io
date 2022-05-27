移动端里面，flex 布局很好用，它能够根据设备宽度来自动调整容器的宽度，但是最近在做项目的时候发现一个问题：

一个li里面设置了flex,flex: 0 0 33.333%,然后想让子元素里面的文字超出flex定义宽度后自动省略。

```xml
<li>
    <a href="">
        <img src="https://img30.360buyimg.com/focus/s140x140_jfs/t13411/188/926813276/3945/a4f47292/5a1692eeN105a64b4.png" alt=""> 
        <p>小米小米小米小米小米小米小米小米小米小米小米小米</p>
    </a>
</li>
ul{
     display: flex;
}
li{
    -webkit-box-flex: 0;
    -ms-flex: 0 0 33.333%;
    flex: 0 0 33.333%;
    text-align: center;
    padding: 0 1.333vw;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    margin-bottom: 2.667vw;
}
li p{
    font-size: 3.2vw;
    color: #8F8E94;
    text-overflow: ellipsis;
    white-space: nowrap;
}
```

这时候会发现，p的文字可能会非常长，一些设备下需要隐藏显示，即不换行，并留下省略符…作标记。
这里会发现text-overflow: ellipsis不生效，省略符根本没有出现。而且因为设置了 nowrap 会发现文字会将 content 撑开，导致内容超出了屏幕。所以必须要解决这个问题。

> 尝试取消父元素.li的flex: 0 0 33.33%，无效。
> 尝试取消ul容器的display: flex，省略号出现。

因此猜测是flex布局的问题，进一步猜测省略符需要对父元素限定宽度。
尝试对父元素li设置width: 100%无效，但是设置width: 0可行。即：

```css
li{
    flex: 0 0 33.333%;
    width: 0
}
```

如果不设置宽度，li可以被子节点无限撑开；因此p总有足够的宽度在一行内显示所有文本，也就不能触发截断省略的效果。测试还有一种方法可以达到效果：

```css
li{
    flex: 0 0 33.333%;
    overflow: hidden;
}
```

上面的二种方法都可以达到我们需要的效果，即给 li 设置了 flex 的值 的时候，它会动态的获得父容器的剩余宽度，且不会被自己的子元素把内容撑开。

> 给html, body设置max-width，元素似乎能强行撑开页宽；
> 给body设置overflow，页宽不能被撑开了，但元素宽度还在，即元素本身还是溢出；
> 给html, body同时设置max-width和overflow，页宽限定在max-width内，元素本身还是溢出；
> 给.main容器设置overflow: hidden，同理.main是不溢出了，.notice本身还是溢出；
> 给.notice元素设置width或max-width，虽然宽度受限，但在特定宽度下省略符…显示不全，有时只显示两个点..