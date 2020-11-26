# 解决scrollView与屏幕边界
一般的scrollView的panGesture会与边界的手势冲突
获取边界手势的方法
1. UINavigationController.interactivePopGestureRecognizer
2. 或者如下
````
NSArray *arrayGesture = vc.navigationController.view.gestureRecognizers;
    for (UIGestureRecognizer *gesture in arrayGesture) {
        if ([gesture isKindOfClass:[UIScreenEdgePanGestureRecognizer class]]) {
            [scrollView.panGestureRecognizer requireGestureRecognizerToFail:gesture];
            break;
        }
    }
````

然后执行这个方法
`[scrollView.panGestureRecognizer requireGestureRecognizerToFail:gesture]`

# 解决使用NestTableView问题
当contentView的contentSize小于自身bounds时, 滚动时触发不了contentView的scrollViewDidScroll方法, 只有contentSize大于bounds时才会触发
解决:
1. 使用MFPageView包一层, 即使用collectionView包一层, numPage = 1, view返回contentView

原因
1. 待解(=.=, 也不知道什么时候会解, 记录今天日期: 2020年9月1日)