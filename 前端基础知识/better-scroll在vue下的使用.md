# better-scroll在vue下的使用

## 1. 安装【package.json】

```
  "dependencies": {
​    "better-scroll": "^2.0.6",
  },
```

## 2. 初始化 【main.js】

```
import VueAwesomeSwiper from 'vue-awesome-swiper'

/// 不同版本引入的css路径会有所不同
import 'swiper/dist/css/swiper.css'



Vue.use(VueAwesomeSwiper */\*  {default options with global component } \*/*)
```



## 3. 使用

#### 结构介绍

> #### 自定义子组件中使用了better-scroll，然后在父组件中使用，在当前情况下父组件不需要额外设置。



### better-scroll【必须在 生命周期updated下执行 另外需要配置一些参数 见完整代码】

```vue
  <template>
  <div class="wrapper" ref="wrapperscroll">
    <div class="content">
      <div class="area">
        <div class="title">热门城市</div>
      </div>
      <div class="hot-cities">
        <div v-for="item in hotCities" :key="item.id" class="hot-city">
          {{ item.name }}
        </div>
      </div>
      <div v-for="item in Object.keys(cities)" :key="item" :ref="item">
        <div class="area">
          <div class="title">{{ item }}</div>
        </div>
        <div class="city-list">
          <div
            v-for="element in cities[item]"
            :key="element.id"
            class="city-item"
          >
            {{ element.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BetterScroll from 'better-scroll'
import { mapState } from 'vuex'
export default {
  name: 'CityList',
  data() {
    return {
      scroll: null,
    }
  },
  props: {
    cities: {},
    hotCities: [],
  },
  mounted() {
    // let scroll = new BetterScroll(this.$refs.wrapper, {})
    // console.log(this.scroll)
    // this.scroll.on('scroll', (event) => console.log(event))
  },
  updated() {
    this.$nextTick(() => {
      if (!this.scroll) {
        this.scroll = BetterScroll(this.$refs.wrapperscroll, {
          probeType: 3,
          scrollY: true,
          scrollX: false,
          //项目启动后不能直接实现滚动的效果，刷新后才能正常滚动
          mouseWheel: true,
        	disableMouse: false,
        	disableTouch: false,
        })
      } else {
        this.scroll.refresh()
      }
    })
  },
  computed: {
    ...mapState(['alphabet']),
  },
  watch: {
    alphabet: function (newValue, oldValue) {
      console.log(newValue, oldValue)
      console.log(this.$refs[newValue][0])
      console.log(this.scroll)
      this.scroll.scrollToElement(this.$refs[newValue][0], 100, 0, 0, 'easing')
    },
  },
}
</script>

<style lang="stylus" scoped>
@import '~styles/varibles.styl';
.wrapper {
  background: #fff
  overflow: hidden
  top: 1.52rem
  height: calc(100vh - 1.52rem)
  .area {
    box-sizing: border-box
    height: 0.72rem
    background: $lineColor
    // background: red
    .title {
      padding: 0.24rem 0.3rem
      line-height: 0.24rem
      color: $darkTextColor
      font-size: 0.24rem
    }
  }
  .hot-cities {
    overflow: hidden
    width: 100%
    .hot-city {
      width: calc((100% - 3px) / 3)
      line-height: 0.9rem
      float: left
      text-align: center
      border-right: 1px solid $lineColor
      border-bottom: 1px solid $lineColor
    }
  }
  .city-list {
    overflow: hidden
    width: 100%
    .city-item {
      width: calc((100% - 4px) / 4)
      line-height: 0.9rem
      float: left
      text-align: center
      border-right: 1px solid $lineColor
      border-bottom: 1px solid $lineColor
    }
  }
}
</style>

```

> ### 注意点 
>
> ### 1. better-scroll必须在 生命周期updated下初始化 如果在mounted（）初始化 造成无法滚动 
>
> ### 2. 初始化中配置probeType: 
>
> ### 3. scrollY: true, 参数，保证可以在Y轴滚动并且相应滚动事件。
>
> ### 4.外层容器设置，必须比内层content高度底 且需要设置overflow: hidden 
>
> ### 5. 滚动到指定位置函数scrollToElement参数加API文档 也可以直接参考这个。

