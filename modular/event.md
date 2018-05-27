# 事件

使用事件监听机制，以实现更加灵活方便的跨组件交互。

在事件实现中，我们提供了方便好用的接口 ：`Axe`中的事件支持 同步、异步、优先级设定。

以下是iOS中的`Axe`框架的使用说明 ：

## 注册监听 

	[AXEEvent registerListenerForEventName:@"eventA" handler:^(AXEData *payload) {
        NSLog(@"eventA !!!");
    }];

注册的返回值是一个 `id<AXEListenerDisposable>` ， 如果要取消监听，调用其`dispose`方法。

由于监听使用`block`， 所以一定要注意内存问题，避免内存泄漏。

基于监听，我们还实现了两个有用的功能：

##  组件初始化

基于事件通知实现组件自注册。 通过事件中的优先级和同异步设定，来进行组件初始化的排序与管理：

	@implementation LoginModuleInitialize
	
	AXEMODULEINITIALIZE_REGISTER()
	
	- (void)AEXInitialize {
	    [AXEAutoreleaseEvent registerSyncListenerForEventName:AXEEventModulesBeginInitializing handler:^(AXEData *payload) {
	        [[AXERouter sharedRouter] registerPath:@"login/login" withJumpRoute:^(AXERouteRequest *request) {
	            LoginViewController *vc = [[LoginViewController alloc] init];
	            vc.routeRequest = request;
	            vc.hidesBottomBarWhenPushed = YES;
	            [request.fromVC.navigationController pushViewController:vc animated:YES];
	        }];
	        [[AXERouter sharedRouter] registerPath:@"login/register" withJumpRoute:^(AXERouteRequest *request) {
	            RegisterViewController *vc = [[RegisterViewController alloc] init];
	            vc.routeRequest = request;
	            vc.hidesBottomBarWhenPushed = YES;
	            [request.fromVC.navigationController pushViewController:vc animated:YES];
	        }];
	    } priority:DEMOGROUND_MODULE_INIT_PRIORITY];
	
	}
	@end

上诉代码进行登录组件的初始化，该部分代码放在登录组件内部。当接入登录组件时，就会在启动时自动初始化，以注册路由。


## 界面监听

提供一种适用于界面展示的监听接口，该监听会保留回调，直到页面回到前台才执行；且同名的通知，不会出现重复处理，只会使用最新的数据执行一次。

![](../Axe/5.png)

使用方式 ：

	[wself registerUIEvent:TEST_EVENT_LOGINSTATUS withHandler:^(AXEData *payload) {
                BOOL login = [payload boolForKey:LOGIN_DATA_LOGIN];
                if (!login) {
                    [wself toastMessage:@"退出登录 ！！！"];
                }
 	}];

