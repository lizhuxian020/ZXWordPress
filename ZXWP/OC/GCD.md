# dispatch_async / dispatch_sync
加上并发队列, 串行队列, 一共四种情况

* 在并发队列, 添加异步任务
    * 会把所有异步任务, 分发到多个(因为并发)子线程(因为异步)
    * 先后执行顺序, 与添加到队列的顺序无关(因为并发)
* 在并发队列, 添加同步任务
    * 全在当前线程执行, 并顺序执行(因为同步)
    * 个人理解: 
        * 这种情况应该比较少用, 相当于把"阻塞当前线程的任务""分发"到当前线程
        * 下面的例子可知: 同步任务的添加, 会阻塞当前线层, 并等把之前添加过的任务全部执行完, 之后才执行该同步任务.
* 在串行队列, 添加异步任务
    * 开启子线程(因为异步), 并在该子线程顺序(因为串行)执行全部任务
* 在串行队列, 添加同步任务
    * 当前线程, 全部顺序执行
    * 个人理解: 
        * 阻塞当前线程, 把该串行队列里的"在该同步任务之前的"任务全部执行完
        * 如果该串行队列是主队列, 在阻塞当前线程的情况下, 主队列已经无法执行了, 然后同步队列还要把主队列里的执行完, 才执行, 所以会阻塞.

```
syncQueue = dispatch_queue_create("asd", DISPATCH_QUEUE_CONCURRENT);
[self doSomeAsync];
[self doSomeSync];
    
    
- (void)doSomeAsync {
    doThreeTime(^{
        dispatch_async(syncQueue, ^{
            NSLog(@"asd + %@", NSThread.currentThread);
        });
    });
}

- (void)doSomeSync {
    NSLog(@"123312");
    
    doThreeTime(^{
        dispatch_sync(syncQueue, ^{
            NSLog(@"zxc + %@", NSThread.currentThread);
        });
    });
    NSLog(@"123");
}

2019-09-29 09:33:49.034524+0800 GCD[37708:1580404] 123312
2019-09-29 09:33:49.034772+0800 GCD[37708:1580477] asd + <NSThread: 0x600002d19e40>{number = 5, name = (null)}
2019-09-29 09:33:49.034838+0800 GCD[37708:1580479] asd + <NSThread: 0x600002d1d200>{number = 6, name = (null)}
2019-09-29 09:33:49.034848+0800 GCD[37708:1580404] zxc + <NSThread: 0x600002d45380>{number = 1, name = main}
2019-09-29 09:33:49.034854+0800 GCD[37708:1580476] asd + <NSThread: 0x600002d19c00>{number = 4, name = (null)}
2019-09-29 09:33:49.034861+0800 GCD[37708:1580478] asd + <NSThread: 0x600002d1aa40>{number = 3, name = (null)}
2019-09-29 09:33:49.034939+0800 GCD[37708:1580404] zxc + <NSThread: 0x600002d45380>{number = 1, name = main}
2019-09-29 09:33:49.035034+0800 GCD[37708:1580404] zxc + <NSThread: 0x600002d45380>{number = 1, name = main}
2019-09-29 09:33:49.035255+0800 GCD[37708:1580404] zxc + <NSThread: 0x600002d45380>{number = 1, name = main}
2019-09-29 09:33:49.035371+0800 GCD[37708:1580404] 123
```

* 灵感总结=.=
    * 先执行async, 让4个async任务异步的放入syncQueue里
    * 会有头x个(0~4)个async任务先放进去
    * 然后执行sync, 先打印123321
    * 之后插入第一个sync任务, dispatch_sync的意思是, 阻塞当前线程, 往队里插入一个任务, 并执行完毕, 再回到原来线程继续执行.
        * 所以这里当有x个async已经插入队列的时候, 先把队列里的执行完, 最后执行插入第一个sync任务, 然后返回原来线程
        * 然后插入第二个sync任务期间, 又有y个async任务插入, 所以在执行第二个sync之前, 先把队列里的任务执行完, 然后执行sync
        * 类似这样执行, 最后执行完所有任务, 打印123
    * 以上是我的灵感总结. 不知道错多少=.=

    
```
syncQueue = dispatch_queue_create("asd", DISPATCH_QUEUE_SERIAL);    
[self doSomeAsync];

- (void)doSomeAsync {
    __block int i = 0;
    doThreeTime(^{
        dispatch_async(syncQueue, ^{
            NSLog(@"asd%d + %@", i++, NSThread.currentThread);
        });
    });
}


2019-09-29 10:04:13.808373+0800 GCD[38348:1638337] asd0 + <NSThread: 0x60000228be80>{number = 3, name = (null)}
2019-09-29 10:04:13.808600+0800 GCD[38348:1638337] asd1 + <NSThread: 0x60000228be80>{number = 3, name = (null)}
2019-09-29 10:04:13.808685+0800 GCD[38348:1638337] asd2 + <NSThread: 0x60000228be80>{number = 3, name = (null)}
2019-09-29 10:04:13.808765+0800 GCD[38348:1638337] asd3 + <NSThread: 0x60000228be80>{number = 3, name = (null)}

```