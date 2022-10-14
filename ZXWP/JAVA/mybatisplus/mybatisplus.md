# 使用mybatisPlus

> 使用了mybatisPlus时候，不能引用mybatis，否则会报错：java.lang.NoClassDefFoundError: org/mybatis/logging/LoggerFactory

![img.png](img.png)
> 使用`MybatisSqlSessionFactoryBuilder`就已经把mybatis和mybatisPlus整合起来了

![img_1.png](img_1.png)
> 使用`BaseMapper<T>`则继承了他很多查询方法
> 
![img_2.png](img_2.png)
> 使用`@TableName`指定改POJO对应的表
> 

![img_3.png](img_3.png)
> `使用@TableId来指定主键`
> 
![img_4.png](img_4.png)
![img_5.png](img_5.png)
![img_6.png](img_6.png)

# 条件更新
![img_7.png](img_7.png)
![img_8.png](img_8.png)

# 删除
![img_9.png](img_9.png)
![img_10.png](img_10.png)
![img_11.png](img_11.png)
![img_12.png](img_12.png)

# 查询
## 大于20岁
![img_13.png](img_13.png)
## 模糊查询
![img_14.png](img_14.png)