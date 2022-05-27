# uni-app修改页面根节点page属性

## 因为==scoped==的原因，所以改变不了全局的page样式。

### 可以在添加一行 style样式，单独修改page样式，其他样式保持不变。

```scss

///设置page
<style>
page {
	height: 100%;
	display: flex;
}
</style>

/// 设置公共样式
<style lang="scss" scoped>

.home {
	display: flex;
	flex-direction: column;
	flex: 1;
	border: 5px solid green;
}


```

