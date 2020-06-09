# NSOperation
1. 是一个抽象类, 一般使用子类: NSInvocationOperation, NSBlockOperation
2. 每一个operation对象有自己的State: 
    1. cancelled
    2. executing
    3. finished
    4. ready
3. 可以实现依赖, 依赖某个operation任务结束之后, 才执行. 就是有前置任务

#与GCD的区别
1. 实现依赖, 
    1. NSOperation的依赖
    2. GCD实现依赖是通过barrier实现或者group
2. 停止线程
    1. NSOperation的停止
    2. GCD要停止已经加入queue的线程, 需要写复杂的代码
3. 优先级
    1. NSOperation可以对任务进行设置优先级
    2. GCD只能对队列进行设置优先级
4. NSOperation可以跨队列操作依赖关系
5. 最大并发数
    1. NSOperation可以设置最大并发数
    2. GCD不好做
6. 继续/暂停/全部取消
    1. NSOperation好做
    2. GCD不好做

使用的步骤和GCD一样
1. 创建队列
2. 创建任务(NSOperation)
3. 调度(有两种方法)
    1. [NSOperation start]
    2. 添加到队列中

```c
NSInvocationOperation *p = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(asd) object:nil];
[p start];
```    
```c
NSOperationQueue *q = [[NSOperationQueue alloc] init];
NSInvocationOperation *p = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(asd) object:nil];
    
[q addOperation:p];
```
```c
NSOperationQueue *q = [[NSOperationQueue alloc] init];
    
NSBlockOperation *o = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"%s --- %@", __FUNCTION__, NSThread.currentThread);
}];
    
[q addOperation:o];
```
```c
NSOperationQueue *q = [[NSOperationQueue alloc] init];
    
[q addOperationWithBlock:^{
    NSLog(@"%s --- %@", __FUNCTION__, NSThread.currentThread);
}];
```

