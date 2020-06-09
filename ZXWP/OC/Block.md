# Block
__block int i = 0;
会被编译成一个数据结构

\__NSStackBlock__
```
int i = 0;
__unsafe_unretained void (^b)(void) = ^{
    NSLog(@"%d", i);
};
NSLog(@"%@", b);
```
1. 如果没有__unsafe_unretained, 那block就是MallocBlock, 因为默认__strong修饰符
