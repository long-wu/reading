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
* 检索单个值: select name, address from user where age > 21		查询年龄大于21岁的用户的name和address
* 不匹配检索: select name from user where age != 21				查询年龄不等于21岁的用户的name
* 范围值检索: select name from user where age BETWEEN 18 AND 30 	查询年龄在18岁到30岁之间的用户的name
* 空值检索: select name from user where address IS NULL			查询地址为空的用户的name

### WHERE高级用法
* AND使用: select name from user where age !=21 and address IS NULL 		查询地址为空且年龄不等于21岁的用户的name
* OR使用:  select name from user where age !=21 or address IS NULL 		查询地址为空或年龄不等于21岁的用户的name
* WHERE后面的and和or原则上可以有任意个，但是请注意AND的优先级比OR高 一般有多个AND和OR时推荐使用()来人为确定优先级
* 人为确定优先级: select name from user where (auth=1 OR name="root") and address IS NULL
* IN使用: select name, address from user where auth in (1, 2)			查询权限在1和2之中的用户的name和address
* NOT使用: select name from user where not auth = 1 order by name		查询权限不为1的用户的name并按name升序排序

### 使用通配符进行过滤
* 通配符: 用来匹配值的一部分的特殊字符，本身是SQL中的where子句中有特殊含义的字符 比如%代表任何字符出现任意次
* 搜索模式: 由字面值、通配符或两者组合而成构成的搜索条件 
* 在SQL中使用LIKE操作符来使用通配符和字面值组成的搜索模式
* %	替代 0 个或多个字符	eg: select name from user where name LIKE "wyb%"	通配以wyb开头的name
* _	替代一个字符			eg: select name from user where name LIKE "wyb___"	通配像wyb666 wyb123这样的6位name
* [charlist] 字符列中的任何单一字符	eg: select name from user where name LIKE "[WZ]%" 通配以W或Z开头的name
* [^charlist] 不在字符列中的任何单一字符  eg: select name from user where name LIKE "[^WZ]%" 通配不以W和Z开头的name(有时候要用!代替^)


## 函数相关
### SQL函数介绍
	SQL中提供了一些函数可以进行数据处理，所有函数一般都可以在不同的数据库中使用，但实现可能不同，具体情况具体分析
	SQL函数用法: SELECT function(列) FROM 表
	SQL函数的基本类型:
		Aggregate 函数 	--> 	聚集函数 操作面向一系列的值，并返回一个单一的值		eg: AVG COUNT MAX MIN SUM FIRST LAST 
		Scalar 函数 		--> 	标量函数 操作面向某个单一的值，并返回基于输入值的一个单一的值		eg: 下面的文本处理函数和数值处理函数
	在下面我介绍一些常用的函数的用法

### 文本处理函数
* LEFT:		返回串左边的字符
* LENGTH:	返回串的长度
* LOWER:	将串转换成小写
* LTRIM:	去掉串左边的空格
* RIGHT:	返回串左边的空格
* RTRIM:	去掉串左边的空格
* SOUNDEX:	返回串的SOUNDEX值
* UPPER: 	将串转换成大写
* eg: select name, UPPER(name) AS name_upper from user where auth = 1	查询auth为1的用户的name和name对应的大写(AS给列起别名)

### 日期和时间处理函数
	各个数据库都有自己的日期和时间处理函数，在此不详细介绍

### 数值处理函数
* ABS:		返回一个数的绝对值
* COS:		返回一个角度的余弦
* EXP:		返回一个数的指数值
* PI:		返回圆周率
* SIN:		返回一个角度的正弦值
* SQRT:		返回一个数的平方根
* TAN:		返回一个角度的正切

### 聚集函数
	聚集函数作用: 汇总数据	eg: 确定表中行数、找出表中行组的和、找出表中某列的最大值、最小值、平均值
	AVG: 返回某列的平均值	select AVG(score_py) as avg_score_py from score		返回成绩表中python列的平均成绩 
	COUNT: 		计算表中行的数目或计算符合特定条件的行的数目	
		select COUNT(*) as num_user from user  								返回user表中行的数目也就是用户数
		select COUNT(email) as num_email from user							返回user表中有email的用户数目
	MAX: 返回指定列中的最大值		select MAX(age) as max_age from user		返回user表中最大的年龄
	MIN: 返回指定列中的最小值		select MAX(age) as max_age from user		返回user表中最大的年龄
	SUM: 返回指定列中的和  		select SUM(score_py) as sum_score_py		返回成绩表中python列的总成绩 
							
	以上五个聚集函数都可以这样使用:
		对所有的行执行计算 指定ALL参数或者不给参数(ALL是默认行为)
		只包含不同的值 指定DISTINCT参数(去重)
		eg: select AVG(DISTINCT score_py) as avg_score_py from score		返回成绩表中python列的平均成绩(只考虑不同的成绩)	
					
	组合聚合函数: select语句可根据需要包含多个聚集函数
		eg:	select COUNT(*) as num_items, MIN(score_py) as min_score_py, MAX(score_py) as max_score_py from score 


## 分组数据
### 数据分组介绍	
	SQL聚集函数可以用来汇总数据
	数据分组允许把数据分成多个逻辑组，以便能对某个组进行聚集计算

### 创建分组


### 过滤分组


### 分组和排序