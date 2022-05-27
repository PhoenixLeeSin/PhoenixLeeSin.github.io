# 一：

### 第一步：搜索插件：ESLint，并存于开启状态

### 第二步：追加配置：settings.json (在VSCode设置里面搜索)



```javascript
{  ...   "editor.codeActionsOnSave":{      "source.fixAll.eslint":true  },      ... }
```



### 自己的setting.json文件

```javascript
{

​    "editor.tabSize": 2,

​    "settingsSync.ignoredExtensions": [],

​    "editor.formatOnType": true,

​    "editor.formatOnSave": true,

​    "editor.fontSize": 18,

​    "editor.fontFamily": "'Jetbrains Mono'",

​    "[dart]": {

​        "editor.formatOnSave": true,

​        "editor.formatOnType": true,

​        "editor.rulers": [

​            80

​        ],

​        "editor.selectionHighlight": false,

​        "editor.suggest.snippetsPreventQuickSuggestions": false,

​        "editor.suggestSelection": "first",

​        "editor.tabCompletion": "onlySnippets",

​        "editor.wordBasedSuggestions": false

​    },

​    "standard.autoFixOnSave": true,

​    "http.proxyAuthorization": null,

​    "editor.suggest.snippetsPreventQuickSuggestions": false,

​    "leek-fund.fundSort": 0,

​    "workbench.startupEditor": "welcomePage",

​    "editor.wordWrap": "on",

​    "launch": {

​        "configurations": [],

​        "compounds": []

​    },

​    "editor.quickSuggestions": true,

​    "editor.suggestSelection": "first",

​    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",

​    "workbench.colorTheme": "Better Solarized Light",

​    "workbench.iconTheme": "vscode-icons",

​    *//* 

​    "editor.codeActionsOnSave": { *//自动修复*

​        "source.fixAll.eslint": true

​    },

​    "eslint.validate": [

​        "html",

​        "vue",

​        "javascript",

​        "jsx"

​    ],

​    "files.insertFinalNewline": true *// 文件的最后一行空一行*

}
```



### vscode下如果保存之后新增一行不生效或者手动新增一行保存时却被删除了， 请查看是否安装了"JS-CSS-HTML Formatter"插件，卸载重启vs就ok了。



# 二：

## 解决使用ESLint，出现代码中的双引号、尾随逗号和分号不符合规范的报错

## 问题

   vue-cli 构建的项目默认启用 ESLint 进行代码检测，凡是不符合它规范的就会报错，但是 vscode 代码格式化中的有些规则和 ESLint 规则相反。

1. 按 ESLint 的规则：单引号，结束无逗号，末尾没有分号

```vue
<script>
export default {
  name: 'HelloWorld'
}
</script>
```

1. 使用 vscode 的格式化：双引号，逗号， 末尾带分号

```vue
<script>
export default {
  name: "HelloWorld",
};
</script>
```

![](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/UgkXQr.jpg)

## 解决

   在项目根目录创建 .prettierrc 文件，添加如下配置内容，然后重新启动项目即可

```vue
{
  "semi": false, //格式化为单引号
  "singleQuote": true //格式化不加分号
  "trailingComma": false //格式化去除尾部逗号
}
```



