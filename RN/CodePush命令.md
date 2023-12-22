# CodePush命令











```java
npm install -g code-push-cli：安装工具

yarn add react-native-code-push
 或者
npm install --save react-native-code-push
集成到项目中

react-native link react-native-code-push：连接到应用
code-push register：注册
code-push login ：登陆
code-push login 自定义服务器网址 ：登陆
code-push logout： 注销
code-push access-key ls 列出登陆的token
code-push access-key rm <accessKye> 删除某个 access-key
```



如果登录过程中可能会出现问题如下问题：



```csharp
在终端输入：code-push login，
如果出现[Error] You are already logged in from this machine.
在终端试着输入命令：code push logout，
如果出现[Error] connect ECONNREFUSED 127.0.0.1:3000错误，
可以直接删除 ~/.code-push.config文件
```



.code-push.config 文件默认为隐藏文件，可通过如下命令设置显示

```css
1、
（设置隐藏文件可见）
defaults write com.apple.finder AppleShowAllFiles TRUE
（设置隐藏文件不可见）
defaults write com.apple.finder AppleShowAllFiles FALSE
2、
（终端执行命令重启Finder）
killall Finder
```



### code-push app 相关命令

```csharp
code-push app add MyApp ios react-native：添加 ios 应用
code-push app add MyApp android cordova：添加安卓应用
code-push app add MyApp windows react-native：添加 windows应用
code-push app remove MyApp：移除应用
code-push app rename oldName newName：重命名一个存在app
code-push app list ：或则 ls 列出账号下面的所有app
```



### 部署APP相关命令

```xml
code-push deployment add <appName> [deploymentName]
code-push deployment add <appName> -d：部署--default, -d  Add the default "Staging" and "Production" deployments
code-push deployment rename <appName>： 重命名
code-push deployment rm <appName>  <deploymentName> ：删除部署
code-push deployment ls <appName> ：列出应用的部署情况
code-push deployment ls <appName> -k： 查看部署的key
code-push deployment history <appName> <deploymentNmae> ：查看历史版本(Production 或者 Staging)
code-push rollback <appName> <deploymentNmae> --targetRelease <v> ：回退版本
```

### 打包

```java
进入工程根目录：
react-native bundle --platform ios --dev false --entry-file index.js --bundle-output ./release/ios/main.jsbundle --assets-dest release/ios

需要现在根目录下添加 release/ios 目录

参数说明：

--entry-file 指定入口文件 因为要打包ios平台，所以指定为rn项目的index.ios.js作为入口

--bundle-output 指定输出的jsbundle文件路径和文件名 指定到rn项目的ios工程文件夹下，记得一定要先创建bundle文件夹，不然终端会报文件夹找不到的错误

--platform 指定平台类型

--assets-dest 指定资源文件夹路径 assets文件夹的路径，包含图片、node模块等资源

--dev 是否为开发模式 如果设置为false，不会产生警告，并且bundle会被压缩
```



### 发布



```undefined
code-push release FirstApp-ios ./release/ios 版本号 -d Production
默认不写 -d 表示 Staging 环境
```



### 其他

```
// 发布
code-push release-react <appName> <platform> -t 版本  -d 环境  --des 描述 -m true （强制更新）
// 清除历史部署记录
code-push deployment clear <appName> Production or Staging
// 回滚
code-push rollback <appName> Production --targetRelease v1(codepush服务部署的版本号)

code-push release-react android_rngxt android -t 1.0.0 -d Production 



```



# 国信通 打包 

#### 国信通 清理掉测试以及生产环境下发布的更新包

```
code-push deployment clear android_rngxt Staging
code-push deployment clear android_rngxt Production
```



#### 查看历史记录 [测试 / 生产]

```
code-push deployment h android_rngxt Staging
code-push deployment h android_rngxt Production

code-push deployment h ios_rngxt Staging
code-push deployment h ios_rngxt Production
```



#### 打iOS bundle包 [测试 / 生产]

```
react-native bundle --platform ios --dev true --entry-file index.js --bundle-output ./release/ios/main.jsbundle --assets-dest release/ios   

react-native bundle --platform ios --dev false --entry-file index.js --bundle-output ./release/ios/main.jsbundle --assets-dest release/ios   
```

   

#### 打android bundle包 [测试 / 生产]

```
react-native bundle --entry-file index.js --platform android --dev true --bundle-output ./android/app/src/main/assets/index.android.bundle --assets-dest ./android/app/src/main/res/

react-native bundle --entry-file index.js --platform android --dev false --bundle-output ./android/app/src/main/assets/index.android.bundle --assets-dest ./android/app/src/main/res/
```

#### 发布 

```
code-push release-react ios_rngxt ios -t 1.0.0 -d Production 
```



#### WebStorm连接安卓真机命令

```
 react-native run-android --variant=DevDebug
```



#### 查看更新的详细信息 [测试 / 生产]

```
code-push deployment h android_rngxt Staging --format=json
code-push deployment h android_rngxt Production --format=json
```



#### 获取应用下的key和更新信息等

```
code-push deployment ls android_rngxt -k
code-push deployment ls ios_rngxt -k 
```

####                          





```react
package com.rngxtt;

import com.microsoft.codepush.react.CodePush;
import android.app.Application;
import android.content.Context;
import com.facebook.react.PackageList;
import com.facebook.react.ReactApplication;
import com.facebook.react.ReactInstanceManager;
import com.facebook.react.ReactNativeHost;
import com.facebook.react.ReactPackage;
import com.facebook.soloader.SoLoader;
import java.lang.reflect.InvocationTargetException;
import java.util.List;
import com.microsoft.codepush.react.CodePush;

public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost =
      new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
          return BuildConfig.DEBUG;
        }

        @Override
        protected List<ReactPackage> getPackages() {
          @SuppressWarnings("UnnecessaryLocalVariable")
          List<ReactPackage> packages = new PackageList(this).getPackages();
          // Packages that cannot be autolinked yet can be added manually here, for example:
          // packages.add(new MyReactNativePackage());

        // String deploymentKey, Context context, boolean isDebugMode, String serverUrl
            /*
                deploymentKey 为当前环境(测试环境/生产环境)的热更新key,
              serverUrl 为热更新私有服务器地址  如:http://45.40.193.123:3000
            */
          // 加入这个代码
          packages.add(new CodePush(BuildConfig.CODEPUSH_KEY,MainApplication.this,BuildConfig.DEBUG, BuildConfig.CODEPUSH_URL));
          return packages;
        }

          // 2. Override the getJSBundleFile method in order to let
          // the CodePush runtime determine where to get the JS
          // bundle location from on each app start
        @Override
        protected String getJSMainModuleName() {
          return "index";
        }

          @Override
          protected String getJSBundleFile() {
              return CodePush.getJSBundleFile();
          }
      };

  @Override
  public ReactNativeHost getReactNativeHost() {
    return mReactNativeHost;
  }

  @Override
  public void onCreate() {
    super.onCreate();
    SoLoader.init(this, /* native exopackage */ false);
    initializeFlipper(this, getReactNativeHost().getReactInstanceManager());
  }

  /**
   * Loads Flipper in React Native templates. Call this in the onCreate method with something like
   * initializeFlipper(this, getReactNativeHost().getReactInstanceManager());
   *
   * @param context
   * @param reactInstanceManager
   */
  private static void initializeFlipper(
      Context context, ReactInstanceManager reactInstanceManager) {
    if (BuildConfig.DEBUG) {
      try {
        /*
         We use reflection here to pick up the class that initializes Flipper,
        since Flipper library is not available in release mode
        */
        Class<?> aClass = Class.forName("com.rngxtt.ReactNativeFlipper");
        aClass
            .getMethod("initializeFlipper", Context.class, ReactInstanceManager.class)
            .invoke(null, context, reactInstanceManager);
      } catch (ClassNotFoundException e) {
        e.printStackTrace();
      } catch (NoSuchMethodException e) {
        e.printStackTrace();
      } catch (IllegalAccessException e) {
        e.printStackTrace();
      } catch (InvocationTargetException e) {
        e.printStackTrace();
      }
    }
  }
}

```

商户-运营人员,商户-自提人员,商户-积分人员,商户-客服人员,商户-积分调整审核员,权益app
