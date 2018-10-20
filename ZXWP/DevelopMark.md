#Terminal
* MD5命令
    * md5 文件路径(不可以文件夹)
    * md5 -s "需加密字符串"
#开发技巧
* 代码库
    * 选中所需代码, 拖到代码库里
    * 所需要填空的地方, 比如 name 是变量, <#name#>即可
* 
# UI 基础

* 使用[UIImage imgaeName:@""]来加载图片, 会一直存在内存当中!
* CornerRadius的意思如下:
    * ![picName](ImageSet/1.png)




# RunTime
动态字典转模型

* 动态给对象添加方法

```
- (void)viewDidLoad {
    [super viewDidLoad];   
    Person *p = [[Person alloc] init];
    // 默认person，没有实现run:方法，可以通过performSelector调用，但是会报错。
    // 动态添加方法就不会报错
    [p performSelector:@selector(run:) withObject:@10];
}

@implementation Person
// 没有返回值,1个参数
// void,(id,SEL)
void aaa(id self, SEL _cmd, NSNumber *meter) {
    NSLog(@"跑了%@米", meter);
}

// 任何方法默认都有两个隐式参数,self,_cmd（当前方法的方法编号）
// 什么时候调用:只要一个对象调用了一个未实现的方法就会调用这个方法,进行处理
// 作用:动态添加方法,处理未实现
+ (BOOL)resolveInstanceMethod:(SEL)sel
{
    // [NSStringFromSelector(sel) isEqualToString:@"run"];
    if (sel == NSSelectorFromString(@"run:")) {
        // 动态添加run方法
        // class: 给哪个类添加方法
        // SEL: 添加哪个方法，即添加方法的方法编号
        // IMP: 方法实现 => 函数 => 函数入口 => 函数名（添加方法的函数实现（函数地址））
        // type: 方法类型，(返回值+参数类型) v:void @:对象->self :表示SEL->_cmd
        class_addMethod(self, sel, (IMP)aaa, "v@:@");
        return YES;
    }
    return [super resolveInstanceMethod:sel];
}
@end

// 打印输出
2016-02-17 19:05:03.917 runtime[12761:543574] runtime动态添加方法--跑了10米

作者：GitHubUser_CoderLN
链接：https://juejin.im/post/593f77085c497d006ba389f0
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 内存管理
* ARC 下查看 AutoReleasePool:
` _objc_autoreleasePoolPrint `

> 需要在文件里先声明才能使用: 
> `extern void _objc_autoreleasePoolPrint();`
> 



* ARC 下查看 retainCount: 
`_objc_rootRetainCount`

> 需要在文件里先声明才能使用: 
> `extern int _objc_rootRetainCount();`


# 建立自己的pod库
* 使用dependent时, 多个依赖就写多个s.dependent就好
    * 记得加入`s.static_framework = true  //不加这个就报错=.=`
* 使用pod lib create命令来创建自己pod库
* pch文件要先声明在podspec里, 加入`s.prefix_header_file = 'KMTGlobe/Classes/Interface/Macro/Macro.h'`来声明pch文件


