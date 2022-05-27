# Chimera Flutter Code Push配置步骤

## 1. 下载相关工具以及配置



###  1.1 安装http-server 

```js
npm install http-server -g
```

#### 如果报找不到文件的错误，请用管理员权限运行

```
sudo npm install http-server -g
```

###  

### 1.2 下载 rust_compile

![image-20211104143534178](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104143534178.png)

#### 下载完成之后，桌面新建文件夹（命名随意 这里叫tool ）把rust_compile放置其中，并通过终端给与权限

![image-20211104143834502](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104143834502.png)

```
chmod 777 rust_compile
```

####  在cd 到该文件夹下执行./rust_compile命令来生成config.yaml文件

```
./rust_compile
```

![image-20211104144314644](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104144314644.png)

###  

### 1.3 新建保存dart_compile_cache文件夹

![image-20211104145119492](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104145119492.png)



## 2. 创建工程并配置



###  2.1 桌面创建test文件夹并新建名为hello的工程。创建完成之后一定要使用IDE调试一次次项目，不管是使用模拟器还是真机。创建完成之后一定要使用IDE调试一次次项目，不管是使用模拟器还是真机。创建完成之后一定要使用IDE调试一次次项目，不管是使用模拟器还是真机。

```
flutter create hello
```



### 2.2  配置 tool文件夹下的config.yaml

```
environment:

### 文件路径
  projectPath: /Users/liguicheng/Desktop/fluttertest/hello

### 修改当前flutter环境的SDK目录
  flutterSdkPath: /Users/liguicheng/Desktop/FlutterDev/flutter3

  
#设置一个专门目录保存dart_compile_cache
  buildCachePath: /Users/liguicheng/Desktop/dart_compile_cache


  *# FTP*

  ftpConfig: 192.168.80.144|ftpuser|ftpuser
```



###  2.3  cd到rust_compile所在的文件夹下执行命令

![image-20211104150057517](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104150057517.png)

```
 ./rust_compile
```

#### 执行完成之后出现此界面既是正常

![image-20211104150257826](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104150257826.png)



### 2.4 修改hello工程下wtbase下的pubspec.yaml文件

####  配置`wtbase/pubspec.yaml`在`dependencies` 修改flutter_code_push的引导路径为以下：

```js
flutter_code_push_next:
    git:
      url: https://github.com/Waytoon/chimera_flutter_code_push.git
      path: flutter_code_push_next
```

#### 修改完成之后 终端执行 flutter pub get 命令

```
flutter pub get 

```

![image-20211104150724188](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104150724188.png)



### 2.5  修改主工程hello下 pubspec.yaml文件

1. 修改`hello/pubspec.yaml`，在`dependencies`添加如下：

```
wtbase: 
  path: ./wtbase
```

![image-20211104151452475](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104151452475.png)

#### 修改完成之后 终端执行 flutter pub get 命令

```
flutter pub get 
```



### 2.6 拆分main.dart文件

#### 包含main函数的代码视为入口函数，不进行JIT执行，所以需要进行代码拆分。具体代码改为类似如下

> #### 具体步骤复制main.dart代码， 新建myapp.dart文件，并将main.dart内容复制到里面。其中入口函数main()修改为run()

![image-20211104152037573](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104152037573.png)

#### 复制一下代码粘贴至main.dart

```dart
import 'package:hello/myapp.dart';
import 'package:wtbase/wtbase.dart';

Future main() async {
  bool isNative = false;
  if (isNative) {
    run();
  } else {
    String downloadUrl = "http://192.168.80.144:8080/hello.bin";
    readCode = WTAnalysisReadCode();
    await readCode.initDownloadFilePath();
    await readCode.downloadPathAndReadFile(downloadUrl);
    readCode.executeMethod('package:hello/myapp.dart', 'run');
  }
```



### 2.7 第一次运行的报错解决

#### 此时wtbase工程下文件夹generate会报错（第一次），此时在此文件夹下新建mapping文件夹

![image-20211104150950233](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104150950233.png)

#### **然后在tool文件夹下再次执行 ./rust_compile命令** ，执行完成之后如果mapping文件夹下仍然无文件可以重启一下AS。



### 2.8 test文件夹报错处理

####     暂时注释掉即可



## 3. 执行热更新步骤

###  3.1 启动http-server

```js
///在终端下 cd到工程下的assets目录下，执行 
http-server -c -1
```

![image-20211104153702380](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104153702380.png)

复制`http-server`下面的链接，我的是`http://192.168.80.115:8080`,用来修改`main.dart`里的

```dart
            String downloadUrl = "http://yourlocalhost/hello.bin";
复制代码
```

修改成：

```dart
            String downloadUrl = "http://192.168.1.144:8082/hello.bin";
```

tool文件夹下的config.yaml的ftpConfig 修改为上面的IP地址

```js
 ftpConfig: 192.168.1.144|ftpuser|ftpuser
```



### 3.2  从IDE运行一下你的APP，方法就是点击那个绿虫子



### 3.3 执行热更新

####  任意修改代码，如把`Icons.add`修改为`Icons.home`，保存你所做的修改，然后回到terminal，进入你的编译器目录，运行`./rust_compile`

![image-20211104162551012](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104162551012.png)



#### **这里亲请注意了**，我们到此不需要IDE来做什么了，只需要在模拟器里关掉当前运行的app，重新打开它，你就会发现它变成了下面的样子：

![image-20211104162826640](/Users/liguicheng/Library/Application Support/typora-user-images/image-20211104162826640.png)