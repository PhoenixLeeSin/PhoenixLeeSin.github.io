# iOS工程下集成Flutter 两者之间的通信（BasicMessageChannel）



## 1. 通过BasicMessageChannel实现iOS和Flutter互相通信

> 这里要说明一下，实现iOS和Flutter之间的相互通信是指： iOS主动向Flutter端发送消息并接收到Flutter的回复消息；Flutter端主动向iOS发送消息并接收到iOS端的回复消息。

![](https://cdn.jsdelivr.net/gh/PhoenixLeeSin/LeeImages@master/uPic/r71i3k.png)

> ##### 为此我们实现一个这种demo: 模拟器的上半部分为iOS原生页面， 下半部分为Flutter页面，我们通过BasicMessageChannel来实现iOS和Flutter的相互通信。

### 前方高能 前方高能  前方高能  代码部分

### iOS部分

> ##### Appdelegate.swift 主要实现插件的注册，channel实例化等操作，注意 这个项目的结构为iOS原生项目集成Flutter！这个项目的结构为iOS原生项目集成Flutter！这个项目的结构为iOS原生项目集成Flutter！ ViewController.swift实现basicmessageChannel接收Flutter发送的消息并回复以及发送消息到Flutter并等待Flutter的回复逻辑。
>
> ├── demo_flutter
> │   ├── README.md
> │   ├── assets
> │   ├── demo_flutter.iml
> │   ├── demo_flutter_android.iml
> │   ├── lib
> │   ├── pubspec.lock
> │   ├── pubspec.yaml
> │   └── test
> └── iOSFlutter
>     ├── Podfile.lock
>     ├── Pods
>     ├── iOSFlutter
>     ├── iOSFlutter.xcodeproj
>     ├── iOSFlutter.xcworkspace
>     └── podfile

#### AppDelegate.swift

```swift
import UIKit
import Flutter
import FlutterPluginRegistrant

@main

class AppDelegate: UIResponder, UIApplicationDelegate{

  let basicMessageChannelName = "com.basicMessageChannel.iOSFlutter"
  var window: UIWindow?
  var flutterBasicMessageChannel: FlutterBasicMessageChannel? // flutter <-> iOS
  var flutterEngine: FlutterEngine?
  var flutterViewController: FlutterViewController?

  func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
​    self.window = UIWindow.init(frame: UIScreen.main.bounds)
​    self.window?.backgroundColor = .white
​    let nav = UINavigationController.init(rootViewController: ViewController())
​    self.window?.rootViewController = nav
​    self.window?.makeKeyAndVisible()

​    flutterEngine = FlutterEngine(name: "my flutter engine")

​    if let flutterEngine = flutterEngine {
​      flutterEngine.run()
​      GeneratedPluginRegistrant.register(with: flutterEngine ) // 注意这两者的顺序应该是在前面的
​      flutterViewController = FlutterViewController(engine: flutterEngine, nibName: nil, bundle: nil)
​      flutterBasicMessageChannel = FlutterBasicMessageChannel.init(name: basicMessageChannelName, binaryMessenger: flutterViewController!.binaryMessenger)

​    }
​    return true
  }
}
```

#### ViewController.swift

```swift
import UIKit

import Flutter

let messageDic = [

  "code": NSNumber.init(value: 200),

  "message": "iOS向Flutter传递消息"

] as [String : Any]


class ViewController: UIViewController {

  var eventSink: FlutterEventSink?
  var label: UILabel = UILabel()

  override func viewDidLoad() {
​    super.viewDidLoad()
​    self.view.backgroundColor = .cyan

    ///FlutterViewController页面添加到VC上
​    if let flutterViewController = (UIApplication.shared.delegate as? AppDelegate)?.flutterViewController {
​      flutterViewController.view.frame = CGRect(x: 0, y: UIScreen.main.bounds.height/2, width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height/2)
​      self.addChild(flutterViewController)
​      self.view.addSubview(flutterViewController.view)
​    }

​   ///显示消息的UI
​    label = UILabel.init(frame: CGRect(x: 80.0, y: 80.0, width: 300, height: 140))
​    label.font = .systemFont(ofSize: 12)
​    label.numberOfLines = 0
​    label.backgroundColor = .white
​    label.textColor = .red
​    self.view.addSubview(label)
​     
     ///Btn
​    let button = UIButton(type:UIButton.ButtonType.custom)
​    button.addTarget(self, action: #selector(click), for: .touchUpInside)
​    button.setTitle("通过MessageChannel向Flutter传递消息", for: UIControl.State.normal)
​    button.titleLabel?.font = .systemFont(ofSize: 13)
​    button.frame = CGRect(x: 80.0, y: 300.0, width: 300.0, height: 40.0)
​    button.backgroundColor = UIColor.blue
​    self.view.addSubview(button)
    
​    ///监听flutter传递的信息并回复
​    /** 接受的格式
​     {"method": "close", "content": "传递flutter数据给iOS", "code": 200}
​     */
​    if let basicMessageChannel = (UIApplication.shared.delegate as? AppDelegate)?.flutterBasicMessageChannel {
​      basicMessageChannel.setMessageHandler { message, callBack in
​        if let msg = message as? NSDictionary, let method = msg["method"] as? String {
​          if (method == "close") {
​            let messageNewDic = [
​              "code": NSNumber.init(value: 200),
​              "message": "iOS接收到Flutter传递消息并回复"
​            ] as [String : Any]
​            self.label.text = self.dicValueString(msg as! [String : Any])
​            callBack(messageNewDic)
​          }
​        }
​      }
​    }
  }

  @objc func click() {
​    if let basicMessageChannel = (UIApplication.shared.delegate as? AppDelegate)?.flutterBasicMessageChannel {
​      basicMessageChannel.sendMessage(messageDic) { reply in
​        if let msg = reply as? NSDictionary {
​          self.label.text = self.dicValueString(msg as! [String : Any])
​        }
​      }
​    }
  }

///字典转String
  func dicValueString(_ dic:[String : Any]) -> String?{
​     let data = try? JSONSerialization.data(withJSONObject: dic, options: [])
​     let str = String(data: data!, encoding: String.Encoding.utf8)
​     return str
   }

}
```

#### 总结：

#### 1.iOS向Flutter发送消息并接收Flutter的回复时，\- (void)sendMessage:(id _Nullable)message reply:(FlutterReply _Nullable)callback;  callBack就是Flutter端的回复信息（只需要统一内容结构即可）；

#### 2.iOS接收Flutter的信息并给与回复是API比较好动，带FlutterReply的回调，具体参考上面的代码。



### Flutter部分

> 和iOS端发过来就可以了，在需要的页面实现basicMessageChannel的实例化以及收发动作，需要注意就是接收到iOS发来的信息并回复的部分。这里就只贴controller的代码了 

#### controller.dart

```dart
class SignInController extends GetxController {

  final state = SignInState();

  final basicMessageChannel =

​      BasicMessageChannel(BASIC_MESSAGE_CHANNEL, StandardMessageCodec());

  @override

  void onInit() {
​    *super*.onInit();

​    *///接收iOS发来的消息*

​    basicMessageChannel.setMessageHandler((result) async {
​      if (result is Map) {
​        int code = result["code"];
​        String message = result['message'];
​        state.text.value = "接收到消息：code:$code message: $message";

​        return {
​          "method": "close",
​          "content": "Flutter接收到iOS传递的信息并回复",
​          "code": 200
​        };
​      }
​    });
  }


  void sendBasicMessageAndReply() async {

​    Object? reply = await basicMessageChannel
​        .send({"method": "close", "content": "传递flutter数据给iOS", "code": 200});
​    if (reply is Map) {
​      int code = reply["code"];
​      String message = reply["message"];
​      state.text.value = "接收到消息：code:$code message: $message";
​    }
  }

}


```



#### 总结：

#### 1.iOS向Flutter发送消息并接收Flutter的回复时，在.setMessageHandler中执行完逻辑直接返回相应的数据即可；

