# flutter_swiper 修改分页指示器的大小以及自定义



### 修改大小代码

```dart
                pagination: SwiperPagination(

​                  alignment: Alignment.bottomCenter,

​                  builder: DotSwiperPaginationBuilder(

​                      color: Colors.white,

​                      activeColor: Colors.blue,

​                      size: 8.0,

​                      activeSize: 10.0),

​                ),
```



### 自定义代码

#### 自定义的分页指示器类型



```dart
class PageIndicator extends StatefulWidget {
  final SwiperPluginConfig? config;
  final int? count;
  const PageIndicator({Key? key, this.config, this.count}) : super(key: key);

  @override
  _PageIndicatorState createState() => _PageIndicatorState();
}

class _PageIndicatorState extends State<PageIndicator> {

  int index = 0;

  @override

  void initState() {

​    *// TODO: implement initState*

​    widget.config?.pageController?.addListener(_onController);

​    *super*.initState();

  }



  void didUpdateWidget(PageIndicator oldWidget) {

​    if (widget.config?.pageController != oldWidget.config?.pageController) {

​      oldWidget.config?.pageController?.removeListener(_onController);

​      widget.config?.pageController?.addListener(_onController);

​    }

​    *super*.didUpdateWidget(oldWidget);

  }



  @override

  void dispose() {

​    widget.config?.pageController?.removeListener(_onController);

​    *super*.dispose();

  }



  void _onController() {

​    double page = widget.config?.pageController?.page ?? 0.0;

​    index = page.floor();

​    print(index);

​    setState(() {});

  }



  @override

  Widget build(BuildContext context) {

​    return Container(

​      height: 31,

​      alignment: Alignment.bottomCenter,

​      padding: EdgeInsets.only(bottom: 5),

​      decoration: BoxDecoration(

​        *// color: Colors.black38,*

​        borderRadius: BorderRadius.circular(12),

​        gradient: LinearGradient(

​            *//渐变位置*

​            begin: Alignment.topCenter, *//右上*

​            end: Alignment.bottomCenter, *//左下*

​            stops: [0.1, 1.1], *//[渐变起始点, 渐变结束点]*

​            *//渐变颜色[始点颜色, 结束颜色]*

​            colors: [Colors.transparent, Colors.black26]),

​      ),

​      child: Row(

​          mainAxisAlignment: MainAxisAlignment.center, children: _rowWidgets()),

​    );

  }



  List<Widget> _rowWidgets() {

​    List<Widget> list = [];

​    for (int x = 0; x < (widget.count ?? 0); x++) {

​      list.add(x == index ? _linePointWidget() : _pointWidget());

​      list.add(

​        SizedBox(

​          width: 5,

​        ),

​      );

​    }

​    return list;

  }



  Widget _linePointWidget() {

​    return Container(

​      decoration: BoxDecoration(

​        color: Colors.white,

​        borderRadius: BorderRadius.circular(2),

​      ),

​      width: 7,

​      height: 4,

​    );

  }



  Widget _pointWidget() {

​    return Container(

​      decoration: BoxDecoration(

​          color: Colors.white, borderRadius: BorderRadius.circular(2)),

​      width: 4,

​      height: 4,

​    );

  }

}
```



### 使用

```dart
                pagination: SwiperCustomPagination(builder: (context, config) {

​                  return PageIndicator(

​                    count: ctr.state.secondaryBannerList.length,

​                    config: config,

​                  );

​                }),
```

