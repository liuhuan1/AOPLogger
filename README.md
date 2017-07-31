# AOPLogger 切面日志 ![](http://cocoapod-badges.herokuapp.com/v/AOPLogger/badge.png) ![](http://cocoapod-badges.herokuapp.com/p/AOPLogger/badge.png)
## 目的
主要是为了从主工程内分离，可以单独的写日志系统，通过类扩展定制不同的统计方式
##调用
程序内调用这里就不多说了，看注释就都懂了
这里详细说说用Plist的形式或者其他形式，内部真实的字典结构可以看下面Plist的图了解

![Screenshot1](http://heroims.github.io/AOPLogger/QQ20170306-012628.png "Screenshot1") 

最外层的Key就是对应的类名内部对应一个字典，基础Key就是提供的几个常量，要统计的方法名，要统计的方法执行类型，最后是要统计的日志信息，这里再细说一下要统计的日志信息，其实可以在类扩展里加入定制，这样甚至可以根据要统计的日志信息，根据信息内容处理要执行的方法怎么处理怎么记录
###
```Objective-C
extern NSString * const AOPLoggerMethod;//要统计的日志方法Key
extern NSString * const AOPLoggerLogInfo;//要统计的日志信息Key
extern NSString * const AOPLoggerPositionAfter;//方法执行后统计日志
extern NSString * const AOPLoggerPositionBefore;//方法执行前统计日志
extern NSString * const AOPLoggerPositionType;//执行日志统计的类型Key

@protocol AOPLoggerGetConfigInfoProtocol <NSObject>

@required

/**
 创建类扩展如果使用此协议必须实现此方法
 此方法返回统计的配置信息，可以从网络取也可以从本地取

 @return 统计配置字典
 */
-(NSDictionary*)al_getConfigInfo;

@end
@protocol AOPLoggerBLLProtocol <NSObject>

@required

/**
 创建类扩展如果使用此协议必须实现此方法
 此方法主要来处理切面方法后的log信息处理可以存本地也可以使用其他任意第三方输出

 @param log 配置文件里定义的AOPLoggerLogInfo信息
 @param originAOP AspectInfo的方法信息，第三方库Aspect返回的切面方法的所有信息
 */
-(void)al_logger:(id)log originAOP:(id)originAOP;

@end

@interface AOPLogger : NSObject


/**
 开始读取日志Plist配置文件
 */
+(void)startAOPLoggerWithPlist;

/**
 统计日志的调用方法
 （如果不想增加开机时间可以采取每个模块创建一个日志统计类适时调用，在该类里提供一个初始化方法，内部调用此即可）

 @param classString 类名
 @param methodString 方法名
 @param log 相当于AOPLoggerLogInfo信息
 */
+(void)AOPLoggerWithClassString:(NSString*)classString methodString:(NSString*)methodString log:(id)log;

/**
 统计日志的调用方法
 （如果不想增加开机时间可以采取每个模块创建一个日志统计类适时调用，在该类里提供一个初始化方法，内部调用此即可）

 @param classString 类名
 @param methodString 方法名
 @param log 相当于AOPLoggerLogInfo信息
 @param logPosition 日志统计时位置，可放在方法运行前或运行后（默认运行后执行日志统计）
 */
+(void)AOPLoggerWithClassString:(NSString *)classString methodString:(NSString *)methodString log:(id)log logPosition:(NSString*)logPosition;

@end

```
## Installation

### via CocoaPods
Install CocoaPods if you do not have it:-
````
$ [sudo] gem install cocoapods
$ pod setup
````
Create Podfile:-
````
$ edit Podfile
platform :ios, '5.0'

pod 'AOPLogger'

#切面所有点击事件
pod 'AOPLogger/AOPClick'

$ pod install
````
Use the Xcode workspace instead of the project from now on.
