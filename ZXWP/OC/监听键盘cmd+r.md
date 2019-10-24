# UIKeyCommand

通过创建UIKeyCommand实例, 添加到系统, 在创建实例的时候, 就已经决定action了

添加到系统两种方法:
1. 通过UIViewController的addKeyCommand, 来把UIKeyCommand实例添加到系统, 得以监听, keyCommand的action的target就是vc实例
2. 通过添加UIResponse的分类, 把keyCommand属性的get方法重写一遍, return一个NSArray<id<UIKeyCommand>>的数组, 那系统就会自动注册这些keyCommand, key的action的target就是UIResponse, 需要还需要在分类上添加action, 响应这个通知.


# 架构设计
需求: 目前需要让APP监听cmd+r / d, r则重新刷新, d则弹出alertSheet

直接码: 直接撰写UIResponse分类, 在keyCommand上返回UIKeyCommand(r/d), 添加处理的action, 在action上, 判断是r还是d, 做出不同的选择.

以上实现有以下缺点:
* 所有逻辑都写在action里. 
* 目前只有UIResponse可以注册UIKeyCommand, 但可能其他类也想注册.

设计一个架构, 弥补以上的缺点.
* 任何类都能注册UIKeyCommand, 并且有自己的实现.

实现:
* 设计一个interface, 使得所有实现这个interface的类都能监听cmd+r, 并有自己的action
    * 这样会导致有多个实例监听, 并有多个action
    * 但是我们的UIResponse的action只有一个.
    * 所有会有个地方记录这些实例, 并在收到来自Response的action的时候, 遍历这些实例, 一个个执行interface的方法
* 综上, 创建一个RCTReloadCommand类
    * 编写interface
    * 创建一个方法(表示加入监听的实例), 入参是实现interface的实例, 该方法做如下:
        * 维护一个队列, 装载实现interface的实例
    * 创建一个方法, 接收Response监听cmd+r的通知, 如下行为
        * 遍历队列, 执行每个实例的interface接口方法.
* 接下来就监听, 写Response的分类. 
    * 分类需要一次性返回一个数组, 元素是UIKeyCommand
        * 每次更新keyWindow的rootViewController都会触发一次该方法
    * 但有可能各个不同的地方, 监听不懂的按钮, cmd+d / i / n
    * 所以要收齐这些按钮, 再一次性返回, 因此要先收集各个地方, 所要监听的按钮.
    * 综上所述, 设计一个单例, 名为RCTKeyCommands
        * 维护一个commands的成员变量
        * 暴露一个注册方法, 输入input和action
        * 上述的ReloadCommands就使用这个类来注册cmd+r接口
* 封装UIKeyCommand
    * 为什么要封装UIKeyCommand
        * 封装成RCTKeyCommand, 带一个UIKeyCommand, 和一个block
        * 因为RCTKeyCommands暴露出去监听的接口, 不是用interface, 而是用block, 拿到这个block, 要与UIKeyCommand结合在一起, 所以要封装.
    * 