# 声明文件

声明文件是对一个组件对外接口的描述文件，详细说明了组件的

* 路由
* 事件
* 数据

所有这些内容，都是以字符串形式进行声明的，而数据中的`Model`类型需要声明一下具体结构。

## iOS的声明文件

为了实现编译隔离，iOS中的声明文件是头文件形式，且只有宏和`protocol`的声明。 

宏的格式为 `MODULE_TYPE_DETAIL` ， 如：

	#define LOGIN_DATA_ACCOUNT                  @"LOGIN_DATA_ACCOUNT"
	#define LOGIN_ROUTER_LOGIN                  @"axes://login/login"
	#define LOGIN_EVENT_LOGINSTATUS         	@"LOGIN_EVENT_LOGINSTATUS"

通过`protocol`声明`Model`类型 ：

	@protocol LoginUserInfoModelProtocol <NSObject>
	@property (nonatomic,strong) NSString *account;
	@property (nonatomic,strong) NSNumber *level;
	@property (nonatomic,strong) NSDictionary *detailInfo;
	@property (nonatomic,strong) NSArray *tagList;
	@end
