# RN常见错误问题处理

1. > ##### Build failed after update Xcode 12.5 beta Cannot initialize a parameter of type 'NSArray<id<RCTBridgeModule>> *' with an rvalue of type 'NSArray<Class> *'

解决方案：

RCTxxBridge.mm文件 line 746

替换

```
- (NSArray<RCTModuleData *> *)_initializeModules:(NSArray<id<RCTBridgeModule>> *)modules
```

为 :

```
- (NSArray<RCTModuleData *> *)_initializeModules:(NSArray<id> *)modules
```



2. #### **Connection to http://192.168.1.144:8081/debugger-proxy?role=client timed out. Are you running node proxy? If you are running on the device, check if you have the right IP address in `RCTWebSocketExecutor.m`.**

解决方案：

查看你的手机设备网络是否和电脑一致，同时可以设置 RCTSRWebSocket.m 文件下的host地址

```objective-c


\- (void)setUp

{

 if (!_url) {

  NSInteger port = [[[_bridge bundleURL] port] integerValue] ?: RCT_METRO_PORT;

  NSString *host = [[_bridge bundleURL] host] ?: @"localhost";

   //// 直接设置为电脑对应的IP地址
//  NSString *host = @"192.168.1.144";

  NSString *URLString = [NSString stringWithFormat:@"http://%@:%lld/debugger-proxy?role=client", host, (long long)port];

  _url = [RCTConvert NSURL:URLString];

 }



 _jsQueue = dispatch_queue_create("com.facebook.react.WebSocketExecutor", DISPATCH_QUEUE_SERIAL);

 _socket = [[RCTSRWebSocket alloc] initWithURL:_url];

 _socket.delegate = self;

 _callbacks = [NSMutableDictionary new];

 _injectedObjects = [NSMutableDictionary new];

 [_socket setDelegateDispatchQueue:_jsQueue];



 NSURL *startDevToolsURL = [NSURL URLWithString:@"/launch-js-devtools" relativeToURL:_url];



 NSURLSession *session = [NSURLSession sharedSession];

 NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:[NSURLRequest requestWithURL:startDevToolsURL]

​                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error){}];

 [dataTask resume];

 if (![self connectToProxy]) {

  [self invalidate];

  NSString *error = [NSString stringWithFormat:@"Connection to %@ timed out. Are you "

​            "running node proxy? If you are running on the device, check if "

​            "you have the right IP address in `RCTWebSocketExecutor.m`.", _url];

  _setupError = RCTErrorWithMessage(error);

  RCTFatal(_setupError);

  return;

 }
```

