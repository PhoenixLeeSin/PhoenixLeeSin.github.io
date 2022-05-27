# iOS工程下集成Flutter 两者之间的通信

![ Platform Channel 架构图](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/IAM5Zj.jpg)



## 1. Flutter向iOS端发送信息

### iOS端代码处理

```swift
import UIKit

import Flutter
import FlutterPluginRegistrant


@main
class AppDelegate: UIResponder, UIApplicationDelegate{

    let methodChannelName = "com.methodChannelName.iOSFlutter"
    let eventChannelName = "com.eventChannelName.iOSFlutter"
    var window: UIWindow?
    var flutterMethodChannel: FlutterMethodChannel? //flutter -> iOS
    var flutterEventChannel: FlutterEventChannel? // iOS -> flutter
    var flutterEngine: FlutterEngine?
    var flutterViewController: FlutterViewController?
    

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        self.window = UIWindow.init(frame: UIScreen.main.bounds)
        self.window?.backgroundColor = .white
        let nav = UINavigationController.init(rootViewController: ViewController())
        self.window?.rootViewController = nav
        self.window?.makeKeyAndVisible()
        
        flutterEngine = FlutterEngine(name: "my flutter engine")

        if let flutterEngine = flutterEngine {
            flutterEngine.run()
            GeneratedPluginRegistrant.register(with: flutterEngine )
            
            flutterViewController =
                FlutterViewController(engine: flutterEngine, nibName: nil, bundle: nil)
            flutterMethodChannel = FlutterMethodChannel.init(name: channelName, binaryMessenger: flutterViewController!.binaryMessenger)
            ///
            flutterEventChannel = FlutterEventChannel(name: eventChannelName, binaryMessenger: flutterViewController!.binaryMessenger)
            ///
            flutterMethodChannel?.setMethodCallHandler({ call, result in
                if(call.method == "aaa") {
                    if let message: String = call.arguments as? String {
                        print("接收到flutter信息" + message)
                    }
                }
            })
        }
        return true
    }
    
}

```



### Flutter端

```dart
  void handleItemTap(int index) {

​    ForbidCancelReason reason = state.logoutList[index];

​    const methodChannel = const MethodChannel('com.iOSFlutter');

​    methodChannel.invokeMethod('aaa', reason.opNameDesc);

  }
```



1. ## iOS向Flutter端发送信息

### iOS端代码

```swift
import UIKit



import Flutter

import FlutterPluginRegistrant





@main

class AppDelegate: UIResponder, UIApplicationDelegate{



  let methodChannelName = "com.methodChannelName.iOSFlutter"

  let eventChannelName = "com.eventChannelName.iOSFlutter"

  var window: UIWindow?

  var flutterMethodChannel: FlutterMethodChannel? //flutter -> iOS

  var flutterEventChannel: FlutterEventChannel? // iOS -> flutter

  var flutterEngine: FlutterEngine?

  var flutterViewController: FlutterViewController?

   



  func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {



​    self.window = UIWindow.init(frame: UIScreen.main.bounds)

​    self.window?.backgroundColor = .white

​    let nav = UINavigationController.init(rootViewController: ViewController())

​    self.window?.rootViewController = nav

​    self.window?.makeKeyAndVisible()

​     

​    flutterEngine = FlutterEngine(name: "my flutter engine")



​    if let flutterEngine = flutterEngine {

​      flutterEngine.run()

​      GeneratedPluginRegistrant.register(with: flutterEngine )

​       

​      flutterViewController =

​        FlutterViewController(engine: flutterEngine, nibName: nil, bundle: nil)

​      flutterMethodChannel = FlutterMethodChannel.init(name: channelName, binaryMessenger: flutterViewController!.binaryMessenger)

​      ///

​      flutterEventChannel = FlutterEventChannel(name: eventChannelName, binaryMessenger: flutterViewController!.binaryMessenger)

​      ///

​      flutterMethodChannel?.setMethodCallHandler({ call, result in

​        if(call.method == "aaa") {

​          if let message: String = call.arguments as? String {

​            print("接收到flutter信息" + message)

​          }

​        }

​      })

​    }

​    return true

  }

   

}
```

### iOS端具体发消息业务端

```swift
import UIKit

import Flutter



class ViewController: UIViewController, FlutterStreamHandler {



  var eventSink: FlutterEventSink?

   

  override func viewDidLoad() {

​    super.viewDidLoad()

​    self.view.backgroundColor = .cyan

​     

​    let button = UIButton(type:UIButton.ButtonType.custom)

​    button.addTarget(self, action: #selector(showFlutter), for: .touchUpInside)

​    button.setTitle("Show Flutter!", for: UIControl.State.normal)

​    button.frame = CGRect(x: 80.0, y: 210.0, width: 160.0, height: 40.0)

​    button.backgroundColor = UIColor.blue

​    self.view.addSubview(button)

​     

​    if let eventChannel = (UIApplication.shared.delegate as! AppDelegate).flutterEventChannel {

​      eventChannel.setStreamHandler(self)

​    }

  }

   

  @objc func showFlutter() {

​    if let flutterViewController = (UIApplication.shared.delegate as! AppDelegate).flutterViewController {

​       

​      flutterViewController.modalPresentationStyle = .fullScreen

​      present(flutterViewController, animated: true, completion: nil)

​    }

  }



  //FlutterStreamHandler 具体发消息好像必须在其代理方法中 还需要进一步研究
 
  func onListen(withArguments arguments: Any?, eventSink events: @escaping FlutterEventSink) -> FlutterError? {

​    self.eventSink = events

​    self.eventSink!("abc")

​    return nil

  }

   

  func onCancel(withArguments arguments: Any?) -> FlutterError? {

​    return nil

  }



}
```



### Flutter端

```dart
class SignInController extends GetxController {

  final state = SignInState();



  final TextEditingController phoneController = TextEditingController();

  final TextEditingController passwordController = TextEditingController();



  *///监听*

  final eventChannel = EventChannel("com.eventChannelName.iOSFlutter");



  @override

  void onInit() {

​    *super*.onInit();

​    eventChannel.receiveBroadcastStream().listen((event) {

​      print("BBBBB");

​      print(event);

​    });

  }
}
```

