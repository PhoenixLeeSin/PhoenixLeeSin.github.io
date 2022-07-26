# Element el-table实现单选操作

#### 在使用el-table实现选择操作的时候，官方提供了一种多选功能，将type设置为selection。而有时候因项目需求，需要进行单选操作，接下来通过一个简单的例子，实现el-table表格单选操作。显然要实现单选，需要用el-radio标签来实现，代码如下：



```vue
<el-table
   ref="multipleTable"
   :data="tableData"
   :header-cell-style="{background:'#F8F8F9'}"
   style="width: 100%;margin-top: 20px">
   <el-table-column label="选择" align="center" width="65">
     <template slot-scope="scope">
       <el-radio :label="scope.$index" v-model="radio"
                 @change.native="getCurrentRow(scope.row)"></el-radio>
     </template>
   </el-table-column>
   <el-table-column align="center" label="编号" width="160" prop="studentCode"></el-table-column>
   <el-table-column align="center" prop="name" label="姓名" width="120"></el-table-column>
   <el-table-column align="center" prop="phone" label="账号"></el-table-column>
 </el-table>
```



#### 在使用el-radio标签的时候，需要绑定label的值，这里label我绑定的是index，有时候在单选位置是不需要将label的值显示出来，这个时候只需要在el-radio标签里面加上'&nbsp ;'即可。

```vue

<el-radio   :label="scope.$index" 
            v-model="radio"
            @change.native="getCurrentRow(scope.row)">&nbsp;</el-radio>
```



#### js代码实现如下：

```javascript
data: {
    return() {
        templateSelection: {}，
        radio: '',
        tableData: []
    }
}
methods: {
   // do something
    getCurrentRow(row){ 
        // 获取选中数据   row表示选中这一行的数据，可以从里面提取所需要的值
        this.templateSelection = row
    }
}
```

