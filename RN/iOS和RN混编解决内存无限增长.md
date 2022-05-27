```

Bridge单例




#import <Foundation/Foundation.h>

\#import <React/RCTBridge.h>

NS_ASSUME_NONNULL_BEGIN

@interface GXBridgeQDJobManger : RCTBridge

/**

 GXBridgeQDJobManger单例

*/

\+ (instancetype)sharedManager;



@end

NS_ASSUME_NONNULL_END
```

```
#import "GXBridgeQDJobManger.h"

\#import <React/RCTBundleURLProvider.h>

\#import <CodePush/CodePush.h>

//dev模式下:RCTBridge required dispatch_sync to load RCTDevLoadingView Error Fix

\#if RCT_DEV

\#import <React/RCTDevLoadingView.h>

\#endif

/**

RCTBridgeDelegate

*/

@interface BridgeHandle : NSObject<RCTBridgeDelegate>

@end



@implementation BridgeHandle

\- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge{

  NSURL *jsCodeLocation = [CodePush bundleURLForResource: @"qdjob"];

  return jsCodeLocation;

}



@end



@implementation GXBridgeQDJobManger



\+ (instancetype)sharedManager {

  static GXBridgeQDJobManger *manager;

  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{

​     manager = [[GXBridgeQDJobManger alloc] initWithDelegate:[[BridgeHandle alloc] init] launchOptions:nil];

//#if RCT_DEV

//    [manager moduleForClass:[RCTDevLoadingView class]];

//#endif

​     

});

   

  return manager;

}



@end
```



#### iOS 进入原生逻辑

RootView持有单例的Bridge

```objective-c
  GXBaseRNViewController *navtiveVC;

  if ([bundleName isEqualToString:@"main"]) {

​    RCTRootView *rootView = [[RCTRootView alloc]initWithBridge: [GXBridgeMainManger sharedManager] moduleName:moduleName initialProperties:userInfoDic];

​    navtiveVC = [[GXBaseRNViewController alloc]initWithNavType:GXNavTypeNone];

​    navtiveVC.view = rootView;

  } else { //qdjob

​    RCTRootView *rootView = [[RCTRootView alloc]initWithBridge: [GXBridgeQDJobManger sharedManager] moduleName:moduleName initialProperties:userInfoDic];

​    navtiveVC = [[GXBaseRNViewController alloc]initWithNavType:GXNavTypeNone];

​    navtiveVC.view = rootView;

​    navtiveVC.popGestureRecognizerDisable = YES;

  }
```

