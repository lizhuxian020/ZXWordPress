# MJRefresh

##deprecate注释
```
// 过期提醒
#define MJRefreshDeprecated(instead) NS_DEPRECATED(2_0, 2_0, 2_0, 2_0, instead)

MJRefreshDeprecated("过期了");
```

##要求调用super
```
NS_REQUIRES_SUPER

/** 初始化 */
- (void)prepare NS_REQUIRES_SUPER;
```

##先给UIScrollView写了分类(MJRefresh)
1. 添加mj_header和mj_footer的属性和成员变量
##MJRefreshComponent
1. .h文件定义了
    1. 组件的state(刷新的前后的状态)
    2. 刷新begin和end的block / target&action
2. 重写willMoveToSuperView的方法得到superView, 如果不是UIScrollView则不作为
3. 得到SuperView(ScrollView)之后, 通过KVO监听ScrollView的contentOffset和contentSize, scrollView的panGesture的state属性
    1. .h暴露方法: scrollViewContentOffsetDidChange/ scrollViewContentSizeDidChange/ scrollViewPanStateDidChange方法给子类重写
##实现下滑刷新的动画
1. 核心属性: contentInset
2. 实现scrollViewDelegate, 在endDrag方法里, 判断offset是否到达指定高度(如: 60), 如果到了, 则设置scrollView.contentInset = UIEdgeMake(60,0,0,0)
3. 然后异步执行freshCallback, 结束时使用UIView animate来执行动画`scrollView.contentInset = UIEdgeZero`
4. 如果在刷新过程中, 不允许下拉, 则加个属性, 是否在刷新中, 在scrollDidScroll中实现offset固定住, 不给下拉
5. PS: 如果不设置contentInset, 就算设置offset也没用, 他还是会滚会offset=0的.

###MJ的实现下拉刷新(主要逻辑: MJRefreshHeader:MJRefreshComponent)
1. 全在scrollViewDidScroll里做事情
2. 子类集成MJRefreshComponent, 实现scrollViewContentOffsetDidChange, 来监听ContentOffset属性
    1. 先判断是否在刷新中, 如果是刷新中, 则不允许下拉超过freshHeader, 但允许bounce出去
        1. 通过contentInset实现, 设置好包含的freshHeader的contentInset即可. 
    2. 如果不是刷新中, 判断是否下滚, 则不关fresh事, 如果露出header了, 判断是否(scrollView.isDragging)
        1. 如果是, 则判断offset是否超过header, 是则置statePulling, 否则置stateIdle
        2. 如果不在dragging, 则判断state是否pulling, 是则直接调freshCallback, 否则设置pullingPercent.
3. SetState
    1. 当state=refresh的时候
        1. inset设置为header的高度
        2. 设置offset=-header.height(让刚好滚到显示完全header)
        3. 以上动画执行完后,执行freshCallback
    2. 当freshCallback执行完后, 调用方主动执行endFresh, 把state置回idle
    3. 当state=idle的时候
        1. 把当前insetTop+=insetTopDelta(这里的insetTopDelta意思是恢复原样的值, 在scrollViewDidScroll的时候记录的, 当state变成fresh的时候)
        2. 当inset恢复原样的时候, scrollView的机制, 自然会恢复原来的offset.
        3. 以上2点都是在UIView animate里实现, 才会有动画效果.
        4. 动画结束后执行endRefreshCompleteBlock
    
##view.window
这个属性是当view出现在屏幕的时候才会有值. 
自己的实验: 
1. 在viewDidLoad的时候, 就算加入subView了, 也不会马上出现. 因为屏幕还没出现. =.=
2. 在TouchBegin的时候, view.window有值, 马上removeFromSuperView后, 马上没值了.


##willMoveToSuperView里
1. 记录originScrollInset

##在scrollViewDidScroll中设置contentInset(实现scrollView上下无限滚动)
如果在scrollViewDidScroll中设置contentInset = UIEdgeMake(-offset.y,0,0,0)
然后scrollView.alwaysBounceVertical = YES, 则实现了scrollView上下无限滚动