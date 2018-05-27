# 路由

![](../Axe/4.png)

界面逻辑的处理交给路由。

根据路由的表现，我们定义两种路由：

* 跳转路由 ： 进行页面跳转，如使用`UINavigationController`的`push`，以打开一个新页面。
* 视图路由 ： 返回一个界面元素， 即返回一个`UIViewController`， 以满足嵌套页面的需求， 如 侧边栏、Tab栏等页面。

路由的表现形式是一个`URL`， 如同大多数的路由方案一样。默认支持的路由是`Axe`路由，结构为：

	axe://{moduleName}/{pageName}
	
如果`URL`附带参数，会自动转换为数据类型。路由支持回调处理。
 
 `Axe`的路由支持协议扩展，以实现自定义的路由处理。
 
 以下是iOS中的`Axe`框架的使用说明 ：
 
## 注册路由

	// 跳转路由
	[[AXERouter sharedRouter] registerPath:@"login/register" withJumpRoute:^(AXERouteRequest *request) {
            RegisterViewController *vc = [[RegisterViewController alloc] init];
            vc.routeRequest = request;
            vc.hidesBottomBarWhenPushed = YES;
            [request.fromVC.navigationController pushViewController:vc animated:YES];
        }];
        // 视图路由
	 [[AXERouter sharedRouter] registerPath:@"ios/test" withViewRoute:^UIViewController *(AXERouteRequest *request) {
	            return [[TestViewController alloc] init];
	        }];
	        
## 视图路由

视图路由用来处理界面嵌套组合的情况， 如`UITabBarController` ：

	UITabBarController *tabController = [[UITabBarController alloc] init];
    NSMutableArray *vcList = [[NSMutableArray alloc] init];
    for (NSString *url in routes) {
        UIViewController *vc = [[AXERouter sharedRouter] viewForURL:url];
        [vcList addObject:vc];
    }
    tabController.viewControllers = vcList;
 
## 路由回调
 
 路由跳转时可以设置回调 ：
 
 		[wself jumpTo:LOGIN_ROUTER_REGISTER withParams:nil finishBlock:^(AXEData *payload) {
            id<LoginUserInfo> userInfo = [payload modelForKey:LOGIN_DATA_USERINFO];
            MBProgressHUD *hud = [MBProgressHUD showHUDAddedTo:wself.view animated:YES];
            hud.mode = MBProgressHUDModeText;
            hud.detailsLabel.text = [NSString stringWithFormat:@"注册成功 ,信息如下 ：\n 帐号 : %@ \n 等级: %@ \n detailInfo: %@ \n tagList: %@", userInfo.account, userInfo.level, userInfo.detailInfo, userInfo.tagList];
            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(3 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                [hud hideAnimated:YES];
            });
        }];
        
设定路由时，`block`中可以获取一个`AXERouteRequest`对象，以记录跳转来源和URL信息，以及回调`block`。 

但是回调的逻辑，必须是模块`A`声明自己的某个路由支持回调，并说明回调时附带的数据，然后调用者才可以使用回调。

## 协议路由

默认路由用于处理原生界面，使用协议`axe` 。 我们也支持扩展一些其他的协议， 如 ：

	[[AXERouter sharedRouter] registerProtocol:@"https" withViewRoute:^UIViewController *(AXERouteRequest *request) {
        AXEWebViewController *controller = [AXEWebViewController webViewControllerWithURL:request.currentURL];
        controller.routeRequest = request;
        return controller;
    }];
    
上述代码注册了一个`https`的协议，以使路由模块可通过打开一个`Webview`来处理H5页面。

