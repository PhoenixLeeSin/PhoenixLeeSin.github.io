## [原文](https://blog.csdn.net/weixin_43233914/article/details/118937811)

### 1.安装 eslint

```
yarn add eslint --save-dev
```

1

### 2.安装 ts 解析器以及 ts 规则补充

```
yarn add @typescript-eslint/parser --save-dev
yarn add @typescript-eslint/eslint-plugin --save-dev
```

##### eslint 默认使用 Espree 进行解析，无法识别 ts 的一些语法，所以需要安装一个 ts 的解析器 @typescript-eslint/parser，用它来代替默认的解析器

##### @typescript-eslint/eslint-plugin 作为 eslint 默认规则的补充，提供了一些额外的适用于 ts 语法的规则。

### 3.支持 tsx

```
yarn add eslint-plugin-react --save-dev
```

##### 由于是 react 项目，所以还需要插件 eslint-plugin-react 来支持 .tsx

### 4.在项目根目录创建 .eslintrc.js

#### 当运行 ESLint 的时候检查一个文件的时候，它会首先尝试读取该文件的目录下的配置文件，然后再一级一级往上查找，将所找到的配置合并起来，作为当前被检查文件的配置。

```
module.exports = {
  parser: "@typescript-eslint/parser",
  plugins: [
    "react",
    "react-hooks",
    "@typescript-eslint/eslint-plugin"
  ],
  settings: {
    react: {
      "version": "detect"
    }
  },
  rules: {
    "no-console": process.env.NODE_ENV === 'production'
      ? 'error'
      : 'off',
    "no-debugger": process.env.NODE_ENV === 'production'
      ? 'error'
      : 'off',
    

   	 // 其余配置项自行添加

  }
}
```



### 5.安装 prettier

```
yarn add prettier --save-dev
```

1

#### 安装 eslint-plugin-prettier

```
yarn add eslint-plugin-prettier --save-dev
```



#### eslint-plugin-prettier 插件会调用prettier对你的代码风格进行检查，其原理是先使用 prettier 对你的代码进行格式化，然后与格式化之前的代码进行对比，如果出现了不一致，这个地方就会被 prettier 进行标记。

### 6.修改.eslintrc.js

##### 修改 plugins 字段，增加一项：

```
plugins: [
  "react",
  "react-hooks",
  "@typescript-eslint/eslint-plugin",
  "prettier" // 这一项为增加项
]
```



##### 修改 rules 字段，增加一项：

##### 表示被 prettier 标记的地方抛出错误信息

```
rules: {
  "prettier/prettier": "error", // 这一项为增加项
  ...其余若干配置项
}
```


7. ### 在项目根目录创建 .prettierrc.js

配置供参考

```json
module.exports = {
  // 强制使用单引号
  singleQuote: true,
  // 字符串使用单引号
  singleQuote: true,
  // 大括号内的首尾需要空格
  bracketSpacing: true,
  // 末尾不需要逗号
  trailingComma: 'none',
  // 箭头函数参数括号
  arrowParens: 'avoid',
  // 在jsx中把'>' 是否单独放一行
  jsxBracketSameLine: true,
  // 使用默认的折行标准
  proseWrap: 'preserve',
  // 根据显示样式决定 html 要不要折行
  htmlWhitespaceSensitivity: 'css',
  // 换行符使用 crlf/lf/auto
  endOfLine: 'auto'
}
```



### 8.完整 .eslintrc.js 文件

```json
module.exports = {
  parser: "@typescript-eslint/parser",
  plugins: [
    "react",
    "react-hooks",
    "@typescript-eslint/eslint-plugin",
    "prettier"
  ],
  settings: {
    react: {
      "version": "detect"
    }
  },
  rules: {
    // "error"/"off" 开启/关闭prettier
    "prettier/prettier": "error",
    "no-console": process.env.NODE_ENV === 'production' ? 'error' : 'off',
    "no-debugger": process.env.NODE_ENV === 'production' ? 'error' : 'off',
	

	// 其余配置项自行添加

  }
}
```



#### 配置项参考

	// 取消函数参数需要重新赋值给另一个变量才能使用
	"no-param-reassign": [0],
	// 取消 { a, b, c } 多个变量需要换行
	"object-curly-newline": [0],
	
	// 禁用var，用let和const代替
	"no-var": 2,
	// 开启强制单引号
	"quotes": [2, "single"],
	// 强制全等( === 和 !==)
	"eqeqeq": 2,
	// 语句强制分号结尾
	"semi": [2, "always"],
	// 禁止出现未使用的变量
	"@typescript-eslint/no-unused-vars": [2],
	// 箭头函数参数括号，一个参数时可省略括号
	"arrow-parens": [2, "as-needed"],
	// 箭头函数，箭头前后空格
	"arrow-spacing": [2, { before: true, after: true }],
	// 禁止对象最后一项逗号
	"comma-dangle": [2, "never"],
	// 单行代码/字符串最大长度
	"max-len": [2, { code: 120 }],
	// jsx缩进2个空格
	"react/jsx-indent": [2, 2],
	// 文件末尾强制换行
	"eol-last": 2,
	
	// react配置
	// 强制组件方法顺序
	"react/sort-comp": [2],
	// 结束标签，组件省略闭合标签，html不省略闭合标签
	"react/self-closing-comp": [2, { "component": true, "html": false }],
	// 检查 Hook 的规则，不允许在if for里面使用
	"react-hooks/rules-of-hooks": [2],
	// 检查 effect 的依赖
	"react-hooks/exhaustive-deps": [2]


#### 如需关闭eslint以及prettier

#### 1.在 package.json 文件中找到 eslintConfig 配置 ，并添加 rules 字段以及三个属性：

```json
"eslintConfig": {
  "extends": [
    "react-app",
    "react-app/jest"
  ],
  "rules": {
    "no-undef": "off",
    "no-restricted-globals": "off",
    "no-unused-vars": "off"
  }
},
```



#### 2.如果在根目录新建了 .eslintrc.js 文件，需要将这个文件的内容全部注释

#### 3.重启项目 yarn start

##### 遇到问题

##### yarn start 启动出现问题

```
Failed to load config "react-app" to extend from.
Referenced from: /home/rudy/projects/webstorm/my-app/package.json
```

#### 解决：

```
yarn add eslint-config-react-app --save-dev
```

