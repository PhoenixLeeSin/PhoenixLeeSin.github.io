# uView样式穿透

## 1. 自定义组件下（在微信小程序中）

### 1. 样式穿透需要在父页面下进行修改

### 2. 保险起见，写一个view标签给个class，里面放上你的自定义组件然后在页面中进行样式穿透

### 3.scss 样式穿透，尽量采用v-deep



### 示例 修改 u-input下的边框颜色

## 自定义组件代码



```vue
<view class="common-row">
  <text class="title">销售价</text>
  <u-input border="surround" v-model="price">
    <u--text
      text="¥"
      slot="prefix"
    ></u--text>
  </u-input>
</view>
```



## 父页面



```vue
<view class="main">
  <ordercategoryheader :item="productInfoItem"></ordercategoryheader>
  <view class="productitem" v-for="(element, index) in products" :key="index">
    <submitorderitem :item="element.item"></submitorderitem>
  </view>
</view>
```



## 父页面样式

```scss

 .productitem {
    margin: 16rpx;
        ::v-deep .u-border {
           border-color: #007AFF !important;
        }
  }
```

