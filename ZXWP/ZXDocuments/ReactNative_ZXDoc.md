# Animated
##使用Animated有几个步骤

* 初始化Animated值
    * 例如: `this.state.valua = new Animated.Value(0)`
* 需要执行动画的View用AnimatedView包起来, 并把关键属性用AnimatedValue
    * 例如: `<Animated.View style={this.state.value}/>`
* 到某个逻辑执行动画, 使用Animated.timing | Animated.spring | Animated.decay
    * 这里返回的是一个Animated对象, 需要执行start()才会执行动画


##interpolate方法
这是在第二步的时候, 根据animatedValue来进行区间判断和赋值的方法, 如: 

```
style={this.state.value.interpolate({
    inputRange: [0, 1],
    outputRange: [0, 100]
})
```
普通的`style={this.state.value}`就是把animatedValue的值赋给style, 上述那种做法是, 当Value在0~1区间变化的时候, 对应输出的是0~100的值.


##PS
```
style={[styles.content, {transform: [

              {translateX: this.state.translateValue.interpolate({
                  inputRange: [0, 1],
                  outputRange: [0, 100],
                })},
              {translateY: this.state.translateValue.interpolate({
                  inputRange: [0, 1],
                  outputRange:[0, 100]
                })},

              {scale: this.state.translateValue.interpolate({
                  inputRange: [0, 1],
                  outputRange: [1, 3],
                })},
              {rotate: this.state.translateValue.interpolate({
                  inputRange: [0, 1],
                  outputRange: [
                    '0deg', '720deg'
                  ],
                })},

            ]}]}
```
在上述代码里, translateX, translateYY, scale, rotate的先后顺序会影响动画效果!


#通知

##RN内部接收通知

```JavaScript
import {DeviceEventEmitter} from 'react-native'

//监听
DeviceEventEmitter.addListener(name, (params)=>{...})

//发送
DeviceEventEmitter.emit(name, params)

```
* 这里DeviceEventEmitter是JS的<font color='red'>RCTDeviceEventEmitter</font>类, 这个类继承自<font color='red'>EventEmitter</font>类

##RN接收native的通知(SmarkSpeaker做法)

* Native的RN类, 继承RCTEventEmitter并实现RCTBridgeModule协议
    * 所以在RN里可以这么导入, `let event = new NativeEventEmitter(NativeModules.RN);`
        * 这里拿到NativeModule作为入参, 创建了一个NativeEventEmitter类的实例: event
    
* 接着在RN的subscribe里, 给Native的RN类的supportEvent加入key值.
* 接着在RN里调用`event.addListener(key, callback)`
    * 因为event是JS的<font color='red'>NativeEventEmitter</font>类的实例, 所以先走JS逻辑, 如下:
        * 如果有NativeModule, 则调NativeModule的addListener方法
        * 之后return super.addListener, 这个JS类的父类是<font color='red'>EventEmitter</font>
        * <font color='red'>所以这里监听了一个RN的通知!</font>
    * 然后这里调用了Native里RCTEventEmitter的addListener方法
        * 因为RCTEventEmitter也是一个RCTBridgeModule, addListener也是通过RCT_EXPORT_METHOD宏来暴露出去的
    
* 在RCTEventEmitter的addListener里面逻辑如下
    * 在RNDebug状态下, 如果Key值不在supportedEvents里,则return
     
     > (一般到调这里时候, 在RN代码里已经把key加入到这个supportedEvent里了)
     
    * _listenerCount自增1, 如果_listenerCount==1,则调startObserving, 之后如果再监听的话, _listenerCount就大于1, 则不会再调startObserving方法.
    * 这里的startObserving, 父类默认不实现, 需要子类实现, 所以RN作为子类, 在这里监听了Native通知, 名RNManagerSendEventNotification
* 当Native发起通知, 名为RNManagerSendEventNotification, 则走Native回调, 逻辑如下:
    * 拿到通知的Info, 取name的值, 如果name为nil则return
    * 拿到name的值, 判断supportEvent是否有, 没有则加进去
    * 在当前线程队列里加入发送通知的事件, 调用RCTEventEmitter的方法`-(void)sentEventWithName:body:`
    
        > 这个方法实际上也是调用JS源码里的发送通知的类: <font color='red'>RCTDeviceEventEmitter</font>
        > 由此可以得出, Native实际上也是通过JS的类来给RN发送通知
* 以上完成了监听, 总结: 
    * 在Native注册module, 在RN发起监听addListener, 同时在RN监听了对应的通知.
    * 在native发送RNManagerSendEventNotification通知, RN来订阅这个通知, 拿到通知里的info, 再使用RCT框架来发送react_native的通知.

## ReactNative加入Toast
* 自己通过Component和Animated来实现Toast组件
* 在RN目录的index.js里, 自己写一个页面用

```html
<View style={flex:1}>
    <App/>
    <Toast ref=global.toast=ref/>
</View>
```
* 直接在页面用`global.toast.show(message)`即可

> 可以自己写个Util来showToast
> 同理可以实现Loading
    


