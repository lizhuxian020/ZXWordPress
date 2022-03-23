# mysql的命令

链接数据库

查看数据库列表
`show databases`

使用数据库(指定)
`use databaseName`

查看表列表
`show tables`

创建表
`　　 mysql> create table name (id int(3) auto_increment not null primary key, xm char(8),xb char(2),csny date);`

查看表字段
`desc tableName`

修改表字段
`alter table tableName modify name varchar(256);`

修改字段为自动增加
` alter table user modify id int(3) auto_increment;`