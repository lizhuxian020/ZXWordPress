---
ID: 35
post_title: EmptyXcode到RN引入
author: lizhuxian020@gmail.com
post_excerpt: ""
layout: post
permalink: https://leezix13.com/wordpress/?p=35
published: true
post_date: 2017-07-21 08:06:11
---
# 从新建Xcode空项目到引入RN模块(含GIT)

> 前提条件:  
> 已经安装cocoaPods, (我的是1.2.1)  
> 已经安装node, 可以使用npm指令  
> 安装Xcode(=.=) 大致流程:  
> 新建Xcode空项目 -> 引入Native子模块(GIT) -> 导入Native子模块的代码(pod) -> 新建react_native仓库 -> 把react_native当做子模块引入到主工程(GIT) -> 初始化npm(新建package.json) -> npm install -> 修改Podfile, 引入RN所需Native库 -> build通Xcode -> 在react_native模块里新建index.ios.js, 并编写RN代码 -> 在Xcode工程引入RN创建的view, 并显示. 
## 新建Xcode空项目 从GitHub新建你的XcodeProject的仓库, 得到地址: 

`https://github.com/lizhuxian020/XcodeProject.git` ![github_emptyXcode][1] 在本地找个地方, 把这个仓库clone下来(此时是个空仓库, 以下称仓库为repo) 在这个空repo里新建Xcode空项目, 并push, 得到一个在远程的空XcodeProject, 此时工程目录如下: ![local-emptyXcodePath][2] 
## 引入Native子模块(GIT)

> 这里我引入我自己创建的Native库: ZXConvenientCode, 地址: https://github.com/lizhuxian020/ZXConvenientCode.git  在主工程的.git目录下, 使用命令 `git submodule add --force https://github.com/lizhuxian020/ZXConvenientCode.git ZXConvenientCode` 便得到一个目录, 如下: ![local-emptyXcodePath][3] ![][4] 
## 导入Native子模块的代码(pod) cd到XcodeProject.xcodeproj目录下, 执行

`pod init`, 然后编写podfile, 添加这么一句: `pod 'ZXConvenientCode', :path=>'../ZXConvenientCode'` ![podfile-addZXConvenientCode][5] 然后在podfile目录下`pod install`  
这样完成把ZXConvenientCode这个库的代码引入工程了. 
## 新建react_native仓库 自己去GitHub上新建react_native仓库去, 空仓库即可, 得到地址

`https://github.com/lizhuxian020/react_native.git` 
## 把react_native当做子模块引入到主工程(GIT) 把你刚刚新建的react_native仓库, 通过git submodule add引入进主工程, 哈? 你不会, 刚刚不是教你了吗..cd到主工程.git目录下, 

`git submodule add --force https://github.com/lizhuxian020/react_native.git react_native`就好啦=.= 
## 初始化npm(新建package.json) 这里解释一下package.json的作用:

  
这是个描述性文件, 记录你这个react_native的模块的信息, 里面有名称, 版本号, 依赖库, 仓库地址等等信息, 具体你cd到react_native目录, 然后执行`npm init`就知道啦. 反正我得到的package.json如下: ![package-content][6] 最后npm init完之后, 你通过自己手动修改, 添加一下dependencies吧. 待会npm install会根据这个来加载的RN依赖库的. 注意! 
> 这里有个坑  
> 这里主要添加react-native的依赖库, 但是rn的库又依赖react的库, 所以react的库也要顺便添加, 但是操蛋的事, 不同版本react-native要依赖不同版本的react, 但是具体哪个rn版本依赖哪个react版本, 却没有说明表, 我就日了 ![reactnative-package][7] 所以, 只能按官网的图片给的来, 那肯定没错的 [react-native中文网: RN嵌入到原生应用][8] 以前老版的RN0.24.0也有文档说, RN植入原生应用的, 太老了, 当然要使用更新的啦, 这个是0.44.0的文档, 现在RN最新的是0.46.0  
但我们本次使用的RN版本是0.39.2 
## npm install 你搞定了package.json, 接下来就可以

`npm install`了, 不知道你们install得快不快, 反正我是很快, 别问我为什么. 
> 这里教你看你装的RN是什么版本的, cd到~/react_native/node_modules/react-native, 查看package.json, 最下面的version就是啦.  如果你怎么装都是最新版的0.46.0或者怎么装都不是你react_native的package指定的版本, 那可能是因为你react_native下的package-lock.json, 不知道这是什么东西, 反正是个lock, 就像你的podfile.lock一样, 删掉他, 再npm install就好了吧=.= 
## 修改Podfile, 引入RN所需Native库 最后的坑 在podfile文件里, 加入 

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">pod 'React', :path =&gt; '../node_modules/react-native/React.podspec', :subspecs =&gt; [
    'Core',
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', # 这个模块是用于调试功能的
    # 在这里继续添加你所需要的模块
  ]
</code></pre> 如果你能成功pod install, 那恭喜你. 直接忽略一下, 到下一步. 我就是在这里被坑了好久, 老是报错, 如下: 

![pod-install-error][9] 
> (意思大概是: 是跟yoga有关的, 根据react-native里的react.podspec里写的, 依赖yoga这个库, 然后版本跟rn版本一样, 然后rn是0.46.4, 所以报错, 说找不到yoga版本0.46.4的podspec)  因为我没搞react_native里的package里的dependencies, 直接npm init然后一直回车, 之后直接执行 <pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">npm install react
npm install react-native
</code></pre> 坑比啊, 直接给我最新的react和react-native, 然后回到工程怎么pod install都报错. .. 最后才给我发现, 官方里的这句话, 如果rn版本 >= 0.42.0要在podfile里加多一句: 

`pod "Yoga", :path => "../node_modules/react-native/ReactCommon/yoga"` ![pod-install-success][10] 
## build通Xcode 如果你在上一步pod install成功了, 就打开你的Xcode, 如果你又能build成功, 那....直接下一步吧T_T..我一般会遇到这个错误 

![Xcode-error][11] 能有什么办法, 回想一下刚刚是不是出错了=.=, 你又看不懂Facebook的东西. 想了好久, 发现: 没错啊!!! 那只能怪, 这个最新版的react-Native和最新版的react不匹配呗. 然后我修改package.json, 把dependencies的react改为15.4.1, react-native改为0.39.2, 然后删掉package-lock.json, 删掉node_modules, 然后重新npm install, 回到主工程, 注释掉pod yoga那句, 然后pod install. 然后再build一下, 就成功了!=.= 
## 在react_native模块里新建index.ios.js, 并编写RN代码 好啦, 已经完成99%了, 最后写上一个RN代码 在react_native目录下, 新建文件: index.ios.js, 把以下代码烤进去! 这里是写一个RN组件, 然后给Native代码用 

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">/**
 * Created by lzx on 2017/7/21.
 */

import React, {Component} from 'react'
import {
    AppRegistry,
    StyleSheet,
    View,
    Text
} from 'react-native'

class App extends Component {

    render(){
        return(
            &lt;View style = {{borderWidth: 1}}&gt;
                &lt;Text style = {{marginTop: 30, marginLeft: 30}}&gt;wocao&lt;/Text&gt;
            &lt;/View&gt;
        )
    }
}

AppRegistry.registerComponent('App', () =&gt; App);


</code></pre>

## 在Xcode工程引入RN创建的view, 并显示. 在APPDelegate.m里, 这么写! 

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">//
//  AppDelegate.m
//  XcodeProject
//
//  Created by lzx on 2017/7/21.
//  Copyright © 2017年 lzx. All rights reserved.
//

#import "AppDelegate.h"
#import "ViewController.h"
#import &lt;React/RCTRootView.h&gt;

@interface AppDelegate ()&lt;RCTBridgeDelegate&gt;

@end

@implementation AppDelegate


- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.

    self.window = [[UIWindow alloc] init];

    UIViewController *vc = [[ViewController alloc] init];
    RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
    RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge moduleName:@"App" initialProperties:nil];
    rootView.frame = [UIScreen mainScreen].bounds;
    vc.view = rootView;

    self.window.rootViewController = vc;
    [self.window makeKeyWindow];


    return YES;
}


#pragma mark -- RCTBridgeDelegate
- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge{
    //不知道localhost行不行, 不行就ip地址
    NSURL *jsCodeLocation = [NSURL URLWithString:@"http://10.180.185.3:8081/index.ios.bundle?platform=ios"];
    return jsCodeLocation;

}



@end


</code></pre> 然后cmd + r 跑起来~~~ 然后报错~ 

![Xcode-run-error][12] 你要cd到react_native, 执行`npm start`, 然后点击红屏下面的Reload JS, 就可以看到啦 ![Xcode-run-success][13] 完成, 谢谢

 [1]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/github-emptyXcode-1.png
 [2]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/local-emptyXcodePath-1.png
 [3]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/local-addZXConvenientCode-1.png
 [4]: /Users/lzx/Documents/MacDown/EmptyXcode到RN引入/.png
 [5]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/podfile-addZXConvenientCode.png
 [6]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/package-content-1.png
 [7]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/reactnative-package.png
 [8]: https://reactnative.cn/docs/0.44/integration-with-existing-apps.html
 [9]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/pod-install-error-1.png
 [10]: /Users/lzx/Documents/MacDown/EmptyXcode到RN引入/pod-install-success.png
 [11]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/Xcode-error.png
 [12]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/Xcode-run-error.png
 [13]: https://leezix13.com/wordpress/wp-content/uploads/2017/07/Xcode-run-success.png