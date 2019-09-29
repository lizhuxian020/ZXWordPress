# CodePush
```JS
switch (status) {
      case CodePush.SyncStatus.CHECKING_FOR_UPDATE:
        msg = '正在检查更新...';break; //调用检查接口时候
      case CodePush.SyncStatus.DOWNLOADING_PACKAGE:
        msg = '正在下载更新包...';break; //第一次下载更新包的时候
      case CodePush.SyncStatus.INSTALLING_UPDATE:
        msg = '正在安装更新...';break; //自动更新的时候
      case CodePush.SyncStatus.SYNC_IN_PROGRESS:
        msg = '正在同步...';break; //正在下载, 然后多次调用sync时候
      case CodePush.SyncStatus.UNKNOWN_ERROR:
        msg = '未知错误...';break; // 网络错误
      case CodePush.SyncStatus.UP_TO_DATE:
        msg = '已经是最新的了...';break; //更新完之后还点sync
      case CodePush.SyncStatus.UPDATE_INSTALLED:
        msg = '更新已经安装完毕...';break;
    }
```

#如何接入CodePush
>具体如何接入, 还是得参考[微软文档](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/react-native)

1. `npm install react-native-code-push --save`
2. `react-native link react-native-code-push`
    1. 这个会帮你完成配置iOS/Android的配置, 例如在podfile文件加入CodePush, 在info.plist加入DeploymentKey
    2. 另外还会改写APPDelegate获取BundleURL的逻辑, 若想在debug状态下测试CodePush, 一定要使用CodePush BundleURL, 否则没效果. (reload之后还是本地代码=.=)
    3. 但是要手动Pod install一次
3. 在RN项目中, AppRegister的container, 要使用CodePush包一层.
    1. `CodePush(APPContainer)`
    2. 另外可以加入CodePushOption: `CodePush(CodePushOption)(APPContainer)`

    
### CodePushOption extend SyncOption 
=.=具体有什么, 我也不想写了, 自己看代码吧..

