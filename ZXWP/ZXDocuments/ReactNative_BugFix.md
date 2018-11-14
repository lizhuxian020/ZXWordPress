# ReactNative

##ios端alert会与modal冲突
###表现现状
在iOS端, 在一次RN逻辑过程中, 如果使用alert, 之后有渲染出modal组件, 则要看先后顺序, 先弹alert, 则modal会被覆盖, 先弹modal, 则alert就弹不出来.
###原因
虽然alert和modal使用不同的组件, 但是都是在native端, 通过presentController来展示的, 所以会有个先后顺序, 后面present的就不会出来的, 这里不管是否是用VC来present还是NavigationController来present.
###附加
alert的实现原理:
1. 在RN源码里, 会判断平台, iOS则走AlertIOS.alert(), 然后native端走UIAlertController

Modal的实现原理:
1. 在Native端注册了一个组件, 在RN里调用, 获取属性, 在native端创建UIView, 并赋给VC里的view, 然后通过Present这个VC来展示

