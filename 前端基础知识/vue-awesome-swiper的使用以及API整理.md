[转载自](https://segmentfault.com/a/1190000014609379)





## 一、先说一个看关于vue-awesome-swiper的一个坑

​    vue项目的package.json中显示的**"vue-awesome-swiper": "^2.5.4"**，用**npm install**自动安装依赖时装的版本为**"version": "2.6.7"**
​    2.5.4与2.6.7都是基于swiper3的，从官网上swiper3的教程来看并不需要单独引入样式文件，而2.6.7版本需要单独引入swiper/dist/css目录下的swiper.css样式文件（类似于swiper4）。

​    并且2.6.7版本swiper位于**node_modules/vue-awesome-swiper/node_modules**下；2.5.4不需要单独引入样式文件，并且swiper直接位于**node_modules**文件夹下。

## 二、基本使用方法

### 1.安装版本

```
    "swiper": "^4.5.1",
​    "vue": "^2.6.11",
​    "vue-awesome-swiper": "^3.1.3",
```



### 2.引用

```javascript
    /*全局引入*/
    import VueAwesomeSwiper from 'vue-awesome-swiper'
    import 'swiper/dist/css/swiper.css'//这里注意具体看使用的版本是否需要引入样式，以及具体位置。
    Vue.use(VueAwesomeSwiper, /* { default global options } */)
    /*组件方式引用*/
    import 'swiper/dist/css/swiper.css'////这里注意具体看使用的版本是否需要引入样式，以及具体位置。
    import { swiper, swiperSlide } from 'vue-awesome-swiper'
    export default {
    components: {
      swiper,
      swiperSlide
    }
```

### 3.使用

就是一般组件的用法

```xml
    <swiper :options="swiperOption">
      <swiper-slide><img src="static/images/jay.jpg"></swiper-slide>
      <swiper-slide><img src="static/images/jay.jpg"></swiper-slide>
      <swiper-slide><img src="static/images/jay.jpg"></swiper-slide>
      <swiper-slide><img src="static/images/jay.jpg"></swiper-slide>
      <swiper-slide><img src="static/images/jay.jpg"></swiper-slide>
      <swiper-slide><img src="static/images/jay.jpg"></swiper-slide>
    </swiper>
    <!--以下看需要添加-->
    <div class="swiper-scrollbar"></div> //滚动条
    <div class="swiper-button-next"></div> //下一项
    <div class="swiper-button-prev"></div> //上一项
    <div class="swiper-pagination" slot="pagination"></div> //标页码  slot插槽需要写上
  data(){
    return{
      swiperOption: {//swiper3
      autoplay: 3000,
      speed: 1000,
      }
    }
  }
    
```

## 三、配置

| 参数                          | 类型（swiper3）      | 默认值（swiper3）                                            | 类型（swiper4） | 默认值（swiper4）                                            | 说明                                                         |
| ----------------------------- | -------------------- | ------------------------------------------------------------ | --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| autoplay                      | Number/Boolean       | 0/false                                                      | Object          | [autoplay](https://segmentfault.com/a/1190000014609379#autoplay) | 自动切换                                                     |
| speed                         | Number               | 300                                                          | Number          | 300                                                          | 切换速度                                                     |
| loop                          | Boolean              | false                                                        | Boolean         | false                                                        | loop模式                                                     |
| loopAdditionalSlides          | Number               | 0                                                            | Number          | 0                                                            | loop模式下会在slides前后复制若干个slide,，前后复制的个数不会大于原总个数。 |
| loopedSlides                  | Number               | 1                                                            | Number          | 1                                                            | 在loop模式下使用slidesPerview:'auto',还需使用该参数设置所要用到的loop个数。 |
| direction                     | String               | horizontal                                                   | String          | horizontal                                                   | Slides的滑动方向                                             |
| autoplayDisableOnInteraction  | Boolean              | true                                                         | -               | -                                                            | 用户操作swiper之后，是否禁止autoplay                         |
| autoplayStopOnLast            | Boolean              | true                                                         | -               | -                                                            | 切换到最后一个slide时停止自动切换                            |
| grabCursor                    | Boolean              | false                                                        | Boolean         | false                                                        | 鼠标覆盖Swiper时指针会变成手掌形状，拖动时指针会变成抓手形状 |
| width                         | Number               | -                                                            | Number          | -                                                            | 强制Swiper的宽度                                             |
| height                        | Number               | -                                                            | Number          | -                                                            | 强制Swiper的高度                                             |
| autoHeight                    | Boolean              | false                                                        | Boolean         | false                                                        | 自动高度                                                     |
| **freeMode:**                 |                      |                                                              |                 |                                                              | **swiper3/4 api相同**                                        |
| freeMode                      | Boolean              | false                                                        | -               | -                                                            | free模式，slide会根据惯性滑动且不会贴合                      |
| freeModeMomentum              | Boolean              | true                                                         | -               | -                                                            | free模式动量。若设置为false则关闭动量，释放slide之后立即停止不会滑动。 |
| freeModeMomentumRatio         | Number               | 1                                                            | -               | -                                                            | free模式动量值（移动惯量）                                   |
| freeModeMomentumVelocityRatio | Number               | 1                                                            | -               | -                                                            | 动量反弹                                                     |
| freeModeMomentumBounce        | Boolean              | true                                                         | -               | -                                                            | Slides的滑动方向                                             |
| freeModeMomentumBounceRatio   | Number               | 1                                                            | -               | -                                                            | 值越大产生的边界反弹效果越明显，反弹距离越大。               |
| freeModeSticky                | Boolean              | false                                                        | -               | -                                                            | 使得freeMode也能自动贴合。                                   |
| **effect:**                   |                      |                                                              |                 |                                                              | **swiper3/4 api相同**                                        |
| effect                        | String               | slide                                                        | -               | -                                                            | slide的切换效果。                                            |
| fade/fadeEffect(4)            | Object               | [fade](https://segmentfault.com/a/1190000014609379#fade)     | -               | -                                                            | fade效果参数。                                               |
| cube/cubeEffect               | Object               | [cube](https://segmentfault.com/a/1190000014609379#cube)     | -               | -                                                            | cube效果参数。                                               |
| coverflow/coverflowEffect     | Object               | [coverflow](https://segmentfault.com/a/1190000014609379#coverflow) | -               | -                                                            | coverflow效果参数。                                          |
| flip/flipEffect               | Object               | [flip](https://segmentfault.com/a/1190000014609379#flip)     | -               | -                                                            | flip效果参数。                                               |
| **Zoom:**                     |                      |                                                              |                 |                                                              |                                                              |
| zoom                          | Boolean              | false                                                        | Object          | [zoom](https://segmentfault.com/a/1190000014609379#zoom)     | 焦距功能：双击slide会放大，并且在手机端可双指触摸缩放。      |
| zoomMax                       | Number               | 3                                                            | -               | -                                                            | 最大缩放焦距（放大倍率）.                                    |
| zoomMin                       | Number               | 1                                                            | -               | -                                                            | 最小缩放焦距（缩小倍率）。                                   |
| zoomToggle                    | Boolean              | true                                                         | -               | -                                                            | 设置为false，不允许双击缩放，只允许手机端触摸缩放。          |
| **pagination:**               |                      |                                                              |                 |                                                              |                                                              |
| pagination                    | String               | -                                                            | Object          | [pagination](https://segmentfault.com/a/1190000014609379#pagination) | 分页器容器的css选择器或HTML标签                              |
| paginationType                | String               | -                                                            | -               | -                                                            | bullets’ 圆点（默认）‘fraction’ 分式 ‘progress’ 进度条‘custom’ 自定义 |
| paginationClickable           | Boolean              | false                                                        | -               | -                                                            | 点击分页器的指示点分页器控制Swiper切换                       |
| paginationHide                | Boolean              | false                                                        | -               | -                                                            | 默认分页器会一直显示                                         |
| paginationElement             | String               | span                                                         | -               | -                                                            | 设定分页器指示器（小点）的HTML标签。                         |
| **Navigation Buttons:**       |                      |                                                              |                 | [swiper4 navigation](https://segmentfault.com/a/1190000014609379#navigation) |                                                              |
| nextButton                    | string / HTMLElement | -                                                            | -               | -                                                            | 前进按钮的css选择器或HTML元素。                              |
| prevtButton                   | string / HTMLElement | -                                                            | -               | -                                                            | 后退按钮的css选择器或HTML元素。                              |
| **Scrollbar:**                |                      |                                                              |                 |                                                              |                                                              |
| scrollbar                     | string / HTMLElement | -                                                            | Object          | [swiper4 Scrollbar](https://segmentfault.com/a/1190000014609379#Scrollbar) | Scrollbar容器的css选择器或HTML元素                           |
| scrollbarHide                 | Bolean               | true                                                         | -               | -                                                            | 滚动条是否自动隐藏。                                         |
| scrollbarDraggable            | Boolean              | false                                                        | -               | -                                                            | 该参数设置为true时允许拖动滚动条。                           |
| scrollbarSnapOnRelease        | Boolean              | false                                                        | -               | -                                                            | 如果设置为true，释放滚动条时slide自动贴合。                  |

### [**autoplay:**](https://segmentfault.com/a/1190000014609379#REautoplay)

```javascript
  autoplay: {
    delay: 3000, //自动切换的时间间隔，单位ms
    stopOnLastSlide: false, //当切换到最后一个slide时停止自动切换
    stopOnLastSlide: true, //如果设置为true，当切换到最后一个slide时停止自动切换。
    disableOnInteraction: true, //用户操作swiper之后，是否禁止autoplay。
    reverseDirection: true, //开启反向自动轮播。
    waitForTransition: true, //等待过渡完毕。自动切换会在slide过渡完毕后才开始计时。
  },
```

### [**fade:**](https://segmentfault.com/a/1190000014609379#REfade)

```javascript
  fadeEffect: {
    crossFade: false,
  }
```

### [**cube:**](https://segmentfault.com/a/1190000014609379#REcube)

```javascript
  cubeEffect: {
    slideShadows: true, //开启slide阴影。默认 true。
    shadow: true, //开启投影。默认 true。
    shadowOffset: 100, //投影距离。默认 20，单位px。
    shadowScale: 0.6 //投影缩放比例。默认0.94。
  },
```

### [**coverflow:**](https://segmentfault.com/a/1190000014609379#REcoverflow)

```javascript
  coverflowEffect: {
    rotate: 30, //slide做3d旋转时Y轴的旋转角度。默认50。
    stretch: 10, //每个slide之间的拉伸值，越大slide靠得越紧。 默认0。
    depth: 60, //slide的位置深度。值越大z轴距离越远，看起来越小。 默认100。
    modifier: 2, //depth和rotate和stretch的倍率，相当于depth*modifier、rotate*modifier、stretch*modifier，值越大这三个参数的效果越明显。默认1。
    slideShadows : true //开启slide阴影。默认 true。
  },
```

### [**flip:**](https://segmentfault.com/a/1190000014609379#REflip)

```javascript
   flipEffect: {
    slideShadows : true, //slides的阴影。默认true。
    limitRotation : true, //限制最大旋转角度为180度，默认true。
  }
```

### [**zoom:**](https://segmentfault.com/a/1190000014609379#REzoom)

```javascript
   zoom: {
     maxRatio: 5, //最大倍数
     minRatio: 2, //最小倍数
     toggle: false, //不允许双击缩放，只允许手机端触摸缩放。
     containerClass: 'my-zoom-container', //zoom container 类名
   },
```

### [**navigation:**](https://segmentfault.com/a/1190000014609379#REnavigation)

```javascript
   navigation: {
    nextEl: '.swiper-button-next', //前进按钮的css选择器或HTML元素。
    prevEl: '.swiper-button-prev', //后退按钮的css选择器或HTML元素。
    hideOnClick: true, //点击slide时显示/隐藏按钮
    disabledClass: 'my-button-disabled', //前进后退按钮不可用时的类名。
    hiddenClass: 'my-button-hidden', //按钮隐藏时的Class
   },
```

### [**pagination:**](https://segmentfault.com/a/1190000014609379#REpagination)

```javascript
   pagination: {
    el: '.swiper-pagination',
    type: 'bullets',
    //type: 'fraction',
    //type : 'progressbar',
    //type : 'custom',
    progressbarOpposite: true, //使进度条分页器与Swiper的direction参数相反
    bulletElement : 'li', //设定分页器指示器（小点）的HTML标签。
    dynamicBullets: true,  //动态分页器，当你的slide很多时，开启后，分页器小点的数量会部分隐藏。
    dynamicMainBullets: 2, //动态分页器的主指示点的数量
    hideOnClick: true, //默认分页器会一直显示。这个选项设置为true时点击Swiper会隐藏/显示分页器。
    clickable: true, //此参数设置为true时，点击分页器的指示点分页器会控制Swiper切换。

  },
```

### [**Scrollbar:**](https://segmentfault.com/a/1190000014609379#REScrollbar)

```javascript
   scrollbar: {
     el: '.swiper-scrollbar',
     hide: true, //滚动条是否自动隐藏。默认：false，不会自动隐藏。
     draggable: true, //该参数设置为true时允许拖动滚动条。
     snapOnRelease: true, //如果设置为false，释放滚动条时slide不会自动贴合。
     dragSize: 30, //设置滚动条指示的尺寸。默认'auto': 自动，或者设置num(px)。
   },
```

