## 示例



### DOM 结构

```

<template>
  <div class="parent">
    <div class="child">
      <nav-header v-if="appType === 0" title="就诊人管理" class="header"></nav-header>

</div>

  </div>
</template>
```



### CSS

```scss
.parent  ::-webkit-scrollbar {
  width: 0;
  display: none;
}
.parent {
  height: calc(100vh - 0.01rem);
  overflow: hidden;
  overflow-y: scroll;
  background: $inquiryBgColor;
  /* 使得ios滑动流畅 */
  -webkit-overflow-scrolling: touch;
}
.child {
  height: 100vh;
  padding-top: 0.74rem;
  background: $inquiryBgColor;
  .patients-info {
    display: flex;
    &-mange {
      line-height: 0.17rem;
      font-size: 0.12rem;
      color: #2F71FB;
    }
  }
}
```

