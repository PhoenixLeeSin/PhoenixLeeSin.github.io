# GetX 结合BottomNavigationBar + PageView使用

### 解决的问题点：

> ### 结构大致流程为 页面push到BottomNavigationBar的页面后，后续的Page0 ,Page1.Page2都是在当前页面的PageView生成，无法获取到对应页面的GetController,解决的方式是在BottomNavigationBar的binding逻辑中懒加载对应的controller。

### Page 代码：

```dart
import 'package:flutter/material.dart';

import './index.dart';

import 'package:get/get.dart';

import 'package:flutter_screenutil/flutter_screenutil.dart';

import '../home/view.dart';

import '../right/view.dart';

import '../me/view.dart';



class BottomNavPage extends GetView<BottomNavController> {

  final iconsMap = {

​    "首页": Icons.home,

​    "权益": Icons.military_tech,

​    "我的": Icons.person

  };



  @override

  Widget build(BuildContext context) {

​    return Obx(() => Scaffold(

​          body: PageView(

​            controller: controller.pageController,

​            onPageChanged: (int index) {},

​            physics: NeverScrollableScrollPhysics(),

​            children: [

​              HomePage(),

​              RightPage(),

​              MePage(),

​            ],

​          ),

​          bottomNavigationBar: _buildBottomNavigationBar(),

​        ));

  }



  Widget _buildBottomNavigationBar() {

​    return BottomNavigationBar(

​      currentIndex: controller.state.currentIndex.value,

​      elevation: 10,

​      onTap: (index) {

​        controller.handleBottomNavigationIndexSelected(index);

​      },

​      type: BottomNavigationBarType.fixed,

​      iconSize: 25.sp,

​      selectedLabelStyle:

​          TextStyle(fontSize: 12.sp, fontWeight: FontWeight.bold),

​      unselectedLabelStyle:

​          TextStyle(fontSize: 12.sp, fontWeight: FontWeight.normal),

​      items: iconsMap.keys

​          .map((key) => BottomNavigationBarItem(

​                icon: Icon(iconsMap[key]),

​                label: key,

​              ))

​          .toList(),

​    );

  }

}


```

### Binding代码

```dart
import 'package:get/get.dart';

import './controller.dart';



import '../home/controller.dart';

import '../right/controller.dart';

import '../me/controller.dart';



class BottomNavBinding implements Bindings {

  @override

  void dependencies() {

​    Get.lazyPut(() => BottomNavController());



​    Get.lazyPut(() => HomeController());

​    Get.lazyPut(() => RightController());

​    Get.lazyPut(() => MeController());

  }

}
```

