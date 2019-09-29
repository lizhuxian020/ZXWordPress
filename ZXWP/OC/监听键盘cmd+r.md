# UIKeyCommand

通过创建UIKeyCommand实例, 添加到系统, 在创建实例的时候, 就已经决定action了

添加到系统两种方法:
1. 通过UIViewController的addKeyCommand, 来把UIKeyCommand实例添加到系统, 得以监听, keyCommand的action的target就是vc实例
2. 通过添加UIResponse的分类, 把keyCommand属性的get方法重写一遍, return一个NSArray<id<UIKeyCommand>>的数组, 那系统就会自动注册这些keyCommand, key的action的target就是UIResponse, 需要还需要在分类上添加action, 响应这个通知.