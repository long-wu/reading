sql语句复习整理
=======================


## 了解SQL
### 数据库基础概念
* 数据库: 数据库是以一个以某种有组织的方式存储的数据集合，换句话说数据库就是保存有组织的数据的容器，类似一个excel文件
* 表: 某种特点的数据类型的结构化清单，类似一个excel文件中的一张表
* 模式: 关于数据库和表的布局及特性的信息
* 列: 表中的一个字段，所有表都是由一个获多个列组成的
* 数据类型: 表中的每一列都有对应的数据类型，用来限制该列中存储的数据
* 行: 表中的一个记录
* 主键: 一列(或者一组列)的值能够唯一标识表中的每一个行

### 主键补充
* 任意两行的主键不应该相等
* 每个行都必须具有一个主键值(主键值不允许为NULL)
* 主键值不得更新不得重用

### 关于SQL
	SQL: SQL是结构化查询语言(Structured Query Language)的缩写，是专门用来与数据库进行通信的语言
	SQL的优点: 不是一门专有语言，是数据库领域的通用语言；简单易学；看上去简单实际上可以用SQL完成很复杂和高级的数据库操作
	注意SQL中不区分大小写，为了方便，后面SQL都一般使用小写，SQL一般要带分号，为了简便后面省略分号


## 检索数据相关
### SELECT
* 检索单个列: select 列名 from 表名	eg: select name from user	从user表中检索所有的姓名
* 检索多个列: select 列名1, 列名2, 列名3 from 表名		eg: select name, auth, address from user	从user表中检索所有的姓名、权限、地址
* 检索所有列: select * from 表名		eg: select * from user		从user表中检索所有列(输出所有数据)

### SELECT + order by
* 可以使用order by子句指定对某列进行排序然后输出
* 按当前选择列排序: select name from user order by name 		对name列以字母顺序进行排序
* 按非当前选择列排序: select name from user order by auth		根据auth列排序输出所有name
* 按多个列排序: select id, name, auth, address from user order by name, auth 		首先按姓名排序再按权限排序然后输出以上列
* 指定排序方向: 数据排序默认升序 但是可以指定DESC(降序)  eg: select name from user order by id DESC		按用户id降序排序然后输出name列
* 多个列排序指定排序方向: select name from user order by id DESC, auth	 	 按用户id降序排序用户权限升序排序然后输出name列
* 多个列排序指定降序排序: select name from user order by id DESC, auth DESC	 按用户id降序排序用户权限降序排序然后输出name列


## 过滤数据相关
### WHERE基础
* 基础使用: select name, address from user where age = 21	查询年龄为21岁的用户的name和address
* WHERE子句后面的条件操作符: =  <>  !=  <  <=  >  >=  !>  !<  BETWEEN	 IS NULL
* WHERE子句条件操作符部分解释: = 等于  <> 不等于  != 不等于  !> 不大于  BETWEEN 在指定的两个值之间  IS NULL 为NULL值(也就是为空)
* 注意: 不同数据库对不同操作符有不同的支持，有的操作符在某些数据库上可能无法使用

### WHERE实际使用
* 检索单个值: select name, address from user where age > 21	查询年龄大于21岁的用户的name和address
* 不匹配检索: select name from user where age != 21			查询年龄不等于21岁的用户的name
* 范围值检索: select name from user where age BETWEEN 18 AND 30 	查询年龄在18岁到30岁之间的用户的name
* 