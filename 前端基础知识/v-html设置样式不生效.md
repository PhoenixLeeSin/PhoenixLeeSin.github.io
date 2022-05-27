# v-html设置样式不生效

#### 1.由于scoped影响，导致v-html设置样式不生效；可以去掉scoped，不推荐，导致样式混乱；也可以在全局style样式中设置样式，当前组件仍使用scoped样式。

#### 2.使用深度>>> 或/deep/ 样式选择器，进行scoped穿透。 （CSS使用 >>>    scss使用/deep/）

### 3.外嵌一层，在外层上设置某些样式。



### 示例

<style lang="scss" scoped



```scss
.container {
  background: $inquiryBgWhiteColor;
  padding: 0.18rem 0.12rem;
  .list {
    padding-left: 0.14rem;
    &-item {
      line-height: 0.41rem;
      border-bottom: 1px solid #F1F1F1;
      font-size: 0.14rem;
      color: #282828;
      @include text-ellipsis()
    }
  }
  .list-item /deep/ .keywords-text {
    color: $blueColor;
  }
}
```

</style>