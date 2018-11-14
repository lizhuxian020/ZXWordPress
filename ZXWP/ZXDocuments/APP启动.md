# Global
* 单例
* 收到LoginSuccess通知
    * 创建各个Client(不带AT):
        * chinamusic(音乐云)
            * 可配置: CloudSwith_MUSIC
        * audio(电台云)
            * host在Native端写死
        * openAudio(代码没写注释, 不知道是什么云)
            * host在Native端写死
        * beva(贝瓦儿歌)
            * host在Native端写死
        * qr(二维码)
            * host在Native端写死
        * 百度map
            * 没URL
    * 向HMS获取AT
        * 入参: ATToken的clientID, 这个在注册HMS的时候确定的
            * APP启动时, 判断本地有没有getServiceUrl
            * ...自己看
* 拿到AT, 则向RN发送通知: HMSInterfaceResult, 带上AT
* 拿到AT创建ClientHandle实例, 带着AT初始化各个Client: 
 * skill(技能云)
     * CloudSwith_SpeakerServer
 * train(训练云)
     * CloudSwith_SpeakerServer
 * smartauth(智家云)
     * CloudSwith_IOT
 * iot(不知道什么云)
     *  CloudSwith_IOT
* 之后获取HomeID, 成功就拿到HomeID, 然后走success回调, 不成功就走fail回调
* 走success回调之前, 创建MQTTClient!
    * mqtt(mqtt云)
        * homeCloudFordevice
        * 创建MQTTClient对象, 并向MQTTManager发送请求iotConfig方法
        * 这里略微复杂, 不写下去了, 寄几研究=.=
* success回调给RN发了通知initClientFinish, RN的app-start-page收到这个通知.

##处理initClientFinish通知
* RN的`app-get-devices`的`initClientFinish`处理这个通知
* 处理在用户协议, 用户是否同意`硬件升级(autoUpgrade)`, 是否同意`用户体验改进计划(impPlan)`. 1则同意, 0则不同意.
    * 从本地获取后, 通过SkillClient上传服务器
    * 上传之后, 把本地标记置为-1
* 本地获取HomeID, 拼接URL, 通过IOTClient请求服务器上的设备list
    * 目前返回空数组=.=
    * 有数据返回时
        * map数组, `return obj.devInfo.proID=='001T'`
        * 拿到deviceArray
        * 判断deviceArray是否有空的名字, 有的话默认加上`我的音箱`
            * 名字不能与当前deviceArray重复, 重复则`我的音箱1`, 以此类推
            * 修改完成, 与HomeID和自身DevIDB拼接URL, 通过IOTClient发起修改设备名字的请求
            * <font color='green'>TODO: 这里的逻辑太冗余, 网络请求发送多次, 效率低下. 有机会优化一下.</font>
        * 拿到云数据, 跟本地数据对比, 把本地数据的DevID在云数据上没有的移除
        * 巴拉巴拉巴拉
        * 最后ActionReset('add-speaker')

##分析IOTClient请求逻辑
* 所有的client都通过固定key值, 存放在ClientManager单例维护的一个mutableDic里.
* 创建一个HTTPBaseRequest实例, 赋予属性如下
    * Method: Post
    * URL: NSString(子路径)
    * body: 请求头参数
* 通过ClientManager和key值拿到对应的client对象, 并对他发送-send:callback:
    * 这个父类实现的方法, 调用子类实现的asyncExchangeRequest
* 在子类的asyncExchangeRequest里头, 主要通过HttpBaseRequest来创建RequestManager的实例, 通过这个实例来发送网络请求
    * <font color='red'>在创建这个实例的时候, 把实例的成员变量securityPolicy实例也创建了</font>
        * 根据baseURL, 找到Bundle里对应的证书, 创建AFSecurityPolicy
    * <font color='red'>正因为这个所以才抓包不了!</font>
* 把client的Header和HTTPBaseRequest的Header都加到RequestManager的Header里



