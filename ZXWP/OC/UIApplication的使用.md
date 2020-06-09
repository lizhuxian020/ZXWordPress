# UIApplication的使用

##对App退出到后台的几种测试:
* 开启死循环打印
    * 退出到后台后还是不断的打印, 即使杀掉APP, 也还是会打印, 只能卸载APP
* 开启Timer, 5秒后打印
    * 退出到后台, 5秒后没打印, 但一回到APP. 马上打印

## openURL
使用URL: 统一资源定位符, 协议://IP/路径
可以打电话, 发短信, 发邮件, 打开网页, 打开其他APP
对协议的理解: 协议是指双方互相达成的逻辑, 根据这个规则来执行固定的逻辑. 比如网络协议, HTTP: 协议是向服务器请求HTML文件, 拿到文件来解析并显示内容. SSL安全套接层, 表示在传输层和应用层之前添加一层安全传输协议, 验证双方分身信息等. TCP/IP等等.

打电话: tel://10086
发短信: sms://10086
发邮件: mailto://12345@qq.com
打开浏览器: http://www.baidu.com
打开其他APP: ..

##UIWindow
* 在UIApplicationDelegate里, 存在一个window的Property属性, 是希望AppDelegate有个强引用引着他, 如果没有成员变量引着, keyWindow则不会被显示出来(即使他还存在, 未释放)
* UIApplication.shareApplication.windows.count, 这个值, 每次UIWindow alloc init都会+1
* makeKeyAndVisible, 用于展示界面(需与第一个条同时达成才会显示), 并被强引用(应该是被Application引用)

##APP启动流程
1. 执行Main函数, 执行UIApplicationMain方法
2. 创建UIApplication对象, ApplicationDelegate对象, 并赋值给UIApplication对象的delegate属性
3. 开启事件循环, 开启Run Loop
4. 加载Info.plist, 看是否有默认StoryBoard, 有就加载![](media/15833302546445.jpg)
    1. 加载SB, 创建UIWindow, 使用SB创建VC, 赋值给Window.rootVC, makeKeyAndVisible
        1. 将vc的view添加到UIWindow上面去
