# Pod库的创建
1. 在安装好CocoaPod环境下使用`pod lib create PodName`来创建私有库
2. 在终端上回答5个问题, ios, objc, yes, none, no
3. 在pod里创建文件时, target需要是podName, 而不是Pod-podName-Example, 文件属性要public
    1. ![-w273](media/15923587660668.jpg)
4. 创建好之后, 需要重新pod install一下, 即可在原project上调用
5. 以上就是一次完成的从0到创建class, 使用class. 如果有发现调用不了, 或者调用没反应, 就去删除deviceData, cmd + shift + k, 再rebuild一下.


