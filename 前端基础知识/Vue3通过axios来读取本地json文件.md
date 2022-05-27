# Vue3通过axios来读取本地json文件（直接读取 和 通过配置代理转发读取）



## **前端直接存放JSON文件的位置**

### 我们的项目是通过[vue](https://so.csdn.net/so/search?from=pc_blog_highlight&q=vue)-cli3创建的，vue-cli3版本脚手架对外暴露的静态文件入口是public文件夹(原来是static文件夹)，这里本地json文件也应该放在这里

![](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/KgYEXM.png)

## **请求JSON数据**

### 直接通过[axios](https://so.csdn.net/so/search?from=pc_blog_highlight&q=axios)请求即可

```vue
export default {

  name: 'Home',

  components: { HomeHeader, HomeSwiper, HomeIcons, HomeRecommend, HomeWeekend },

  methods: {

​    getHomeInfo() {

​      axios.get('/mock/index.json').then(*this*.getHomeInfoSuc)

​    },

​    getHomeInfoSuc(res) {

​      console.log(res)

​    },

  },

  mounted() {

​    *this*.getHomeInfo()

  },

}
```



## 配置代理转发读取

### [vue](https://so.csdn.net/so/search?from=pc_blog_highlight&q=vue) cli 3.x 版本对整个项目的构建都有很大的改动，没有原先的config文件夹，没有dev.env.js和prod.dev.js，我们要对[webpack](https://so.csdn.net/so/search?from=pc_blog_highlight&q=webpack)做配置，需要自己手动在项目根目录创建一个[vue](https://so.csdn.net/so/search?from=pc_blog_highlight&q=vue).config.js文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112103801508.png)

### 比如我们在组件中想通过axios请求一个api，mock数据在我们本地的public/static/mock下面的index.json文件中：(注意vue cli3里面已经不能访问和src平级的static目录里面的本地静态资源了，全都迁移到了public目录中，也就是之前和src目录平级的static文件夹移动到public目录中就可以正常访问了。比如此时我们可以通过浏览器http://localhost:8080/static/mock/index.json来访问本地的index.json文件)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112104112437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2R5dzMzOTAxOTk=,size_16,color_FFFFFF,t_70)



### 但是如果我们要通过下面这种api形式的请求访问，就肯定不行

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112103905707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2R5dzMzOTAxOTk=,size_16,color_FFFFFF,t_70)



### 所以我们需要一个请求的转发机制，[vue](https://so.csdn.net/so/search?from=pc_blog_highlight&q=vue)-cli3里面需要我们在vue.config.js里配置：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210112104815431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2R5dzMzOTAxOTk=,size_16,color_FFFFFF,t_70)





## public不用写 public不用写 public不用写

```
const path = require('path')
module.exports = {
    devServer: {
        proxy: {
            '/api': {
                target: 'http://localhost:8080',
                pathRewrite: {
                    '^/api': '/static/mock'
                }
            }
        }
    }
}
```

