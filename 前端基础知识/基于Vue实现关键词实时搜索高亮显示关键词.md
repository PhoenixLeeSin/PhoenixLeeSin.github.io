# [基于Vue实现关键词实时搜索高亮显示关键词]

### [链接](https://segmentfault.com/a/1190000015691987)



> **实时搜索**

实时搜索通过触发input事件和定时器来实现

```routeros
<input v-model="keyWords" type="text" placeholder="请输入关键词" @input="handleQuery">
```

在每次输入框的值变化的时候都会执行handleQuery方法

```kotlin
    clearTimer () {
      if (this.timer) {
        clearTimeout(this.timer)
      }
    },
    handleQuery (event) {
      this.clearTimer()
      console.log(event.timeStamp)
      this.timer = setTimeout(() => {
        console.log(event.timeStamp)
        // console.log(this.lastTime)
        // if (this.lastTime - event.timeStamp === 0) {
        this.$http.post('/api/vehicle').then(res => {
          console.log(res.data.data)
          this.changeColor(res.data.data)
        })
        // }
      }, 2000)
    },
```

在handleQuery方法中有一个定时器，通过设置时间来控制搜索的执行，由于输入时input的框中的值总是变化的，所以每次变化都会执行一次handleQuery，我们通过clearTimer方法清除timer定时器，如果两次输入的间隔时间小于你设置的时间间隔（2s）的话第一个定期器将会被清除，同时执行第二个定时器。这样就实现了实施搜多的控制，而不是每次输入的时候就去请求数据。

**注意**：如果时间设置过短或者说我们服务器请求较慢的话，可能第一次查询还没有返回便进行了第二次查询，那么返回的数据将是两次查询的结果，造成查询结果的混乱，如果使用的是axios可以利用axios.CancelToken来终止上一次的异步请求，防止旧关键字查询覆盖新输入的关键字查询结果。

> **高亮显示**

通过RegExp实现对关键词的替换，通过添加class实现关键词高亮显示

```stylus
  changeColor (resultsList) {
      resultsList.map((item, index) => {
        // console.log('item', item)
        if (this.keyWords && this.keyWords.length > 0) {
          // 匹配关键字正则
          let replaceReg = new RegExp(this.keyWords, 'g')
          // 高亮替换v-html值
          let replaceString =
            '<span class="search-text">' + this.keyWords + '</span>'
          resultsList[index].name = item.name.replace(
            replaceReg,
            replaceString
          )
        }
      })
      this.results = []
      this.results = resultsList
}
```

在查询到结果后执行changeColor方法将查询出来的数据传递过来通过RegExp来将关键词替换成huml标签，同时用vue中的v-html进行绑定。最后将demo完整的代码展示出来

```xml
<template>
  <div class="Home">
   <input v-model="keyWords" type="text" placeholder="请输入关键词"  @input="handleQuery">
   <ul>
       <li v-for="(item,index) in results" :key='index' v-html='item.name'></li>
   </ul>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      keyWords: '',
      results: []
    }
  },
  methods: {
    clearTimer () {
      if (this.timer) {
        clearTimeout(this.timer)
      }
    },
    handleQuery (event) {
      this.clearTimer()
      console.log(event.timeStamp)
      this.timer = setTimeout(() => {
        console.log(event.timeStamp)
        // console.log(this.lastTime)
        // if (this.lastTime - event.timeStamp === 0) {
        this.$http.post('/api/vehicle').then(res => {
          console.log(res.data.data)
          this.changeColor(res.data.data)
        })
        // }
      }, 2000)
    },

    changeColor (resultsList) {
      resultsList.map((item, index) => {
        // console.log('item', item)
        if (this.keyWords && this.keyWords.length > 0) {
          // 匹配关键字正则
          let replaceReg = new RegExp(this.keyWords, 'g')
          // 高亮替换v-html值
          let replaceString =
            '<span class="search-text">' + this.keyWords + '</span>'
          resultsList[index].name = item.name.replace(
            replaceReg,
            replaceString
          )
        }
      })
      this.results = []
      this.results = resultsList
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.search-text{
color: red;
}
</style>

```