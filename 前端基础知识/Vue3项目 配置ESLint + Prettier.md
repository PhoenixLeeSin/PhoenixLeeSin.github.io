

# ESLint配置

参考连接 [🔗](https://www.panyanbin.com/article/b679027e.html)

## 一个项目配置：

#### .prettierrc 

```js
{

  "singleQuote": true,

  "semi": false,

  "bracketSpacing": true,

  "htmlWhitespaceSensitivity": "ignore"

}
```



### .eslintrc.js

```js
module.exports = {
  env: {
    browser: true,
    node: true,
  },
  parser: 'vue-eslint-parser', // 解析.vue文件
  parserOptions: {
    parser: 'babel-eslint',
    sourceType: 'module',
    allowImportExportEverywhere: false,
  },
  extends: ['standard', 'plugin:prettier/recommended', 'prettier'],
  rules: {
    'prettier/prettier': 'error',
    'no-console': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    indent: 0, // 解决：Expected indentation of 0 spaces but found 2，这个报错
    'space-before-function-paren': 0, // 解决：Missing space before function parentheses，这个报错，就是你写的函数，函数名字和后面随之带着的括号之后没有空格，加上之后可以自动加上
  },
}
```



### 项目下的settings.json

```json
{

  "eslint.validate": [

    "javascript",

   "javascriptreact",

   "vue"

  ],

  "vetur.validation.template": false,

  "editor.formatOnSave": true,

  "editor.codeActionsOnSave": {

   "source.fixAll": true

  }

}
```



### package.json

```json
{
  "name": "jingdongvue",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "axios": "^0.24.0",
    "core-js": "^3.6.5",
    "normalize.css": "^8.0.1",
    "vue": "^3.0.0",
    "vue-router": "^4.0.0-0",
    "vuex": "^4.0.0-0"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.5.0",
    "@vue/cli-plugin-eslint": "~4.5.0",
    "@vue/cli-plugin-router": "~4.5.0",
    "@vue/cli-plugin-vuex": "~4.5.0",
    "@vue/cli-service": "~4.5.0",
    "@vue/compiler-sfc": "^3.0.0",
    "@vue/eslint-config-standard": "^5.1.2",
    "babel-eslint": "^10.1.0",
    "eslint": "^7.32.0",
    "eslint-config-prettier": "^8.3.0",
    "eslint-plugin-import": "^2.20.2",
    "eslint-plugin-node": "^11.1.0",
    "eslint-plugin-prettier": "^3.3.1",
    "eslint-plugin-promise": "^4.2.1",
    "eslint-plugin-standard": "^4.0.0",
    "eslint-plugin-vue": "^7.0.0",
    "prettier": "2.4.1",
    "sass": "^1.26.5",
    "sass-loader": "^8.0.2"
  }
}
```





## ❎ context.getPhysicalFilename is not a function

### ✅ [答案](https://github.com/prettier/eslint-plugin-prettier/issues/434)

Also "solved" by reverting to `3.3.1` I guess? This has been working for me:

```js
    "eslint": "^7.32.0",
    "react-scripts": "^4.0.3", 
    "eslint-plugin-prettier": "3.3.1",
```



## ❎ prettier 出现 Couldn't resolve parser "babylon" 错误的解决方法

#### 这种问题就总是很恶心，非技术性的小问题，解决起来总是很花时间。今天在此记录一下我的解决方法：

#### 参考1：https://github.com/prettier/prettier-vscode/issues/1299

#### 参考2：https://prettier.io/docs/en/configuration.html

#### 好像是因为不同版本prettier 的parser 的名字不一样，导致无法解析，只需要把prettier 配置文件里的parser 从babylon 改成 babel就可以。

#### 根据参考2，我找到我项目根目录下的 .prettierrc.yaml ，把

```
parser: babylon
```

改成

```
parser: babel
```

#### 保存即可。

#### 或者根据参考2，找到你用的配置文件，修改后保存即可。

#### 补充：修改后请重启 vscode以生效
