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
<h1>从新建Xcode空项目到引入RN模块(含GIT)</h1>

<blockquote>
  前提条件:<br />
  已经安装cocoaPods, (我的是1.2.1)<br />
  已经安装node, 可以使用npm指令<br />
  安装Xcode(=.=)
  
  大致流程:<br />
  新建Xcode空项目 -> 引入Native子模块(GIT) -> 导入Native子模块的代码(pod) -> 新建react_native仓库 -> 把react_native当做子模块引入到主工程(GIT) -> 初始化npm(新建package.json) -> npm install -> 修改Podfile, 引入RN所需Native库 -> build通Xcode -> 在react_native模块里新建index.ios.js, 并编写RN代码 -> 在Xcode工程引入RN创建的view, 并显示.
</blockquote>

<h2>新建Xcode空项目</h2>

从GitHub新建你的XcodeProject的仓库, 得到地址: <code>https://github.com/lizhuxian020/XcodeProject.git</code>
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/github-emptyXcode-1.png" alt="github_emptyXcode" />

在本地找个地方, 把这个仓库clone下来(此时是个空仓库, 以下称仓库为repo)

在这个空repo里新建Xcode空项目, 并push, 得到一个在远程的空XcodeProject, 此时工程目录如下: 
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/local-emptyXcodePath-1.png" alt="local-emptyXcodePath" />

<h2>引入Native子模块(GIT)</h2>

<blockquote>
  这里我引入我自己创建的Native库: ZXConvenientCode, 地址: https://github.com/lizhuxian020/ZXConvenientCode.git
</blockquote>

在主工程的.git目录下, 使用命令 <code>git submodule add --force https://github.com/lizhuxian020/ZXConvenientCode.git ZXConvenientCode</code> 
便得到一个目录, 如下: 
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/local-addZXConvenientCode-1.png" alt="local-emptyXcodePath" />

<img src="/Users/lzx/Documents/MacDown/EmptyXcode到RN引入/.png" alt="" />

<h2>导入Native子模块的代码(pod)</h2>

cd到XcodeProject.xcodeproj目录下, 执行<code>pod init</code>, 然后编写podfile, 添加这么一句: <code>pod 'ZXConvenientCode', :path=&gt;'../ZXConvenientCode'</code>
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/podfile-addZXConvenientCode.png" alt="podfile-addZXConvenientCode" />

然后在podfile目录下<code>pod install</code><br />
这样完成把ZXConvenientCode这个库的代码引入工程了.

<h2>新建react_native仓库</h2>

自己去GitHub上新建react_native仓库去, 空仓库即可, 得到地址<code>https://github.com/lizhuxian020/react_native.git</code>

<h2>把react_native当做子模块引入到主工程(GIT)</h2>

把你刚刚新建的react_native仓库, 通过git submodule add引入进主工程, 哈? 你不会, 刚刚不是教你了吗..cd到主工程.git目录下, <code>git submodule add --force https://github.com/lizhuxian020/react_native.git react_native</code>就好啦=.=

<h2>初始化npm(新建package.json)</h2>

这里解释一下package.json的作用:<br />
这是个描述性文件, 记录你这个react&#95;native的模块的信息, 里面有名称, 版本号, 依赖库, 仓库地址等等信息, 具体你cd到react_native目录, 然后执行<code>npm init</code>就知道啦. 反正我得到的package.json如下: 
<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/package-content-1.png" alt="package-content" />

最后npm init完之后, 你通过自己手动修改, 添加一下dependencies吧. 待会npm install会根据这个来加载的RN依赖库的.

注意!

<blockquote>
  这里有个坑<br />
  这里主要添加react-native的依赖库, 但是rn的库又依赖react的库, 所以react的库也要顺便添加, 但是操蛋的事, 不同版本react-native要依赖不同版本的react, 但是具体哪个rn版本依赖哪个react版本, 却没有说明表, 我就日了
</blockquote>

<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/reactnative-package.png" alt="reactnative-package" />
所以, 只能按官网的图片给的来, 那肯定没错的
<a href="https://reactnative.cn/docs/0.44/integration-with-existing-apps.html">react-native中文网: RN嵌入到原生应用</a>
以前老版的RN0.24.0也有文档说, RN植入原生应用的, 太老了, 当然要使用更新的啦, 这个是0.44.0的文档, 现在RN最新的是0.46.0<br />
但我们本次使用的RN版本是0.39.2

<h2>npm install</h2>

你搞定了package.json, 接下来就可以<code>npm install</code>了, 不知道你们install得快不快, 反正我是很快, 别问我为什么.

<blockquote>
  这里教你看你装的RN是什么版本的, cd到~/react&#95;native/node&#95;modules/react-native, 查看package.json, 最下面的version就是啦.
</blockquote>

如果你怎么装都是最新版的0.46.0或者怎么装都不是你react&#95;native的package指定的版本, 那可能是因为你react_native下的package-lock.json, 不知道这是什么东西, 反正是个lock, 就像你的podfile.lock一样, 删掉他, 再npm install就好了吧=.=

<h2>修改Podfile, 引入RN所需Native库</h2>

最后的坑

在podfile文件里, 加入

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">pod 'React', :path =&gt; '../node_modules/react-native/React.podspec', :subspecs =&gt; [
    'Core',
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', # 这个模块是用于调试功能的
    # 在这里继续添加你所需要的模块
  ]
</code></pre>

如果你能成功pod install, 那恭喜你. 直接忽略一下, 到下一步.

我就是在这里被坑了好久, 老是报错, 如下:

<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/pod-install-error-1.png" alt="pod-install-error" />

<blockquote>
  (意思大概是: 是跟yoga有关的, 根据react-native里的react.podspec里写的, 依赖yoga这个库, 然后版本跟rn版本一样, 然后rn是0.46.4, 所以报错, 说找不到yoga版本0.46.4的podspec)
</blockquote>

因为我没搞react_native里的package里的dependencies, 直接npm init然后一直回车, 之后直接执行

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-null">npm install react
npm install react-native
</code></pre>

坑比啊, 直接给我最新的react和react-native, 然后回到工程怎么pod install都报错. ..

最后才给我发现, 官方里的这句话, 如果rn版本 >= 0.42.0要在podfile里加多一句: 
<code>pod "Yoga", :path =&gt; "../node_modules/react-native/ReactCommon/yoga"</code>

<img src="/Users/lzx/Documents/MacDown/EmptyXcode到RN引入/pod-install-success.png" alt="pod-install-success" />

<h2>build通Xcode</h2>

如果你在上一步pod install成功了, 就打开你的Xcode, 如果你又能build成功, 那....直接下一步吧T_T..我一般会遇到这个错误

<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/Xcode-error.png" alt="Xcode-error" />

能有什么办法, 回想一下刚刚是不是出错了=.=, 你又看不懂Facebook的东西. 想了好久, 发现: 没错啊!!!

那只能怪, 这个最新版的react-Native和最新版的react不匹配呗.

然后我修改package.json, 把dependencies的react改为15.4.1, react-native改为0.39.2, 然后删掉package-lock.json, 删掉node_modules, 然后重新npm install, 回到主工程, 注释掉pod yoga那句, 然后pod install. 然后再build一下, 就成功了!=.=

<h2>在react_native模块里新建index.ios.js, 并编写RN代码</h2>

好啦, 已经完成99%了, 最后写上一个RN代码

在react_native目录下, 新建文件: index.ios.js, 把以下代码烤进去! 这里是写一个RN组件, 然后给Native代码用

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

<h2>在Xcode工程引入RN创建的view, 并显示.</h2>

在APPDelegate.m里, 这么写!

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


</code></pre>

然后cmd + r 跑起来~~~
然后报错~

<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/Xcode-run-error.png" alt="Xcode-run-error" />

你要cd到react_native, 执行<code>npm start</code>, 
然后点击红屏下面的Reload JS, 就可以看到啦

<img src="https://leezix13.com/wordpress/wp-content/uploads/2017/07/Xcode-run-success.png" alt="Xcode-run-success" />

完成, 谢谢