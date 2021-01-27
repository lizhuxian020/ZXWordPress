# 计算机基础
1. http与https的区别
2. SSL概念和通讯过程
3. UDP和TCP区别

# iOS
1. NSArray与NSSet的区别？
NSArray内存中存储地址连续，而NSSet不连续
NSSet效率高，内部使用hash查找；NSArray查找需要遍历
NSSet通过anyObject访问元素，NSArray通过下标访问

2. 属性关键字assign、retain、weak、copy

assign：用于基本数据类型和结构体。如果修饰对象的话，当销毁时，属性值不会自动置nil，可能造成野指针。
weak：对象引用计数为0时，属性值也会自动置nil
retain：强引用类型，ARC下相当于strong，但block不能用retain修饰，因为等同于assign不安全。
strong：强引用类型，修饰block时相当于copy。

3. weak属性如何自动置nil的？

Runtime会对weak属性进行内存布局，构建hash表：以weak属性对象内存地址为key，weak属性值(weak自身地址)为value。当对象引用计数为0 dealloc时，会将weak属性值自动置nil。
4. Block的循环引用、内部修改外部变量、三种block

block强引用self，self强引用block
内部修改外部变量：block不允许修改外部变量的值，这里的外部变量指的是栈中指针的内存地址。__block的作用是只要观察到变量被block使用，就将外部变量在栈中的内存地址放到堆中。
三种block：NSGlobalBlack(全局)、NSStackBlock(栈block)、NSMallocBlock(堆block)

5. KVO底层实现原理？手动触发KVO？

KVO原理：当观察一个对象时，runtime会动态创建继承自该对象的类，并重写被观察对象的setter方法，重写的setter方法会负责在调用原setter方法前后通知所有观察对象值得更改，最后会把该对象的isa指针指向这个创建的子类，对象就变成子类的实例。
如何手动触发KVO：在setter方法里，手动实现NSObject两个方法：willChangeValueForKey、didChangeValueForKey

6. categroy为什么不能添加属性？怎么实现添加？与Extension的区别？category覆盖原类方法？多个category调用顺序

Runtime初始化时categroy的内存布局已经确定，没有ivar，所以默认不能添加属性。
使用runtime的关联对象，并重写setter和getter方法。
Extenstion编译期创建，可以添加成员变量ivar，一般用作隐藏类的信息。必须要有类的源码才可以添加，如NSString就不能创建Extension。
category方法会在runtime初始化的时候copy到原来前面，调用分类方法的时候直接返回，不再调用原类。如何保持原类也调用(https://www.jianshu.com/p/40e28c9f9da5)。
多个category的调用顺序按照：Build Phases ->Complie Source 中的编译顺序。

7. load方法和initialize方法的异同。——主要说一下执行时间，各自用途，没实现子类的方法会不会调用父类的？
load initialize 调用时机 app启动后，runtime初始化的时候 第一个方法调用前调用 调用顺序 父类->本类->分类 父类->本类(如果有分类直接调用分类，本类不会调用) 没实现子类的方法会不会调用父类的 否 是 是否沿用父类实现 否 是

8. 对 runtime 的理解。——主要是方法调用时如何查找缓存，如何找到方法，找不到方法时怎么转发，对象的内存布局

OC中向对象发送消息时，runtime会根据对象的isa指针找到对象所属的类，然后在该类的方法列表和父类的方法列表中寻找方法执行。如果在最顶层父类中没找到方法执行，就会进行消息转发：Method resoution（实现方法）、fast forwarding（转发给其他对象）、normal forwarding（完整消息转发。可以转发给多个对象）

9. runtime 中，SEL和IMP的区别?

每个类对象都有一个方法列表，方法列表存储方法名、方法实现、参数类型，SEL是方法名(编号)，IMP指向方法实现的首地址

10. Runloop与线程的关系？Runloop的mode? Runloop的作用？内部机制？

每一个线程都有一个runloop，主线程的runloop默认启动。
mode：主要用来指定事件在运行时循环的优先级
作用：保持程序的持续运行、随时处理各种事件、节省cpu资源(没事件休息释放资源)、渲染屏幕UI

11. iOS中使用的锁、死锁的发生与避免

@synchronized、信号量、NSLock等
死锁：多个线程同时访问同一资源，造成循环等待。GCD使用异步线程、并行队列

12. NSOperation和GCD的区别

GCD底层使用C语言编写高效、NSOperation是对GCD的面向对象的封装。对于特殊需求，如取消任务、设置任务优先级、任务状态监听，NSOperation使用起来更加方便。
NSOperation可以设置依赖关系，而GCD只能通过dispatch_barrier_async实现
NSOperation可以通过KVO观察当前operation执行状态(执行/取消)
NSOperation可以设置自身优先级(queuePriority)。GCD只能设置队列优先级(DISPATCH_QUEUE_PRIORITY_DEFAULT)，无法在执行的block中设置优先级
NSOperation可以自定义operation如NSInvationOperation/NSBlockOperation，而GCD执行任务可以自定义封装但没有那么高的代码复用度
GCD高效，NSOperation开销相对高

13. oc与js交互

拦截url
JavaScriptCore(只适用于UIWebView)
WKScriptMessageHandler(只适用于WKWebView)
WebViewJavaScriptBridge(第三方框架)

14. struct、Class的区别

class可以继承，struct不可以
class是引用类型，struct是值类型
struct在function里修改property时需要mutating关键字修饰

15. 内存泄露问题？

主要集中在循环引用问题中，如block、NSTime、perform selector引用计数问题。

16. UI卡顿优化？

17. 架构&设计模式

MVC设计模式介绍
MVVM介绍、MVC与MVVM的区别？
SDWebImage源码
AFNetWorking源码分析
组件化的实施，中间件的设计

18. git的常用命令

