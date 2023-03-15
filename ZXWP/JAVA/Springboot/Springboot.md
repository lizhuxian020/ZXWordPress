# 内容回顾
@ComponentScan()
1. 默认不写入参, 扫描范围则是当前配置类所在包及其子包

![img.png](img.png)
![img_1.png](img_1.png)

![img_2.png](img_2.png)

临时入参(临时属性)
`java -jar xxx.jar --server.port=8080`

加载顺序
![img_3.png](img_3.png)
其中6: Java System properties则需要以下这么
![img_26.png](img_26.png)


idea调试命令行临时属性
![img_4.png](img_4.png)

多配置文件, 在resource下config目录的配置优先级高于外面的
![img_5.png](img_5.png)
![img_6.png](img_6.png)

`--spring.config.name=ebank`
临时属性, 设置配置文件名, 不需要加扩展名
![img_7.png](img_7.png)
![img_8.png](img_8.png)
![img_9.png](img_9.png)

#多环境开发
![img_10.png](img_10.png)
![img_11.png](img_11.png)
同个环境, 多个文件(分业务)
![img_12.png](img_12.png)
使用group代替, dev 对应着devMVC, devDB
![img_13.png](img_13.png)


@EnableConfigurationProperties
加上这个注解, 那可以不用再加@Component
![img_27.png](img_27.png)

松散绑定
![img_28.png](img_28.png)

给Bean添加属性校验
![img_29.png](img_29.png)
