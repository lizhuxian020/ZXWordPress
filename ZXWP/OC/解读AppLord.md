# 线程安全处理
`CLOCK([_config setObject:value forKey:key];)`

CLOCK是个宏(便利宏), 定义如下:
```
#define CLOCK(...) dispatch_semaphore_wait(_configLock, DISPATCH_TIME_FOREVER); \
__VA_ARGS__; \
dispatch_semaphore_signal(_configLock);
```
1. _configLock初始化为1, 为0时当前线程还能正常使用, 为负数时时阻塞当前线程, 直到时间到了为止
2. dispatch_semaphore_signal : +1
3. dispatch_semaphore_wait : -1

DEMO1
```
//touchBegin:
_signal = 1;
wait(signal);
dispatch_after_inMainThread(3s, {
    do something;
    sign(_signal)
})
```
现象: 连续点两次之后, 主线程卡死. 只能点完, 等3s, 再点
解释:
1. 点第一次, _signal变成0, 主线程还能继续
2. 所以能点第二次, _signal变成-1, 主线程被卡死, 第一次的3s到期后, 主线程已经动不了了, 所以sign也动不了

DEMO2
```
//touchBeigin
NSLog(FunctionName)
CLOKC({
    for(int i = 0; i < 100000; i++){
        NSLog@"%d", i);
    }
})
```
现象: 狂点, 主线程不卡死, 按顺序打印(FunctionName, i:0~10000), 第二遍(FunctionName, i:0~10000),
解释:
1. 打印i很耗时, 而且在狂点情况下, TouchBegin没有因为狂点而马上连续调用wait
2. 与DEMO1对比, DMEO2的wait和sign都在同一个线程(逻辑同步, 即: 想要下一个wait, 必须等上一个sign好了.), 没办法实现"在同一个线程上"在上一个sign没完成的时候马上wait
3. 综上所述: CLOCK是没毛病的, 线程安全!!

写了这多, 发现CLOCK和@synchronized (self)是一样的=.=
```
//是一样效果的=.=
CLOCK({
    for (int i = 0; i < 100000; i++) {
        NSLog(@"%d", i);
    }
})
@synchronized (self) {
    for (int i = 0; i < 10000; i++) {
        NSLog(@"%d", i);
    }
}
[NSLock lock];
[NSLock unlock];

#ALContext
MACH-O文件有3部分: header, segment, commend(大概=.=)
在每个模块声明的: @ALModule(ModuleName), @ALService(ServiceName), 都是在header里写入一些字符串
在ALContext这个单例里, init的时候, 会就去读取这些字符串, 拿到moduleName和serviceName, 然后去生成对应的对象
以上逻辑可以用+load代替
