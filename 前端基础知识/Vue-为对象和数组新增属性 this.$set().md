# Vue-为对象和数组新增属性 this.$set()

#### 问题描述：vue的data里边是已经声明或者赋值过的对象或者数组（数组里边的值是对象）时，向对象中添加新的属性或者操作[数组元素](https://so.csdn.net/so/search?q=数组元素&spm=1001.2101.3001.7020)，如果更新此属性的值，是不会更新视图的。



### **this.$set(object,key,value)**

#### object：对象名

#### key：,要修改 / 增加的属性名

#### value：新增的属性值





### this.$set(array,index,value)

#### array：数组名

#### index：需要修改的索引值

#### value：新元素

#### 