# 总结: 
1. 创建bridge做了以下几件事情
    1. 赋值成员变量: Delegate, bundleURL, moduleProvider, launchOption
    2. 初始化Log(未探索)
    3. 创建batchedBridge(CxxBridge)
        1. 赋值成员变量: parentBridge, pendingCalls, moduleDataByName, moduleDataByID, moduleClassesByID
    4. 调用batchedBridge.start方法, 
        1. 发送”RCTJavaScriptWillStartLoadingNotification”通知
        2. 建立JSThread, 名为com.facebook.react.JavaScript, 并启动RunLoop运行
        3. 注册ExtraModule
            1. 把delegate里声明的extraModule实例, 创建ModuleData, 根据ModuleName赋值给三大队列
        4. 注册RCTModuleClasses
            1. 遍历ModuleClasses里元素, 转化成ModuleData(带moduleProvider, 暂无实例)
            2. 把moduleData赋值给三大队列
            3. 之后遍历moduleDataByID, 逐个调用moduleData.instance方法
                1. 没instance的调用moduleProvider创建instance
                2. 给每个instance赋值bridge(CxxBridge)
                3. 给每个instance赋值methodQueue
                    1. instance有methodQueue实现的话, 则使用方法实现,
                    2. 若无实现, 则默认创建GCD串行队列, 名为com.facebook.react.${moduleName}Queue, 
        5. CxxBridge.loadSource, 开始加载JSBundle资源
            1. 发送通知”RCTBridgeWillDownloadScriptNotification”
            2. 使用RCTJavaScriptLoader.loadBundleAtURL方法加载资源
            3. 使用RCTDevLoadingView展示加载进度
        6. 拿到资源data, 调用CxxBridge.executeSourceCode方法执行
            1. 发送通知”RCTJavaScriptWillStartExecutingNotification”
2. 创建好bridge之后, 就开始创建RCTRootView了.


RCTBridge
-> delegate //bridgeDelegate
-> bundleURL
-> moduleProvider
-> launchOption
-> batchedBridge //CxxBridge
-> Static NSMutableArray<Class<RCTBridgeModule>> RCTModuleClass // 存储注册BridgeModule的class
-> void RCTRegisterModule(Class moduleClass) //注册BridgeModule
-> void RCTBridgeModuleNameForClass(Class moduleClass) //获取moduleClass的moduleName (会除去RCT前缀, 返回+moduleName方法结果)



CxxBridge : RCTBridge
-> parentBridge: RCTBridge
-> Delegate //创建bridge传入的delegate
-> bundleURL 
-> moduleProvider: //属性, 用于根据moduleClass创建moduleInstance的, 用于提供extraModule, 新方法是Delegate里的extraModulesForBridge, 这里提示最好每次创建新的module, 不要每次返回同样的module
-> _moduleDataByName  // <moduleName: moduleData>
-> _moduleClassByID // [moduleClass]
-> _moduleDataByID // [ModuleData]



RCTModuleData
-> _bridge(isRequired) //CxxBridge,
-> instance // module实例, id<RCTBridgeModule>
-> moduleClass(isRequired) //module.class
-> _moduleProvider //用于创建moduleInstance的block
-> -(void)setUp // 方法主要用于判断是否在mainQueue上面setup
    1. 判断是否在mainQueue上面setup
        1. 若没重写requiresMainQueueSetup方法则以下方法获取
        2. customInit(重写了init方法)   ||   重写了constantsToExport方法   ==>则使用mainQueue
-> methodQueue // 方法执行队列, 如果BridgeModule不实现methodQueue的话, 默认给每一个module分配一个serialQueue, 名字是com.facebook.react.${moduleName}Queue, 
PS:


RCTRootView
-> _bridge //RCTBridge
-> _moduleName // JS代码里注册的名字
-> appProperties // 附加属性