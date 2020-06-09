# CGPoint&&CGRect
> 本文收集CGPoint和CGRect的常见操作方法

##- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event 与 CGRectContainsPoint(CGRect rect, CGPoint point)的区别
* 前者是UIView的方法, 一般用于子View重写该方法, 当触摸事件发生时, 该point属不属于self自身view里面
* 后者只是纯粹的判断, 一个point是否在一个rect里面. 大多数重写前者的时候会用到后者.
* 扩大View的响应范围

```c
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event
{
    if (CGRectContainsPoint(CGRectInset(self.bounds, -20, -20), point)) {
        return YES;
    }
    return NO;
}
```

* 当然, `pointInside`方法不重写也是可以直接用的, 默认实现应该就是跟CGRectContainsPoint(self.bounds, point)一样=.=

##- (CGPoint)convertPoint:(CGPoint)point toView:(nullable UIView *)view;
* UIView的方法
* point来自self的坐标, 转化成以targetView为原点的point, 并返回结果

##- (CGPoint)convertPoint:(CGPoint)point fromView:(nullable UIView *)view;
* UIView的方法
* 入参point是以targetView为原点的, 要转化成以self为原点的point

##- (CGRect)convertRect:(CGRect)rect toView:(nullable UIView *)view;
* UIView方法
* 入参Rect是以self为原点的, 要转化成targetView为原点的rect

##- (CGRect)convertRect:(CGRect)rect fromView:(nullable UIView *)view;
* UIView方法
* 入参rect是以targetView为原点的, 要转化成self为原点的rect
