> https://www.jianshu.com/p/6d6573fbd60b

Xcode 11 建新工程默认会创建通过 UIScene 管理多个 UIWindow 的应用，工程中除了 AppDelegate 外还会有一个 SceneDelegate，这是为了实现iPadOS支持多窗口的结果。AppDelegate.h不再有window属性，window属性被定义在了SceneDelegate.h中，AppDelegate中有新增的关于scene的代理方法，SceneDelegate中也有相应的代理方法。因此，当我们用Xcode11针对不同版本的iOS开发应用时，就要做一些适配。

创建好一个工程后，可以看到相关Xcode开发界面变成了下面这样：
![image](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/xuKEZ1.jpg)
![image](https://github.com/PhoenixLeeSin/LeeImages/blob/master/uPic/Thj6eH.jpg)

工程配置general
info.plist文件
AppDelegate.h中多了两个默认的代理方法：


```
#pragma mark - UISceneSession lifecycle

/*
1.如果没有在APP的Info.plist文件中包含scene的配置数据，或者要动态更改场景配置数据，需要实现此方法。 UIKit会在创建新scene前调用此方法。
2.方法会返回一个UISceneConfiguration对象，其包含其中包含场景详细信息，包括要创建的场景类型，用于管理场景的委托对象以及包含要显示的初始视图控制器的情节提要。 如果未实现此方法，则必须在应用程序的Info.plist文件中提供场景配置数据。

总结下：默认在info.plist中进行了配置， 不用实现该方法也没有关系。如果没有配置就需要实现这个方法并返回一个UISceneConfiguration对象。
配置参数中Application Session Role 是个数组，每一项有三个参数:
Configuration Name:   当前配置的名字;
Delegate Class Name:  与哪个Scene代理对象关联;
StoryBoard name: 这个Scene使用的哪个storyboard。
注意：代理方法中调用的是配置名为Default Configuration的Scene，则系统就会自动去调用SceneDelegate这个类。这样SceneDelegate和AppDelegate产生了关联。
*/
- (UISceneConfiguration *)application:(UIApplication *)application configurationForConnectingSceneSession:(UISceneSession *)connectingSceneSession options:(UISceneConnectionOptions *)options {
    // Called when a new scene session is being created.
    // Use this method to select a configuration to create the new scene with.
    return [[UISceneConfiguration alloc] initWithName:@"Default Configuration" sessionRole:connectingSceneSession.role];
}

// 在分屏中关闭其中一个或多个scene时候回调用
- (void)application:(UIApplication *)application didDiscardSceneSessions:(NSSet<UISceneSession *> *)sceneSessions {
    // Called when the user discards a scene session.
    // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
    // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
}
```

SceneDelegate.m中的默认代理方法如下：


```
- (void)scene:(UIScene *)scene willConnectToSession:(UISceneSession *)session options:(UISceneConnectionOptions *)connectionOptions {
    // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
    // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
    // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
}

- (void)sceneDidDisconnect:(UIScene *)scene {
    // Called as the scene is being released by the system.
    // This occurs shortly after the scene enters the background, or when its session is discarded.
    // Release any resources associated with this scene that can be re-created the next time the scene connects.
    // The scene may re-connect later, as its session was not neccessarily discarded (see `application:didDiscardSceneSessions` instead).
}

- (void)sceneDidBecomeActive:(UIScene *)scene {
    // Called when the scene has moved from an inactive state to an active state.
    // Use this method to restart any tasks that were paused (or not yet started) when the scene was inactive.
}

- (void)sceneWillResignActive:(UIScene *)scene {
    // Called when the scene will move from an active state to an inactive state.
    // This may occur due to temporary interruptions (ex. an incoming phone call).
}

- (void)sceneWillEnterForeground:(UIScene *)scene {
    // Called as the scene transitions from the background to the foreground.
    // Use this method to undo the changes made on entering the background.
}

- (void)sceneDidEnterBackground:(UIScene *)scene {
    // Called as the scene transitions from the foreground to the background.
    // Use this method to save data, release shared resources, and store enough scene-specific state information
    // to restore the scene back to its current state.
}
```

1. 不需要多窗口（multiple windows）
如果需要支持iOS 13 及之前多个版本的iOS，且又不需要多个窗口的功能，可以删除项目info.plist文件中的Application Scene Manifest的配置数据，AppDelegate中关于Scene的代理方法、SceneDelegate的类是否删除都可以。
如果使用纯代码来实现显示界面，需要在AppDelegate.h中手动添加window属性，添加以下代码即可：



```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    [self.window setBackgroundColor:[UIColor whiteColor]];
    
    ViewController *con = [[ViewController alloc] init];
    UINavigationController *nav = [[UINavigationController alloc] initWithRootViewController:con];
    [self.window setRootViewController:nav];
    [self.window makeKeyAndVisible];
    return YES;
}
```