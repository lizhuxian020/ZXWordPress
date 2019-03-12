# RN与H5交互Bridge
##react-native-webview-bridge原理
Demo:

```js
//导入
var WebViewBridge = require('react-native-webview-bridge');
//声明嵌入式JS
const injectScript = `
  (function () {
    if (WebViewBridge) {

      WebViewBridge.onMessage = function (message) {
        if (message === "hello from react-native") {
          WebViewBridge.send("got the message inside webview");
        }
      };

      WebViewBridge.send("hello from webview");
    }
  }());
`;

//实例方法
onBridgeMessage(message) {
    switch (message) {
      case "hello from webview":
        this.webview.sendToBridge("hello from react-native");
        break;
      case "got the message inside webview":
        console.warn("we have got a message from webview! yeah");
        break;
    }
  }

//渲染
  render() {
    return (
          <WebViewBridge
              ref="webviewbridge"
              onBridgeMessage={this.onBridgeMessage.bind(this)}
              injectedJavaScript={injectScript}
              source={{uri: "https://google.com"}}/>
    );
  }

```
###导入
* 在react-native-webview-bridge的package.json里有` "main": "webview-bridge"`这个键值对, 表明主目录在`webview-bridge`里
* 所以require会引用这个文件夹里的index.js

###index.ios.js
* render使用了RCTWebViewBridge
    * 这个是原生组件, 是通过以下方式导入的.
    
    ```js
    var RCTWebViewBridge = requireNativeComponent('RCTWebViewBridge', WebViewBridge, {
  nativeOnly: {
    onLoadingStart: true,
    onLoadingError: true,
    onLoadingFinish: true,
  },
});
```
    * 同理, 是因为Native端实现了`RCTWebViewBridge: RCTView`这个类, 继承`RCTView`

###RCTWebViewBridge(Native)
* 本身是一个UIView, 加入了一个UIWebView, 自己成为UIWebViewDelegate
* 在`-webViewDidLoad:`这个代理方法里, 执行了一段<font color='red'>关键的JS代码</font>

    ```js
    (function (window) {
      'use strict';

      //Make sure that if WebViewBridge already in scope we don't override it.
      if (window.WebViewBridge) {
        return;
      }

      var RNWBSchema = 'wvb';
      var sendQueue = [];
      var receiveQueue = [];
      var doc = window.document;
      var customEvent = doc.createEvent('Event');

      function callFunc(func, message) {
        if ('function' === typeof func) {
          func(message);
        }
      }

      function signalNative() {
        window.location = RNWBSchema + '://message' + new Date().getTime();
      }

      //I made the private function ugly signiture so user doesn't called them accidently.
      //if you do, then I have nothing to say. :(
      var WebViewBridge = {
        //this function will be called by native side to push a new message
        //to webview.
        __push__: function (message) {
          receiveQueue.push(message);
          //reason I need this setTmeout is to return this function as fast as
          //possible to release the native side thread.
          setTimeout(function () {
            var message = receiveQueue.pop();
            callFunc(WebViewBridge.onMessage, message);
          }, 15); //this magic number is just a random small value. I don't like 0.
        },
        __fetch__: function () {
          //since our sendQueue array only contains string, and our connection to native
          //can only accept string, we need to convert array of strings into single string.
          var messages = JSON.stringify(sendQueue);

          //we make sure that sendQueue is resets
          sendQueue = [];

          //return the messages back to native side.
          return messages;
        },
        //make sure message is string. because only string can be sent to native,
        //if you don't pass it as string, onError function will be called.
        send: function (message) {
          if ('string' !== typeof message) {
            callFunc(WebViewBridge.onError, "message is type '" + typeof message + "', and it needs to be string");
            return;
          }

          //we queue the messages to make sure that native can collects all of them in one shot.
          sendQueue.push(message);
          //signal the objective-c that there is a message in the queue
          signalNative();
        },
        onMessage: null,
        onError: null
      };

      window.WebViewBridge = WebViewBridge;

      //dispatch event
      customEvent.initEvent('WebViewBridge', true, true);
      doc.dispatchEvent(customEvent);
    }(window));
  );
    ```
    * 以上做了一系列事情, 生成`WebViewBridge`对象, 赋值给`Window.WebViewBridge`

###分析调用过程
####H5发送消息
* 当H5页面调用`WebViewBridge.send("hello from webview");`时, 便是发送消息`hello from webview`, 调用send方法
    * 判断message是否为string
    * 把message放入成员变量`sendQueue`里---入栈 
    * 调用`signalNative()`方法
        * 通过`window.location`跳转一个地址
        * wvb://message+TimeStamp 例如:wvb://message12312312123
    * 在WebView代理方法`-webView:shouldStartLoadWithRequest:`
        * 查询到协议头为`wvb`的Request拦截下来
        * 执行JS代码`WebViewBridge.__fetch__()`, 并拿到返回值
            * JSON解析sendQueue
            * 清空sendQueue
            * 返回JSON字符串
        * 拿到返回的JSON字符串, 转成NSArray, 生成{message:NSArray}, 通过onBridgeMessage方法调用RN方法,并传入参数.
* <font color='red'>完成H5发送消息: H5-Native(URL拦截, 获取return值)-调用RN方法(带入return值)</font>

####RN发送消息
* 拿到RN组件的ref, 调用`ref.sendToBridge("hello from react-native")`
* 走到index.ios.js, 调动`WebViewBridgeManager.sendToBridge(this.getWebViewBridgeHandle(), message);`
    * WebViewBridgeManager一个Native暴露的module, RCTWebViewBridgeManager, 这里调用了Native方法sendToBridge
    * 这里通过reactTag拿到RCTWebViewBridge, 然后调用他的sendToBridge
        * 这里调用JS方法,通过NSString拼接, 把message带进代码里
        * 调用JS的, `WebViewBridge.__push__(message)`
* \_\_push\_\_

    ```js
      receiveQueue.push(message);
      //reason I need this setTmeout is to return this function as fast as
      //possible to release the native side thread.
      setTimeout(function () {
        var message = receiveQueue.pop();
        callFunc(WebViewBridge.onMessage, message);
      }, 15); //this magic number is just a random small value. I don't like 0.
    ```
    * 把message入栈到receiveQueue
    * 在异步调用callFunc, 从receiveQueue出栈一个message作为入参
    * 在调用`WebViewBridge.onMessage`
        * 这个方法是在RN里通过injectedJavaScript(嵌入JS)来给WebViewBridge添加的.
* <font color='red'>完成RN发送消息给H5</font>
