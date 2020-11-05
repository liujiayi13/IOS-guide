# IOS笔记
# 协议
为什么要使用协议？

因为Object－C是不支持多继承的，所以很多时候都是用Protocol（协议）来代替。Protocol（协议）只能定义公用的一套接口，但不能提供具体的实现方法。也就是说，它只告诉你要做什么，但具体怎么做，它不关心。

当一个类要使用某一个Protocol（协议）时，都必须要遵守协议。比如有些必要实现的方法，你没有去实现，那么编译器就会报警告，来提醒你没有遵守某某协议。

定义一套公用的接口（Public）

@required：必须实现的方法，默认在@protocol里的方法都要求实现。

@optional：可选实现的方法（可以全部都不实现）


```
#import <Foundation/Foundation.h>
    /**流氓协议*/
    @protocol BadManDelegate <NSObject>
    @required // 必须实现的方法
    /**吃*/
    - (void) eat;
    /**骗*/
    - (void) lie;
    @optional // 可选实现的方法
    - (void) drink;
    @end
```

类中引用头文件，然后
```
interface YHPerson : NSObject <BadManDelegate, GentlemanDelegate>
```
这样就引用了两个协议，必须实现其中required的方法

1. 一个协议可以扩展自另一个协议，例如上面BadManDelegate就扩展自NSObject，如果需要扩展多个协议中间使用逗号分隔；
2. 和其他高级语言中接口不同的是协议中定义的方法不一定是必须实现的，我们可以通过关键字进行@required和@optional进行设置，如果不设置则默认是@required（注意ObjC是弱语法，即使不实现必选方法编译运行也不会报错）；
3. 协议通过<>进行实现，一个类可以同时实现多个协议，中间通过逗号分隔；
4. 协议的实现只能在类的声明上,不能放到类的实现上（也就是说必须写成@interface YHPerson:NSObject <AnimalDelegate>而不能写成@implementation YHPerson<AnimalDelegate>）；
5. 协议中不能定义属性、成员变量等,只能定义方法；





