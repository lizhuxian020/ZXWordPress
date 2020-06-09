# KVC
当赋值基础数据变量时: int, float, double, bool
可以直接用NSString赋值
```c
@interface Person:NSObject
@property(nonatomic, assign)int age
@end

[person setValue:@"123" forKeyPath:@"age"];
```
以上这样子也能正常赋值

#KVO
常规使用方法:
```
//对象必须强引用, 在结束监听之前, 不能被释放
Person *person = [Person new]
[person addObserver:target forKeyPath:keyPath option:NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld context: nil];


//然后在target.m里实现方法
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey, id> *)change context:(void *)context {
    
 }
```

##KVO监听的target不会强应用(已验证)
* 在A_VC里push一个B_VC
* 在push之前, 当B_VC监听A_VC的成员变量
* push之后, pop回来, B_VC成功被dealloc
