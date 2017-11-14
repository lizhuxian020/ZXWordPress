---
ID: 14
post_title: dispatch_semaphore
author: lizhuxian020@gmail.com
post_excerpt: ""
layout: post
permalink: https://leezix13.com/wordpress/?p=14
published: true
post_date: 2017-07-14 09:47:03
---
# dispatch_semaphore的使用

> 参考文章: [关于dispatch_semaphore的使用][1]  dispatch_semaphore是GCD的方法, 用于线程间的通讯, 与他相关的主要是已下三个方法: dispatch_semaphore_create, dispatch_semaphore_signal, dispatch_semaphore_wait 
*   dispatch_semaphore_create定义为: `dispatch_semaphore_t dispatch_semaphore_create(long value);` 
    *   入参value: 必须>=0, 否则会返回NULL
    *   作用: 生成一个类型为**dispatch_semaphore_t**的信号量
*   dispatch_semaphore_singal定义为: `long
dispatch_semaphore_signal(dispatch_semaphore_t dsema);` 
    *   作用: 入参会自增1 (值+1)
*   dispatch_semaphore_wait定义为: `long
dispatch_semaphore_wait(dispatch_semaphore_t dsema, dispatch_time_t timeout);` 
    *   如果dsema > 0, 则dsema -= 1, 该方法所在的线程会继续往下执行
    *   如果dsema = 0, 则该方法所在线程会阻塞. 
        *   在timeout之前如果dsema自增1(也就是遇到方法2: dispatch_semaphore_signal), 则dsema -= 1, 该方法所在的线程会继续往下执行. 
        *   如果过了timeout, 线程还是会继续往下执行

### 返回值

*   dispatch_semaphore_singal的返回值为long: 
    *   返回0: 表示当前没有线程在等待你这个信号量=.=
    *   返回不是0: 则表示有线程在等你的信号量, 并解除一个wait. (唤醒优先级最高的线程那个wait=.=, 否则随机唤醒)
*   dispatch_semaphore_wait的返回值为long: 
    *   返回0: 表示在timeout之前被唤醒
    *   返回不是0: 表示timeout发生

> 如何设置timeout: (dispatch_semephore_t) 
> > 可以使用宏: `DISPATCH_TIME_NOW`, `DISPATCH_TIME_FOREVER` 或者使用dispatch_time创建: `dispatch_time_t dispatch_time(dispatch_time_t when, int64_t delta)；` 例如: `dispatch_time_t  t = dispatch_time(DISPATCH_TIME_NOW, 1*1000*1000*1000);` 表示当前时间后延一秒为timeout 
* * *

## 生动形象的例子 dispatch_semaphore好比如停车场: 

*   一共有4个车位: signal = dispatch_semaphore_create(4)
*   来了一个车子: dispatch_semaphore_wait(singal) 
    *   如果都停满了就要等, 超过timeout就走=.= 
*   走了一个车子: dispatch_semaphore_signal(signal) 
    *   走了一辆就多一个坑=.=

 [1]: http://www.cnblogs.com/snailHL/p/3906112.html