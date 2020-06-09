# Navigation

* NavigationController是一个不用于显示的控制器
    * 用于控制那些用于显示的控制器之间的跳转

##NavigationBar
* 负责显示导航栏的是UINavigationBar
* 一个UINavigationController只有一个NavigationBar
* 每一个Controller之间显示不同的导航栏样式, 通过配置他们每一个navigationItem来实现. navigationItem每一个VC都有
    * navigationItem是继承与NSObject的, 相当于是navigationBar的配置代码, 来让NavigationBar显示
    * 就像UIToolBar一样, NavigationItem通过添加UIBarButtonItem来添加按钮
    * UIBarButtonItem: UIBarItem : NSObject