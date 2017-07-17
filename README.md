概述
---------------------------------
* 本工程主要是利用iOS 的Objective-C开发的技术要点汇总；
* 涵盖了开发中踩坑的原因，以及填坑的技术分享；
* 抛砖引玉，取长补短，希望能够提供一点思路，避免少走一些弯路。
* 注意 : 此Demo核心代码结合网络一名程序猿共同完成的,谢谢大家!

功能
--------------------------------

* iOS开发中,手机截图并且同时实现分享功能


要求
---------------------------------

* iOS 8+
* Xcode 8.0+

期待
---------------------------------

* 如果在使用过程中遇到BUG，希望你能Issues我，谢谢(或者尝试下载最新的代码看看BUG修复没有)。
* 如果在使用过程中发现有更好或更巧妙的实用技术，希望你能Issues我，我非常为该项目扩充更多好用的技术，谢谢。
* 如果通过该工程的使用，对您在开发中有一点帮助，码字不易，还请点击右上角star按钮，谢谢。


##### 步骤1:获取用户的截屏事件</br>

目前有两种方式：</br>
1.注册通知(常用的方式)  
* iOS7提供一个崭新的推送方法:UIApplicationUserDidTakeScreenshotNotification 将会在截图完成之后显示。现在在截图截取之前无法得到通知。</br>

~~~
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(userDidTakeScreenshot:) name:UIApplicationUserDidTakeScreenshotNotification object:nil];

- (void)userDidTakeScreenshot:(NSNotification *)notification{
    NSLog(@"检测到截屏");
}
~~~

2.第二种是通过开源库ShotBlocker，但是需要获取用户的相册的权限

~~~
[[ShotBlocker sharedManager] detectScreenshotWithImageBlock:^(UIImage *screenshot) {

    NSLog(@"Screenshot! %@", screenshot);
}
~~~

##### 步骤2、获取截图并且漂浮显示,同时增加分享功能</br>

~~~
UIWindow *keyWindow=[[UIApplication sharedApplication]keyWindow];
DPScreenshotsPopView *popView=[DPScreenshotsPopView initWithScreenShots:screenshot selectSheetBlock:^(SelectSheetType type) {
    if (type==QQSelectSheetType) {
        NSLog(@"点击的是QQ分享");
    }else if (type==WeiXinSelectSheetType){
        NSLog(@"点击的是微信好友分享");
    }else if (type==WeiXinCircleSelectSheetType){
        NSLog(@"点击的是微信朋友圈分享");
    }
}];
[popView show];
[keyWindow addSubview:popView];
~~~

最后,查看效果图
---------------------------------
![效果图](/ziji.gif)

