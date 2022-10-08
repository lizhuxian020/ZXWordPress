#编程式事务三大件
##1. PlatformTransactionManager
![img_4.png](img_4.png)

##2. TransactionDefinition
![img.png](img.png)

###隔离级别
![img_1.png](img_1.png)

###传播行为
![img_2.png](img_2.png)


##3. TransactionStatus
![img_3.png](img_3.png)



#基于XML的Spring事务声明式编程
![img_5.png](img_5.png)
需要注意:
1. 如果Dao层用的不是jdbc或mybatis, 则TransactionManager要改

##配置事务的属性
![img_7.png](img_7.png)
1. method: 表示切面表达式里方法的名称, 可以使用通配符update*, 表示updateUser, updateRole...


#基于注解方式编写Spring事务
![img_8.png](img_8.png)
1. 主要是写@Transactional这个注解
2. 接着写组件扫描
3. 接着写tx的注解驱动
![img_9.png](img_9.png)

