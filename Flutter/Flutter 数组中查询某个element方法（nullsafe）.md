

# Flutter 数组中查询某个element方法（nullsafe）

> #### [`package:collection`](https://pub.dev/packages/collection) also contains a convenience extension method for the `null` case (which should also work better with null safety):



```dart
import 'package:collection/collection.dart';

list.firstWhereOrNull((element) => element == other);
```



示例

```dart
 *// 生活或购物*
get shopModule =>

​      _moduleList.value?.firstWhereOrNull((module) => module.contentType == 2);
```

