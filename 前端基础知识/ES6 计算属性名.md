# ES6 计算属性名





### 文档是用于vuex官方文档下mutation写法的理解

```js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```



## 语法应用

### [JavaScript](https://so.csdn.net/so/search?from=pc_blog_highlight&q=JavaScript)语言定义对象的属性，有两种方法。



```javascript
// 方法一
obj.foo = true;

// 方法二
obj['a' + 'bc'] = 123;
```

#### 上面代码的‘方法一’是直接用标识符作为属性名，‘方法二’是用表达式作为属性名，这时要将表达式放在方括号之内。



### ES6 允许字面量定义对象时，用方法二（表达式）作为对象的属性名，即把表达式放在方括号内。

```javascript
let propKey = 'foo';

let obj = {
  [propKey]: true,
  ['a' + 'bc']: 123
};
```



### 下面是另一个例子。

```javascript
var lastWord = 'last word';

var a = {
  'first word': 'hello',
   [lastWord]: 'world'
};

a['first word'] // "hello"
a[lastWord] // "world"
a['last word'] // "world"
```



### 表达式还可以用于定义方法名。

```javascript
let obj = {
  ['h' + 'ello']() {
    return 'hi';
  }
};

obj.hello() // hi
```



## 应用场景

### 当我们进行一个项目开发时（特别是多人项目），要保证命名的可读性和可维护性，这时往往需要进行统一的管理

### 现在一个常用的方法是使用模块化方法（如ES6模块化规范，CommonJS规范，AMD规范，CMD规范），在一个模块中定义一些常量，并进行统一的导出使用，从而保证变量名良好的维护性



```javascript
j//types.js
export const SOME_MUTATION = 'SOME_MUTATION'
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

