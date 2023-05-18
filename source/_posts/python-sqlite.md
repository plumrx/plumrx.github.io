---
title: python-sqlite
date: 2022-12-05 15:57:40
tags:
---

# python-sqlite  

## 目标清单  
1. 了解功能，使用（连接、建表、CRUD）
2. 数据库使用场景，测试数据
3. 与其他数据库区别，优势
4. 事务、连表查询

## 任务  
1. 建库名为‘test_data'
2. 建表名为‘real_world_user’，列“user、email‘，其中email是主键。
3. 建表名为'real_world_password'，列“email,password'，email为主键。
4. 分别对表进行CUDR操作。
5. 联表查询。
6. 从表中查询数据，然后发起接口测试。
7. 事务，同时操作两张表，更新其操作。           



## 1. 基本操作 
### 1.1 建库 & 连库  
创建数据库连接,如果数据库不存在，那么它就会被创建，最后将返回一个数据库对象。
```
con = sqlite3.connect('test_data.db')
```

### 1.2 表操作 
*执行所有SQL都要使用以下语句：*
```
c=con.cursor()
c.execute(sql)
```
1. 新建表  
``` sql
CREATE TABLE real_world_password(
email CHAR(50) PRIMARY KEY  NOT NULL,
password CHAR(26) NOT NULL
);
```

2. 查所有表  
```
 .tables
```
{% asset_image 查所有表.png 查所有表 %}


3. 删除表
```sql
DROP TABLE ready_to_delete_tables;
```
{% asset_image 删除表.png 删除表 %}





### 1.3 数据操作
1. 新增数据- INSERT 语句  
* 用法一 - 带字段名称    
``` sql
INSERT INTO real_world_user(
user,email)VALUES('TEST1','TEST1@EAMIL.COM');
```

{% asset_image 插数结果.png 插数结果 %} 
<br>  

* 用法二 - 不带字段名称  
``` sql
INSERT INTO real_world_userVALUES('TEST11','TEST11@EAMIL.COM');
```

{% asset_image insert用法二.png insert用法二 %} 

2. 删除数据 - DELETE 语句  
* 有条件删除数据
```sql
DELETE FROM table_name
WHERE [condition];
```
{% asset_image 删除记录.png 删除记录 %} 

* 删除所有数据
``` sql
DELETE FROM table_name;
```

3. 修改数据 - UPDATE 语句  
```
UPDATE table_name
SET column1 = value1, column2 = value2...., columnN = valueN
WHERE [condition];
```
{% asset_image 更新表中数据.png 更新表中数据 %}   
若不带 where 条件，则所有行都会被更新。


4. 单表查询 - SELECT语句  
* 查询单表所有字段
```
select * from real_world_user
```
{% asset_image 查单表所有字段.png 查单表所有字段 %} 


* 查询单表部分字段  
```
select user from real_world_user;
```
{% asset_image 查单表部分字段.png 查单表部分字段 %} 
* 

5. 联表查询 - JOIN 语句  
JOIN 是一种通过共同值来结合两个表中字段的手段。  

* 交叉连接 - CROSS JOIN  
如果两个输入表分别有 x 和 y 行，则结果表有 x*y 行。由于交叉连接（CROSS JOIN）有可能产生非常大的表，使用时必须谨慎，只在适当的时候使用它们。  
```sql 
SELECT grd_id,name,age,grade from school CROSS JOIN grade;
```
{% asset_image cross_join_school.png cross_join_school %} 
<center>school 表数据</center>
<br>
{% asset_image cross_join_grade.png cross_join_grade %} 
<center>grade 表数据</center>
<br>
{% asset_image cross_join.png cross_join %} 
<center>两表交叉连接结果</center>

* 内连接  
* 外连接  

```
```
{% asset_image 查单表部分字段.png 查单表部分字段 %} 




### 1.4 命令行操作  
1. 连接数据库  
```
sqlite3 test_data.db 
----------------
SQLite version 3.37.0 2021-11-27 14:13:22
Enter ".help" for usage hints.
```
2. 执行语句  
```
# 展示所有表
 .table
----------------
 real_world_password  real_world_user    
```




## 2.常见报错  
1. 命令行返回 ‘...>’  
原因：上条语句不完整，请继续输入。
解决方案：继续输入剩余语句，直至完整。

2. stepping, UNIQUE constraint failed:  
原因：插数主键重复。
解决方案：修改数据至不重复。