# Vue 中普通 js 文件中获取 this (vue 对象)

### 在 `main.js` 中导出 `vue` 实例

```js
const vue = new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

export default vue
复制代码
```

### 在需要用到的页面导入 `@/main` 获得 `vue` 对象

```js
import vue from '@/main'

// 使用
vue.$message.success('导入成功')
```

