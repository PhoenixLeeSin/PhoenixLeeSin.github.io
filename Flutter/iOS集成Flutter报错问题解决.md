**iOS集成Flutter报错 Target debug_ios_bundle_flutter_assets failed 问题解决**

 **报错信息**

```
`Target debug_ios_bundle_flutter_assets failed: FileSystemException: Cannot open file, path = '/Users/liguicheng/Desktop/flutter练习项目/ios集成flutter/test_flutter/ios/Flutter/AppFrameworkInfo.plist' (OS Error: No such file or directory, errno = 2)           #0      _File.throwIfError (dart:io/file_impl.dart:635:7)           #1      _File.openSync (dart:io/file_impl.dart:479:5)           #2      ForwardingFile.openSync (package:file/src/forwarding/forwarding_file.dart:71:16)           #3      ErrorHandlingFile.copySync. (package:flutter_tools/src/base/error_handling_io.dart:308:22)           #4      _runSync (package:flutter_tools/src/base/error_handling_io.dart:573:14)           #5      ErrorHandlingFile.copySync (package:flutter_tools/src/base/error_handling_io.dart:307:5)           #6      IosAssetBundle.build (package:flutter_tools/src/build_system/targets/ios.dart:510:8)                      #7      _BuildInstance._invokeInternal (package:flutter_tools/src/build_system/build_system.dart:828:9)                      #8      FlutterBuildSystem.build (package:flutter_tools/src/build_system/build_system.dart:595:16)                      #9      AssembleCommand.runCommand (package:flutter_tools/src/commands/assemble.dart:318:32)                      #10     FlutterCommand.run. (package:flutter_tools/src/runner/flutter_command.dart:1043:27)                      #11     AppContext.run. (package:flutter_tools/src/base/context.dart:150:19)                      #12     CommandRunner.runCommand (package:args/command_runner.dart:196:13)                      #13     FlutterCommandRunner.runCommand. (package:flutter_tools/src/runner/flutter_command_runner.dart:284:9)                      #14     AppContext.run. (package:flutter_tools/src/base/context.dart:150:19)                      #15     FlutterCommandRunner.runCommand (package:flutter_tools/src/runner/flutter_command_runner.dart:232:5)                      #16     run.. (package:flutter_tools/runner.dart:62:9)                      #17     AppContext.run. (package:flutter_tools/src/base/context.dart:150:19)                      #18     main (package:flutter_tools/executable.dart:91:3)`       
```

**错误原因**

>  因为在Flutter使用了特殊字体，需要在iOS工程下的info.plist下添加相关配置。
>
> eg:
>
>  在工程里面添加字体文件 xxx.otf,yyy.otf,然后在Info.plist添加字段"Fonts provided by application"，并且添加对应的字体，如图:

![](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/M1YnPX.jpg)