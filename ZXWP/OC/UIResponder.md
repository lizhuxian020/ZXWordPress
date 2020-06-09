#UIResponder
nextResponder: 可以根据这个属性来找到事件的响应链
canBecomeFirstResponder: 默认是NO, subClass必须要重写该方法并returnYES
* 调用becomeFirstResponder会调用这个方法

becomeFirstResponder: 
* 向UIKit框架请求变成第一响应者<Font color='red'>(不一定会成功)</Font>
    * UIKit会先调用当前Object的canBecomeFirstResponder, 是否为YES
    * 然后会调用当前第一响应者的ResignFirstResponder
        * 这里面又会先调用canResignFirstResponder, 默认YES
    * UIKit会询问当前第一响应者, 是否可以辞去第一响应者(canResignFirstResponder), 如果可以, 则会先辞去当前的第一响应者, 然后再让自己成为第一响应者
* 必须加入到window上面才可以使用!!!
    * 未加入到window上使用也没什么反应(已测试)
* 成为新响应者之后, 原来的Event也会跟着过来. <font color='red'>UIKit会视图展示InputView!!</font>
* 成为响应者之后, 响应链也会发生变化!(已测试)
* 重写必须要调用super

canResignFirstResponder: 默认是YES
resignFirstResponder:
* 重写必须要调用super
* 不一定会成功, 只是请求辞去响应者, 会先判断canResignFirstResponder

# UITextField实现原理(个人猜想)
1. 继承UIControl:UIView
2. 重写TouchBegin, 来成为第一个响应者(但还未成为第一响应者)
3. 重新声明inputView等属性, 放开readWrite(原属性是readOnly)
4. 重写BecomeFirstResponder, 改写状态, 改写UI
5. 关于系统键盘的弹出
    1. 一直都想自己重写一个UITextField, 但是问题在于如何弹出并使用系统键盘
    2. 经过一系列百度, 发现还是不行.
    3. 发现一个协议叫UITextInputTrait, 是只有UITextField和UITextView实现的
    4. 所以认为, 系统没给出自己调用并使用系统键盘的方法. 系统键盘也是系统自己编出来的InputView, 只提供给UITextField和UITextView使用.
    5. 那些工作上出现自己定义新的输入框, 新的键盘. 往往都是自己重新写UIView, 重新画InputView
    6. 如果想使用系统键盘, 简单点就继承UITextField, 来实现自定义输入框.