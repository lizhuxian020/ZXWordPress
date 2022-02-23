# 给Object全局添加方法
添加__c方法就是这么添加的
```js
for (var method in _customMethods) {
    if (_customMethods.hasOwnProperty(method)) {
      Object.defineProperty(Object.prototype, method, {value: _customMethods[method], configurable:false, enumerable: false})
    }
  }
```
