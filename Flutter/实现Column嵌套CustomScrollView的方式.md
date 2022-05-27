### 实现Column嵌套CustomScrollView的方式：


```
Column(
        mainAxisSize: MainAxisSize.max,
        children: [
          SizedBox(
            height: 44 + ScreenUtil().statusBarHeight,
            child: navBarWidget(
              leftBtnTap: () {
                Get.back();
              },
              title: "注销账户",
            ),
          ),
          Expanded(
            child: CustomScrollView(
              shrinkWrap: true,
              slivers: [
                // _buildNavbar(),
                _buildSliverHeader(),
                _buildSliverList(),
              ],
            ),
          ),
        ],
      )
```

#### Stack的回答
https://stackoverflow.com/questions/44715865/how-to-put-two-listview-in-a-column

You need to provide constrained height to be able to put ListView inside Column. There are many ways of doing it, I am listing few here.

#### Use Expanded for both the list


```
Column(
  children: <Widget>[
    Expanded(child: _list1()),
    Expanded(child: _list2()),
  ],
)
```

#### Give one Expanded and other SizedBox


```
Column(
  children: <Widget>[
    Expanded(child: _list1()),
    SizedBox(height: 200, child: _list2()),
  ],
)
```

#### Use SizedBox for both of them.


```
Column(
  children: <Widget>[
    SizedBox(height: 200, child: _list1()),
    SizedBox(height: 200, child: _list2()),
  ],
)
```

#### Use Flexible and you can change flex property in it, just like Expanded


```
Column(
  children: <Widget>[
    Flexible(child: _list1()),
    Flexible(child: _list2()),
  ],
)
```