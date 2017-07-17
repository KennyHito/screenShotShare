# screenShotShare

iOS开发中,手机截图分享功能</br>
---------------------------------

步骤1:获取用户的截屏事件</br>
---------------------------------

目前有两种方式：</br>
1.注册通知  
iOS7提供一个崭新的推送方法：UIApplicationUserDidTakeScreenshotNotification。只要像往常一样订阅即可知道什么时候截图了。</br>
注意：UIApplicationUserDidTakeScreenshotNotification 将会在截图完成之后显示。现在在截图截取之前无法得到通知。</br>
希望苹果会在iOS8当中增加 UIApplicationUserWillTakeScreenshotNotification。（只有did, 没有will显然不是苹果的风格...）</br>

~~~
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(userDidTakeScreenshot:)
name:UIApplicationUserDidTakeScreenshotNotification object:nil];

- (void)userDidTakeScreenshot:(NSNotification *)notification{
    NSLog(@"检测到截屏");
}
~~~

2.第二种是通过开源库ShotBlocker，但是需要获取用户的相册的权限

~~~
[[ShotBlocker sharedManager] detectScreenshotWithImageBlock:^(UIImage *screenshot) {</br>
NSLog(@"Screenshot! %@", screenshot);

}
~~~

步骤2、获取截图并且漂浮显示,同时增加分享功能</br>
---------------------------------

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

