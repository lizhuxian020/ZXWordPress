
```
//person.m
static int haha = 1;
int hehe = 2;

//ViewController.m
#import "person.h"

extern int hehe; //2
extern int haha; //编译报错, 找不到haha
```
通过上述调用, vc.m是可以获取到hehe的.
但是找不到haha, 因为static只能在person.m文件里用.