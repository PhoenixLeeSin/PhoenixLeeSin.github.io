# Vue-cli@4.x sockjs-node/info失败导致无法热更新

## 第一种原因：

### 最近在写基于vue-cli@4.5写Vue3项目，写着写着发现不太对劲，每次我修改了*.vue文件后都自己手动刷新才显示新修改的内容，我一直记得vue2项目都是有热更新的，而且到Vue3官方文档也说明了脚手架安装的项目自带热更新，拆箱即用

### 一开始我以为是我项目写的过程中修改了某些配置，导致热更新失败了，我又重新创建了一个vue3项目

### 结果项目启动后依旧没有热更新，我打开network一看，居然有一堆报错，因为是sockjs.js，所以我猜测我的热更新失败因该是和这个有关

```javascript
get http://localhost:8080/sockjs-node/info?t=1462183700002 net::ERR_CONNECTION_REFUSED
[WDS] Disconnected!
get http://localhost:8080/sockjs-node/info?t=1462183700002 net::ERR_CONNECTION_REFUSED
[WDS] Disconnected!
get http://localhost:8080/sockjs-node/info?t=1462183700002 net::ERR_CONNECTION_REFUSED
[WDS] Disconnected!
...
```



### 









![1](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/LZHPCv.jpg)

![2](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/56ecxC.jpg)



### 我的报错信息，我看了这个请求的 IP 地址，使用的外部地址，排查了一下该请求应该是走了代理，而我的外部地址并非公网 IP，所以代理之后的请求一定会得不到响应而失败。

#### 解决方案

#### 1. 注释法

####   顾名思义，找到依赖包中的源码，将其注释：

1. #### 进入路径 `/node_modules/sockjs-client/dist/sockjs.js`

2. #### 代码1605行注释掉：

```javascript
try {
        // self.xhr.send(payload);  //本行注释
    } catch (e) {
        self.emit('finish', 0, '');
        self._cleanup(false);
    }
```



#### 2. 配置vue.config

####   vue.config.js中的module.exports中添加如下，然后重启

```javascript
devServer: {
    proxy: 'http://localhost:8080',
    public: '192.168.xxx.xxx:8080'  // 本地ip
}
```



## 第二种原因：

### vue.config.js配置了extract:true,把style提取至独立的css文件了，而不是动态注入javascipt中。



> ##### 是否将组件中的 CSS 提取至一个独立的 CSS 文件中 (而不是动态注入到 JavaScript 中的 inline 代码)。
>
> ##### 同样当构建 Web Components 组件时它总是会被禁用 (样式是 inline 的并注入到了 shadowRoot 中)。
>
> ##### 当作为一个库构建时，你也可以将其设置为 false 免得用户自己导入 CSS。
>
> ##### 提取 CSS 在开发环境模式下是默认不开启的，因为它和 CSS 热重载不兼容。然而，你仍然可以将这个值显性地设置为 true 在所有情况下都强制提取。
>
> 





## 把 extract: true修改为false即可

```
module.exports = {

  *// 基本路径*

  publicPath: './',

  *// 输出文件目录*

  outputDir: 'dist',

  *// 生产环境是否生成 sourceMap 文件*

  productionSourceMap: false,

  *// css相关配置*

  css: {

    *// 是否使用css分离插件 ExtractTextPlugin*

    extract: true,

    *// 开启 CSS source maps?*

    sourceMap: false,

    *// css预设器配置项*

    loaderOptions: {

      sass: {

        data: `@import "@/stylesheets/main.scss";`

      }

    },

    *// 启用 CSS modules for all css / pre-processor files.*

    modules: false

  },

}
```

