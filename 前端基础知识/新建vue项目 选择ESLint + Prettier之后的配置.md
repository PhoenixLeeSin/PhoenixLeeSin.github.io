# 新建vue项目 选择ESLint + Prettier之后的配置 / 2022-07-27



## 1. .eslintrc.js

```javascript
module.exports = {
  // 默认情况下，ESLint会在所有父级组件中寻找配置文件，一直到根目录。ESLint一旦发现配置文件中有   "root": true，它就会停止在父级目录中寻找。
  root: true,
  // 该配置属性定义来一组预定义的全局变量。这些环境并不是互斥的，所以你可以同时定义多个。
  env: {
    node: true,
  },
  // extends是扩展插件的意思,用来配置vue.js风格
  extends: [
    "plugin:vue/essential", // 全称 eslint-plugin-vue
    "@vue/prettier", // 全称 eslint-plugin-prettier
  ],
  // ESLint 支持使用第三方插件。在使用插件之前，你必须使用包管理工具安装它。
  // 在配置文件里配置插件时，可以使用 plugins 关键字来存放插件名字的列表。
  // 插件名称可以省略 eslint-plugin- 前缀。
  plugins: ["vue"],
  // 额外的全局变量。我们使用第三方提供的全局变量的时候（例如：jQuery,AMap 等对象），
  // ESLint 并不能识别他们，总是会报错。这个时候，该配置的作用就出现了。
  // 使用 globals 指出你要使用的全局变量。将变量设置为 true 将允许变量被重写，或 false 将不允许被重写。
  globals: {
    // $: false,
    // jquery: false,
    // AMap: false
  },
  // ESLint 的规则。你可以使用注释或配置文件修改你项目中要使用的规则。
  // rules：开启规则和发生错误时报告的等级，规则的错误等级有三种：
  // 0 或'off'：关闭规则。
  // 1 或'warn'：打开规则，并且作为一个警告（并不会导致检查不通过）。
  // 2 或'error'：打开规则，并且作为一个错误（退出码为1，检查不通过）
  rules: {
    // allow debugger during development
    "no-console": process.env.NODE_ENV === "production" ? "error" : "off",
    "no-debugger": process.env.NODE_ENV === "production" ? "error" : "off",
    "prettier/prettier": [
      "error",
      {
        semi: false, // 是否使用分号, 默认true
        singleQuote: true, // 使用单引号, 默认false(在jsx中配置无效, 默认都是双引号)
      },
    ],
  },
  parserOptions: {
    parser: "babel-eslint",
  },
};
```



## 2. 工作区设置后会在项目下生成一个.vscode文件夹下面有setting.json以下是我的配置：

```json
{
  "files.autoSave": "afterDelay",
  "prettier.eslintIntegration": true,
  "terminal.integrated.rendererType": "dom",
  "vsicons.dontShowNewVersionMessage": true,
  "editor.tabSize": 2,
  "editor.suggestSelection": "first",
  "editor.parameterHints.enabled": true,
  "editor.quickSuggestions": {
    "other": true,
    "comments": true,
    "strings": true
  },
  "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
  "emmet.triggerExpansionOnTab": true,
  "emmet.includeLanguages": {
    "vue-html": "html",
    "vue": "html",
    "wxml": "html"
  },
  "eslint.format.enable": false,
  "eslint.validate": [
    "javascript",
    "html",
    "vue"
  ],
  "html.format.contentUnformatted": "",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "files.associations": {
    "*.cjson": "jsonc",
    "*.wxss": "css",
    "*.wxs": "javascript"
  },
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "vetur.validation.interpolation": false,
  "[dart]": {
    "editor.formatOnSave": true,
    "editor.formatOnType": true,
    "editor.rulers": [
      80
    ],
    "editor.selectionHighlight": false,
    "editor.suggest.snippetsPreventQuickSuggestions": false,
    "editor.suggestSelection": "first",
    "editor.tabCompletion": "onlySnippets",
    "editor.wordBasedSuggestions": false
  },
  "minapp-vscode.disableAutoConfig": true,
  "workbench.iconTheme": "vscode-icons",
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "git.confirmSync": false,
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "editor.fontSize": 15,
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "prettier.semi": false,
  "prettier.singleQuote": true,
  "prettier.trailingComma": "none",
  "[json]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "terminal.integrated.fontSize": 15,
  "diffEditor.ignoreTrimWhitespace": false
}
```



### .prettierrc文件

```json
{

  "semi": false,

  "singleQuote": true

}
```