![img_3.png](img_3.png)
# 导包
![img_6.png](img_6.png)
# 配置Mapper.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="UserMapper">

</mapper>
```
![img_7.png](img_7.png)
1. 在MyBatis代码中, 通过userMapper.findAll()来执行语句
2. 

# 配置MyBatis核心文件
![img_8.png](img_8.png)

# 编写代码
![img_9.png](img_9.png) 

# 编写更新操作代码
![img_10.png](img_10.png)
![img_11.png](img_11.png)
1. 注意要提交事务, MyBatis默认是不提交事务的.
2. 或者可以rollback
![img_22.png](img_22.png)
![img_21.png](img_21.png)

## 同理更新操作也一样
![img_12.png](img_12.png)
![img_13.png](img_13.png)

#核心配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

```
![img_14.png](img_14.png)

##environment标签
![img_15.png](img_15.png)
![img_16.png](img_16.png) 

##mapper标签
![img_17.png](img_17.png)

##property标签
![img_18.png](img_18.png)
注意: 这里加载property文件和之前学Spring容器加载property文件方式不一样
![img_19.png](img_19.png)
1. 因为命名空间的不一样, MyBatis的是在configuration标签下的命名空间,只用properties来加载文件
2. 在SpringContext命名空间下, 使用context:property-placeholder来加载文件

##typeAliases标签(核心配置文件的)
![img_20.png](img_20.png)