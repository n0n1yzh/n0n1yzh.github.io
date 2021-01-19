---
title: swiftUI(二)
date: 2021-01-16 3:27:52
tags:
categories:
- swift相关
- swiftUI
cover: false
top_img: top_img: https://s3.ax1x.com/2021/01/08/sKV0Fe.jpg
---

今天是第二天，感觉麻也不会，所以打算梳理一下一些不懂的概念。这些东西都是搬运来的，侵删。

# AppDelegate的详解

IOS 中的 AppDelegate.m/h 文件是很重要的呢，因为它是对 Application的整个生命周期进行管理的。

先明白，每个iPhone应用程序都有一个UIApplication，UIApplication是iPhone应用程序的开始并且负责初始化并显示UIWindow，并负责加载应用程序的第一个UIView到UIWindow窗体中。UIApplication的另一个任务是帮助管理应用程序的生命 周期，而UIApplication通过一个名字为UIApplicationDelegate的代理类来履行这个任务。尽管UIApplication 会负责接收事件，而UIApplicationDelegate则决定应用程序如何去响应这些事件，UIApplicationDelegate可以处理的事件包括应用程序的生命周期事件（比如程序启动和关闭）、系统事件（比如来电、记事项警 告），本文会介绍如何加载应用程序的UIView到UIWindow以及如何利用UIApplicationDelegate处理系统事件。

通常对于UIApplication读者是没必要修改它的，只需要知道UIApplication接收系统事件即可，而如何编写代码来处理这些系统事件则 是程序员的工作。处理系统事件需要**编写一个继承自UIApplicationDelegate接口的类**，而**UIApplicationDelegate接口提供 *生命周期函数* 来处理应用程序以及应用程序的系统事件**，这些生命周期函数如下表所示：

 1、- (BOOL)application:(UIApplication \*)applicationdidFinishLaunchingWithOp tions:(NSDictionary \*)launchOptions
{
 NSLog(@"当程序载入后执行");
}
说明：**当程序载入后执行，应用程序启动入口**。只在应用程序启动时执行一次。也就是说在应用程序启动后，要执行的委托调用。application参数用来获取应用程序的状态、变量等，值得注意的是字典参数：(NSDictionary*)launchOptions，该参数存储程序启动的原因。

若用户直接启动，lauchOptions内无数据;

若由其他应用程序通过openURL:启动，则UIApplicationLaunchOptio nsURLKey对应的对象为启动URL（NSURL）,UIApplicationLaunchOptio nsSourceApplicationKey对应启动的源应用程序的bundle ID (NSString)；

若由本地通知启动，则 UIApplicationLaunchOptionsLocalNotificationKey对应的是为启动应用程序的的本地通知对象(UILocalNotification)；

若由远程通知启动，则 UIApplicationLaunchOptionsRemoteNotificationKey对应的是启动应用程序的的远程通知信息userInfo（NSDictionary）；

其他key还有 UIApplicationLaunchOptionsAnnotationKey,UIApplicationLaunchOptionsLocationKey,
UIApplicationLaunchOptionsNewsstandDownloadsKey。

如果要在启动时，做出一些区分，那就需要在下面的代码做处理。比如：应用可以被某个其它应用调起（作为该应用的子应用），要实现单点登录，那就需要在启动代码的地方做出合理的验证，并跳过登录。
例子：
\- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
{
  NSURL *url =[ launchOptionsobjectForKey:UIApplicationLaunchOptionsURLKey];
  if(url)
  {
  }
  NSString*bundleId = [ launchOptionsobjectForKey:UIApplicationLaunchOptionsSourceApplicationKey];
  if(bundleId)
  {
  }
  UILocalNotification * localNotify = [ launchOptionsobjectForKey:UIApplicationLaunchOptionsLocalNotificationKey];
  if(localNotify)
  {
  }
  NSDictionary* userInfo = [ launchOptionsobjectForKey:UIApplicationLaunchOptionsRemoteNotificationKey];
  if(userInfo)
  {
  }
} 

2、- (void)applicationWillResignActive:(UIApplication*)application
{
NSLog(@"应用程序将要进入非活动状态，即将进入后台");
}
在应用程序将要由活动状态切换到非活动状态时候，要执行的委托调用，如 按下 home按钮，返回主屏幕，或全屏之间切换应用程序等。                    
说明：当应用程序将要进入非活动状态时执行，在此期间，应用程序不接收消息或事件，比如来电话了。

3、-(void)applicationDidEnterBackground:(UIApplication*)application
{
   NSLog(@"如果应用程序支持后台运行，则应用程序已经进入后台运行"); 
}

说明： 当程序被推送到后台的时候调用。所以要设置后台继续运行，则在这个函数里面设置即可


4、- (void)applicationWillEnterFore ground:(UIApplication*)application
{
     NSLog(@"应用程序将要进入活动状态，即将进入前台运行");
}
说明： 当程序从后台将要重新回到前台时候调用，这个刚好跟上面的那个方法相反。

5、- (void)applicationDidBecomeActi ve:(UIApplication*)application
{
    NSLog(@"应用程序已进入前台，处于活动状态");
}
说明： 当应用程序进入活动状态时执行，这个刚好跟上面那个方法相反。

6、- (void)applicationWillTerminate :(UIApplication*)application
{
    NSLog(@"应用程序将要退出，通常用于保存数据和一些退出前的清理工作"); 
}
说明： 当程序将要退出是被调用，通常是用来保存数据和一些退出前的清理工作。这个需要要设置UIApplicationExitsOnSusp end的键值。

7、- (void)applicationDidReceiveMem oryWarning:(UIApplication*)application
{
   NSLog(@"系统内存不足，需要进行清理工作");
}
说明：iPhone设备只有有限的内存，如果为应用程序分配了太多内存操作系统会终止应用程序的运行，在终止前会执行这个方法，通常可以在这里进行内存清理工作防止程序被终止。

8、-(void)applicationSignificantTi meChange:(UIApplication*)application
{
   NSLog(@"当系统时间发生改变时执行");
}
说明： 当系统时间发生改变时执行

9、- (void)application:(UIApplication)application willChangeStatusBarFrame :(CGRect)newStatusBarFrame
{
  NSLog(@"StatusBar框将要变化");
}
说明： 当StatusBar框将要变化时执行

10、- (void)application:(UIApplication*)applicationwillChangeStatusBarOrien tation:
(UIInterfaceOrientation)newStatusBarOrientationduration:(NSTimeInterval)duration
{
}
说明：当StatusBar框方向将要变化时执行

11、- (BOOL)application:(UIApplication*)applicationhandleOpenURL:(NSURL*)url
{
}
说明： 当通过url执行

12、- (void)application:(UIApplication*)application didChangeStatusBarOrient ation:(UIInterfaceOrientation)oldStatusBarOrientation
{
}
说明： 当StatusBar框方向变化完成后执行

13、- (void)application:(UIApplication*)applicationdidChangeSetStatusBarFra me:(CGRect)oldStatusBarFrame
{
}
说明： 当StatusBar框变化完成后执行

另外还有一些协议方法需要知道：
Handling Remote Notifications  （处理远程消息）



-( void) **application**:(UIApplication *) **application**didReceiveRemoteNotifica tion:(NSDictonary *) userinfo
说明： 当一个运行着的应用程序收到一个远程的通知 发送到委托去...
-( void) **application**：(UIApplication *) **application**didRegisterForRemoteNoti ficationsWithDeviceToken :(NSData *) deviceToken
说明： 当一个应用程序成功的注册一个推送服务（APS） 发送到委托去...
-( void) **application**:(UIApplication *) **application**didFailToRegisterForRemo teNotificationsWithError :(NSError *) error
说明： 当 APS无法成功的完成向 程序进程推送时 发送到委托去...

Handling Local Notification （处理本地消息）

-( void) **application**:(UIApplication *) **application**didReceiveLocalNotificat ion:(UILocalNotification *)notification
说明： 当一个运行着的应用程序收到一个本地的通知 发送到委托去...

Responding to Content Protections Changes（响应受保护内容的改变）

-applicationProtectedData WillBecomeUnavailable:
说明： 通知委托，受保护的文件当前变为不可用的
-applicationProtectedData WillBecomeAvailable:
说明： 通知委托 受保护的文件当前变为可用



# Storyboard

这里其实之前也看到了好多网上的教程都是用single view application的那个模版，但是Beta版它没了

好，是不是麻也不会？

到这里的时候我去问群，学长说可以手搓一个，但是我不会，一开始我还去Github上找那个以前的老模版，但是说实话，我直到去搜页面跳转才算是清楚了我的方向是错的，这都是五六年前的东西了，看了也没什么用，当然是对我这样一个刚刚接触swift或者说编程的人来说的。

然后算是终于有头绪了。

然后今天写了点，其他算是有头绪。

<center><img src="https://s3.ax1x.com/2021/01/16/sBdpPU.png" style="zoom: 50%;" /></center> 

剩下还没实现的梳理一下吧。

点save的话会把记录的时间以一个TableView的视图呈现在主界面上，这个TableView存在一个List里（我感觉应该是这样），加入类似append这样的操作，比较难的应该是如何让手机音频响，这个我去hackingwithswift上搜了下，有一个AVAudioPlayer可以用，剩下的就是明天去实践了。

睡觉觉～塞牙～