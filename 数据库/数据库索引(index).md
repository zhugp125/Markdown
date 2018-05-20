# 数据库索引(index)

标签（空格分隔）： 数据库

---
[TOC]

### 1. 索引简介
数据库索引适合在数据量大，查询操作频繁的数据表上建立索引。例如一个有一万条数据的表查找一条数据，没有索引的话只能遍历。如果有索引，就可以通过很少的次数确定要查找数据的位置，从而达到优化查找的目的。
索引是在存储引擎中实现的。每种引擎实现的索引不尽相同，支持的索引也不尽相同。所有的储存引擎中每个表最少支持16个索引数，总索引长度最少为256个字节。
MySQL中索引的储存类型有两种：HTREE和HASH，具体和表的储存引擎有关。MyISAM和InnoDB储存引擎只支持HTREE索引；MEMORY和HEAP储存引擎支持HTREE和HASH索引。

#### 1.1 索引的优点：
1. 通过创建唯一索引，可以确保数据的唯一性。
2. 可以大大加快查询速度，建立索引的主要原因。
3. 实现数据完整性，加快表与表之间的连接。
4. 减少查询中排序和分组的时间。

#### 1.2 索引的缺点
1. 创建索引和维护索引要耗费时间，随着数据量的增加而增加。
2. 索引需要占用物理空间，并且更容易达到最大空间限制。
3. 数据表的数据增删改操作时，索引需要动态进行维护，降低了数据的维护速度。

### 2. 索引分类
1. 普通索引和唯一索引(QUNIQUE INDEX)
普通索引是MySQL中最基本的索引类型。允许索引定义列插入重复值和空值。
唯一索引，索引列的值必须唯一，允许为空值。如果是组合索引，则列值的组合必须唯一。
主键索引是特殊的唯一索引，不允许为空值。

2. 单列索引和多列索引
单列索引是指索引只包含一个列，一个表可以有多个单列索引。
多列索引是指索引包含多个列。多列索引查询时遵循左前缀原则。

3. 全文索引(FULLTEXT INDEX)
全文索引支持在索引的列进行全文查找，允许索引列插入空值和重复值。全文索引允许在CHAR、VARCHAR和TEXT类型上创建。MySQL只有MyISAM储存引擎支持全文索引。

4. 空间索引(SPATIAL INDEX)
空间索引是对空间类型列上创建的索引。MySQL有4种空间类型：GEOMETRY、POINT、LINESTRING和POLYGON。创建空间索引的列必须声明为NOT NULL。 MySQL只有MyISAM储存引擎支持空间索引。

### 3. 索引设计原则
1. 索引列要尽可能的少。
2. 避免对经常进行更新的列设计索引，索引中的列要尽量少。
3. 数据量少的尽量不使用索引。
4. 在条件表达式中，经常使用的不同值多时创建索引；不同值少时不创建索引。
5. 数据唯一的情况指定唯一索引。
6. 在频繁进行分组或排序的列上建立索引。

### 4. 创建索引
#### 4.1 创建索引有三种方式：
一种在创建表的时候添加索引；两种在已创建上的表添加索引。

1. 创建表的时候创建索引
语法格式如下：
CREATE TABLE table_name [col_name data_type] [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] [index_name](col_name[length]) [ASC | DESC]
语法解析：
- table_name：数据表名称 必填项
- [col_name data_type]：数据表的列名与列对应的数据类型 可选项
- [UNIQUE | FULLTEXT | SPATIAL]：索引修饰，分别表示唯一索引、全文索引、空间索引 可选项
- [INDEX | KEY]：可填任意一项，表示索引
- [index_name]：索引名称。不写索引名称时，索引名称与col_name相同
- col_name：作为索引的列，必须是数据表table_name中出现的列
- [length]：col_name索引的长度
- [ASC | DESC]：排序，ASC：升序；DESC：降序

 1.1 创建普通索引
普通索引是MySQL最基本的索引，没有唯一性的限制，目的只是为加快查询速度。
示例，在表student创建一个id索引：
CREATE TABLE student(
s_id INT(11) NOT NULL,
s_name VARCHAR(20),
s_addr VARCHAR(50),
INDEX(s_id)
)ENGINE=InnoDB, CHARSET=utf8;

 该语句执行完成之后，可以用SHOW CREATE TABLE查看表结构。
SHOW CREATE TABLE student\G;

 1.2 创建唯一索引
与普通索引相比，唯一索引不允许索引列值重复，允许有空值。如果是组合索引，则组合值不允许重复。
在表class上创建一个唯一索引id：
CREATE TABLE class(
c_id INT(11) NOT NULL,
c_addr VARCHAR(50),
UNIQUE INDEX uqid(c_id)
)ENGINE=InnoDB, CHARSET=utf8;

 1.3 创建单列索引
单列索引就是该索引只有一列。一个表可以有多个单列索引。
在学生成绩表store上创建一个单列索引：学生ID
CREATE TABLE store(
s_id INT(11) NOT NULL,
s_chinese VARCHAR(20),
s_math VARCHAR(20),
s_engish VARCHAR(20),
INDEX(s_id)
)ENGINE=InnoDB, CHARSET=utf8;

 1.4 创建组合索引
组合索引即是在多个字段上创建一个索引。
CREATE TABLE test1(
s_id INT(11) NOT NULL,
s_name VARCHAR(20),
s_age VARCHAR(20),
INDEX multiIndex(s_id,s_name,s_age)
)ENGINE=InnoDB, CHARSET=utf8;
组合索引在查询时，遵循左前缀原则，即索引的第一个列在查询条件中才会起作用。

 1.5 创建全文索引
MySQL只有MyISAM存储引擎支持全文索引，并且只有TEXT、CHAR和VARCHAR类型支持全文索引。
CREATE TABLE test2(
s_id INT(11) NOT NULL,
s_name VARCHAR(20),
s_age VARCHAR(20),
FULLTEXT INDEX fullIndex(s_name)
)ENGINE=MyISAM, CHARSET=utf8;

 1.6 空间索引
建立空间索引的字段必须是非空字段。
CREATE TABLE test3(
s_id INT(11) NOT NULL,
s_geometry geometry NOT NULL,
SPATIAL INDEX fullIndex(s_geometry)
)ENGINE=MyISAM, CHARSET=utf8;

2. 在已存在的表上添加索引

- 使用ALTER TABLE语句来添加索引
语法格式如下：
ALTER TABLE table_name ADD [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] [index_name](col_name[length]) [ASC | DESC]
语法解析同上。
示例：
ALTER TABLE test2 ADD INDEX(s_id);

- 使用CREATE INDEX语句添加索引
语法格式如下：
CREATE INDEX [UNIQUE | FULLTEXT | SPATIAL]  index_name ON table_name(col_name[length]) [ASC | DESC]
语法解析同上。
示例：
CREATE INDEX id ON test3(s_id);

### 5. 删除索引
#### 5.1 删除索引有两种方式
1. ALTER TABLE语句删除索引
语法格式如下：
ALTER TABLE table_name DROP INDEX index_name;
语法解析：
- table_name：数据表名
- index_name：索引名
示例：
ALTER TABLE test2 DROP INDEX s_id;

2. DROP INDEX语句删除索引
语法格式如下：
DROP INDEX index_name ON table_name;
语法解析同上。
示例：
DROP INDEX id ON test3;