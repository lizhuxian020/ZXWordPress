# aop的概念

![img.png](img.png)

#动态代理简单实现
![img_2.png](img_2.png)
1. JDK动态代理是条件是目标对象比如要有接口, 通过接口创建出目标对象的"兄弟对象"即代理对象
2. cglib代理是通过以目标对象为父类, 创建出目标对象, 但不是继承. 是兄弟关系 
##jdk动态代理
![img_1.png](img_1.png)

Advice: 是普通继承Object的类, 里面有简单的前置方法和后置方法
TargetInterface: 就只有一个简单的方法声明

Target: 对Interface的实现

使用Proxy.newProxyInstance来创建代理对象,代理对象调用方法时就对调用invoke方法


##cglib代理
![img_3.png](img_3.png)


#快速入门
![img_4.png](img_4.png)
1. 引入aspectjweave包
2. 在sppringContext.xml上添加aop命名空间, 进行aop配置
![img_5.png](img_5.png)
![img_6.png](img_6.png)
![img_7.png](img_7.png)

##切点表达式的抽取
![img_8.png](img_8.png)

#使用注解方式写AOP

##1. 目标对象
![img_9.png](img_9.png)
##2. 切面对象
![img_10.png](img_10.png)
##3. 配置springContext
![img_11.png](img_11.png)
![img_12.png](img_12.png)

##切面表达式抽取
![img_13.png](img_13.png)