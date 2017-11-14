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
<h1>dispatch_semaphore的使用</h1>

<blockquote>
  参考文章: <a href="http://www.cnblogs.com/snailHL/p/3906112.html">关于dispatch_semaphore的使用</a>
</blockquote>

dispatch_semaphore是GCD的方法, 用于线程间的通讯, 与他相关的主要是已下三个方法: dispatch&#95;semaphore&#95;create, dispatch&#95;semaphore&#95;signal, dispatch&#95;semaphore&#95;wait

<ul>
<li>dispatch&#95;semaphore&#95;create定义为: 
<code>dispatch_semaphore_t dispatch_semaphore_create(long value);</code>

<ul>
<li>入参value: 必须>=0, 否则会返回NULL</li>
<li>作用: 生成一个类型为<strong>dispatch&#95;semaphore&#95;t</strong>的信号量</li>
</ul></li>
<li>dispatch&#95;semaphore&#95;singal定义为:
<code>long
dispatch_semaphore_signal(dispatch_semaphore_t dsema);</code>

<ul>
<li>作用: 入参会自增1 (值+1)</li>
</ul></li>
<li>dispatch&#95;semaphore&#95;wait定义为:
<code>long
dispatch_semaphore_wait(dispatch_semaphore_t dsema, dispatch_time_t timeout);</code>

<ul>
<li>如果dsema > 0, 则dsema -= 1, 该方法所在的线程会继续往下执行</li>
<li>如果dsema = 0, 则该方法所在线程会阻塞. 

<ul>
<li>在timeout之前如果dsema自增1(也就是遇到方法2: dispatch_semaphore_signal), 则dsema -= 1, 该方法所在的线程会继续往下执行. </li>
<li>如果过了timeout, 线程还是会继续往下执行</li>
</ul></li>
</ul></li>
</ul>

<h3>返回值</h3>

<ul>
<li>dispatch&#95;semaphore&#95;singal的返回值为long:

<ul>
<li>返回0: 表示当前没有线程在等待你这个信号量=.=</li>
<li>返回不是0: 则表示有线程在等你的信号量, 并解除一个wait. (唤醒优先级最高的线程那个wait=.=, 否则随机唤醒)</li>
</ul></li>
<li>dispatch&#95;semaphore_wait的返回值为long:

<ul>
<li>返回0: 表示在timeout之前被唤醒</li>
<li>返回不是0: 表示timeout发生</li>
</ul></li>
</ul>

<blockquote>
  如何设置timeout: (dispatch&#95;semephore&#95;t)
  
  <blockquote>
    可以使用宏: <code>DISPATCH_TIME_NOW</code>, <code>DISPATCH_TIME_FOREVER</code>
    或者使用dispatch_time创建: 
    <code>dispatch_time_t dispatch_time(dispatch_time_t when, int64_t delta)；</code>
    例如: <code>dispatch_time_t  t = dispatch_time(DISPATCH_TIME_NOW, 1*1000*1000*1000);</code>
    表示当前时间后延一秒为timeout
  </blockquote>
</blockquote>

<hr />

<h2>生动形象的例子</h2>

dispatch_semaphore好比如停车场:

<ul>
<li>一共有4个车位: signal = dispatch&#95;semaphore&#95;create(4)</li>
<li>来了一个车子: dispatch&#95;semaphore&#95;wait(singal)

<ul>
<li>如果都停满了就要等, 超过timeout就走=.= </li>
</ul></li>
<li>走了一个车子: dispatch&#95;semaphore&#95;signal(signal)

<ul>
<li>走了一辆就多一个坑=.=</li>
</ul></li>
</ul>