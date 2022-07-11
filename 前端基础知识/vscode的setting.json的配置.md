# vscode的setting.json的配置



## vue项目中新建一个.vscode文件夹，文件夹里增加一个settings.json文件，内容如下

```json
{
    "typescript.preferences.quoteStyle": "single",
    "javascript.preferences.quoteStyle": "single",
    // tab 大小为2个空格
    "editor.tabSize": 2,
    // 保存时格式化
    "editor.formatOnSave": false,
    // 编辑器换行
    "editor.wordWrap": "off",
    // //让函数(名)和后面的括号之间加个空格
    "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
    // 开启 vscode 文件路径导航
    "breadcrumbs.enabled": true,
    // 选择 vue 文件中 template 的格式化工具
    "vetur.format.defaultFormatter.html": "js-beautify-html",
    // 让vue中的js按编辑器自带的ts格式进行格式化
    "vetur.format.defaultFormatter.js": "vscode-typescript",
    // vetur 的自定义设置
    "vetur.format.defaultFormatterOptions": {
        "js-beautify-html": {
            "wrap_line_length": 120,
            "wrap_attributes": "auto",//"force-aligned" //属性强制折行对齐
            "end_with_newline": false
        },
        "prettier": {
            "singleQuote": true,
            "semi": false,
            "printWidth": 100,
            "wrapAttributes": false,
            "sortAttributes": false,
            "trailingComma": "none",
        }
    }
}
```

