# vue项目cli4中使用别名



[参考资料](http://www.4k8k.xyz/article/qq_44766883/106108615)

## 创建vue.config.js文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513223722110.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NzY2ODgz,size_16,color_FFFFFF,t_70)

## 

## 写入配置



```js
const path = require('path');//引入path模块
function resolve(dir){
    
    return path.join(__dirname,dir)//path.join(__dirname)设置绝对路径
}

module.exports={
    
    chainWebpack:(config)=>{
    
        config.resolve.alias
            .set('@',resolve('./src'))
            .set('components',resolve('./src/components'))
            .set('views',resolve('./src/views'))
            .set('assets',resolve('./src/assets'))
        //set第一个参数：设置的别名，第二个参数：设置的路径

    }
}
```



## 使用

### 1. js文件下直接使用别名就可以

```js
import 'styles/normalize.css'

import 'styles/base.css'

import 'styles/iconfont.css'
```



### 2. css下使用需要在前面添加‘~’

<style lang="stylus" scoped>
@import '~styles/varibles.styl';
</style>  