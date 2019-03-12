# RCTBridgeModule协议
* 声明了宏RCT_EXPORT_MODULE()
    * 宏默认实现了moduleName方法
    * 实现了load方法, 并执行了C方法: RCTRegisterModule(self)
    * RCTRegisterModule这个方法在RCTBridge实现, 维护了一个静态数组, 把所有实现RCT_EXPORT_MODULE宏的类都收集起来.

# RCTModuleData
* 每个实现了RCTBridgeModule的类都会被转化成RCTModuleData, 里面有维护了一个_instance, 就是RCTBridgeModule的实例, 在react_native代码里, 通过NativeModule点出来的实例都是这个_instance
* 所以每个RCTBridgeModule都是一个单例!    
    

# React_Native启动


