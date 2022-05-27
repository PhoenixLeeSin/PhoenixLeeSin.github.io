# 在VSCode配置RN的ESLint

## 1. 下载插件 ESLint以及 Prettier

```json
 npm install eslint -g
```





## 2. 工程下添加相关配置文件

###  2.1  添加.eslintrc.js配置文件

```objective-c
module.exports = {
  // 环境里定义了一组预定义的全局变量
  "env": {
      "browser": true,
      "commonjs": true,
      "es6": true
  },
  // extends属性表示启用一系列核心规则，若有plugins属性表示同时启用插件的核心规则
  "extends": [
      "eslint:recommended",
      'plugin:react/recommended'
  ],
  // 设置解析器
  "parser": "babel-eslint",
  // 解析器选项
  "parserOptions": {
      // 表示一些附加特性的对象, 支持JSX
      "ecmaFeatures": {
          "jsx": true
      },
      // ECMAScript的版本
      "ecmaVersion": 2018,
      "sourceType": "module"
  },
  // 支持使用的第三方插件，在使用插件之前，你必须使用 npm 安装它。
  "plugins": [
      "react"
  ],
  // 规则配置
  "rules": {
      "indent": [
          "off",
          "tab"
      ],
      "linebreak-style": [
          "off",
          "windows"
      ],
      "quotes": [
          "error",
          "single"
      ],
      "semi": [
          "error",
          "always"
      ],
      "react/jsx-indent-props": [
          "error",
          4
      ],
      "react/no-direct-mutation-state":
          2
      ,
      "no-console":
          0
      ,
      "no-debugger":
          2
  }
};

```



### 2.2 工程项目文件夹vscode下添加settings.json 文件配置

```js
{   *// THIS WILL TRIGGER PRETTIER ON SAVE*

  "prettier.eslintIntegration": true,  

  "editor.tabSize": 2,



  "eslint.validate": [



​    "javascript",



​    "javascriptreact",



​    "vue"



  ],



  "vetur.validation.template": false,



  "editor.formatOnSave": true,



  "editor.codeActionsOnSave": {



​    "source.fixAll": true



  }



}
```



## 添加完成后 之前的文件报错说明已经生效。