# 遍历 vuex-store/modules 下的所有文件，自动加载所有的 vuex 的module

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
import getters from './getters'
import actions from './actions.js'
import mutations from './mutations.js'

Vue.use(Vuex)

// https://webpack.js.org/guides/dependency-management/#requirecontext
const modulesFiles = require.context('./modules', true, /\.js$/)

// you do not need `import app from './modules/app'`
// it will auto require all vuex module from modules file
const modules = modulesFiles.keys().reduce((modules, modulePath) => {
  // set './loading.js' => 'loading'
  // set './work' => 'work'
  const moduleName = modulePath.replace(/^\.\/(.*)\.\w+$/, '$1')
  const value = modulesFiles(modulePath)
  modules[moduleName] = value.default
  return modules
}, {})

const store = new Vuex.Store({
  modules,
  getters,
  actions,
  mutations
})

export default store
```

