# Vue和Vue CLI关系



## **做个类比：**

- #### **Vue CLI** = **Vue** + **一堆的[js插件](https://www.zhihu.com/search?q=js插件&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A343760714})**。

- #### **Spring Cloud** = **Spring Boot** + **一堆第三方组件**。

### **使用方式：**

- #### Vue CLI是一个脚手架，通俗点说就是代码生成器，可以快速生成一套基于Vue完整的[前端框架](https://www.zhihu.com/search?q=前端框架&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A343760714})，单独编译，单独部署。可以再集成各种第三方插件，扩展出更多的功能。

- #### Vue是渐近式框架，你可以用它一个功能，也可以用全家桶。比如你可以在老的jsp或thymeleaf项目里，引入vue.js，只用它核心的数据绑定功能

## **版本号对应：**

- #### Vue CLI 4.5以下，对应的是Vue2

- #### Vue CLI 4.5及以上，对应的是Vue3，当然，创建项目的时候可以选择Vue2



- ## vue安装：

#### npm install -g @vue/cli （安装的是最新版）

#### npm install vue-cli@2.9.6 （指定版本安装【指定版本为3.0以下版本】，其中2.9.6为版本号）

#### npm install -g @vue/cli@3.11.0（指定版本安装【指定版本为3.0以上版本】，其中3.11.0为版本号）





- ### 通过命令行查询可用的包的版本号（3.0以下版本）： 

#### npm view vue-cli versions --json





- ### vue版本查看：

#### vue -V





- ### 版本号对应：

> #### Vue CLI 4.5以下，对应的是Vue2
>
> #### Vue CLI 4.5及以上，对应的是Vue3，当然，创建项目的时候可以选择Vue2