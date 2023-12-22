# 创建一个新的react-native项目



##### 1 创建命令

```
react-native init appName  --version 0.64.2
```

##### 2. cd 到ios目录下 执行 pod install

```
 cd ios
 pod install
```

##### 3. 运行真机运行 报错 Unable to load script.Make sure you are either running a Metro server or that your bundle 'index.android.bundle' is packaged correctly for release.

需要先执行  至此项目运行

```
npm start 或者 react-native start
在执行 react-native run-android
```

##### 4.其他库的安装  react-navigation 4.4.4

[一定需要参照官网指引操作](https://reactnavigation.org/docs/4.x/getting-started/)



###### Failed to install the app. Make sure you have the Android development environment set up: https://reactnative.dev/docs/environment-setup.Error: Command failed: ./gradlew app:installDebug -PreactNativeDevServerPort=8081





###### Caused by: java.io.IOException: Cannot run program "node" (in directory ...

```
**Run this** `open -a /Applications/Android\ Studio.app`
Then sync Gradle
```



###### A problem occurred evaluating project ':react-native-reanimated'.Cannot get property 'supportLibVersion' on extra properties extension as it does not exist

修改工程下 `android/build.gradle`:

```
buildscript {
    ext {
        buildToolsVersion = "25.0.2"
        minSdkVersion = 21
        compileSdkVersion = 30
        targetSdkVersion = 30
        ndkVersion = "20.1.5948944"
        supportLibVersion = "1.3.0"
    }
    ....
```



##### 5. 安装 react-navigation-stack

[官网指引](https://reactnavigation.org/docs/4.x/hello-react-navigation)



###### 6. Error: Unable to resolve module base64-js from /Users/liguicheng/Desktop/smartCommunity/node_modules/react-native/Libraries/Utilities/binaryToBase64.js: base64-js could not be found within the project or in these directories:



###### 7. Error: Unable to resolve module @react-native-community/async-storage from /Users/liguicheng/Desktop/smartCommunity/node_modules/@bang88/react-native-ultimate-listview/src/refreshableScrollView.android.js: @react-native-community/async-storage could not be found within the project or in these directories:

```
import AsyncStorage from '@react-native-async-storage/async-storage';
```



fbjs/lib/invariant could not be found within the project or in these directories