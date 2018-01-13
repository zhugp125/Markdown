# 数据库视图

标签（空格分隔）： 数据库 视图

---
[TOC]

### 1. 视图简介
视图是一个虚拟表，是从一个或多个表中导出的表，行为类似表。在视图中用户可以通过select语句查询数据，通过insert、update、delete修改数据。
视图使用户操作数据库方便，而且保证数据系统的安全。

### 2. 创建视图
视图是基于select语句和已存在的表来创建的，可以基于一张表，也可以基于多张表。
创建视图的语法：
CREATE[OR REPLACE] [ALGORITHM = {UNDEFINED|MERGE|TEMPTABLE}]
VIEW view_name([column_list])
AS SELECT statement
[WITH [CASCADED|LOCAL] CHECK OPTION]
参数说明：

- CREATE表示创建新的视图，REPLACE表示替换已存在的视图
- ALGORITHM表示视图选择的算法
 -  UNDEFINED表示MySQL自动选择算法。
 -  MERGE表示将使用的视图语句和视图定义合并起来，使得视图定义的一部分替换对应的部分语句。
 -  TEMPTABLE表示将视图的结果存入临时表，然后用临时表来执行语句。
- view_name 视图名
- column_list 视图列
- SELECT statement表示SELECT语句
- [WITH [CASCADED|LOCAL] CHECK OPTION]表示视图在更新时保证在视图的权限范围之内
 -  CASCADED表示更新视图满足所有相关视图和表的条件，默认值。
 -  LOCAL表示更新视图满足视图本身定义的条件即可。

视图属于数据库，默认情况下属于当前数据库。指定数据库创建视图，应指定为db_name.view_name

#### 2.1 在单表上创建视图
数据库中已存在如下表：
CREATE TABLE student (
  s_id int(11) NOT NULL,
  s_name varchar(20) DEFAULT NULL,
  s_addr varchar(50) DEFAULT NULL,
  KEY s_id (s_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

在表student上创建视图student_view的语句如下：
CREATE VIEW student_view(id, name) AS SELECT s_id, s_name FROM student;

查询视图语句：
SELECT * FROM student_view;

#### 2.2 在多表上创建视图
数据库中还存在如下的表：
CREATE TABLE score (
  s_id int(11) NOT NULL,
  s_chinese varchar(20) DEFAULT NULL,
  s_math varchar(20) DEFAULT NULL,
  s_english varchar(20) DEFAULT NULL,
  KEY s_id (s_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

在表student和score上创建试图student_score_view的语句如下：
CREATE VIEW 
student_score_view(id,name,chinese,math,english) 
AS SELECT stu.s_id,stu.s_name,s.s_chinese,s.s_math,s.s_english 
FROM student stu, score s 
WHERE stu.s_id = s.s_id;

### 3. 查看视图
#### 3.1 DESCRIBE语句查看视图
语法：DESCRIBE view_name;
简写：DESC view_name;

#### 3.2 SHOW TABLE STATUS语句查看视图
语法：SHOW TABLE STATUS LIKE 'view_name'\G;

#### 3.3 SHOW CREATE VIEW语句查看视图
语法：SHOW CREATE VIEW view_name\G;

#### 3.4 在views表中查看所有视图
SELECT * FROM information_schema.views;

### 4. 修改视图
#### 4.1 CREATE OR REPLACE VIEW 语句修改视图
CREATE[OR REPLACE] [ALGORITHM = {UNDEFINED|MERGE|TEMPTABLE}]
VIEW view_name([column_list])
AS SELECT statement
[WITH [CASCADED|LOCAL] CHECK OPTION]
和创建视图的语法是一样的。当视图存在时，修改视图；当视图不存在时，创建视图。

修改示例：
CREATE OR REPLACE VIEW student_view AS SELECT * FROM student;

#### 4.2 ALTER 语句修改视图
ALTER [ALGORITHM = {UNDEFINED|MERGE|TEMPTABLE}]
VIEW view_name([column_list])
AS SELECT statement
[WITH [CASCADED|LOCAL] CHECK OPTION]

修改示例：
ALTER VIEW student_view AS SELECT * FROM student;

### 5. 更新视图
更新视图是指通过视图来更新、插入和删除表中的数据。视图是虚拟表，更新视图实际上是在更新基本表。

#### 5.1 UPDATE视图
语法：
UPDATE view_name SET col_name = value;
示例：
UPDATE student_view SET s_id = 1;

#### 5.2 INSERT视图
语法：
INSERT view_name([col_name,]) INTO VALUES([value,]);
示例：
INSERT student_view(s_id) INTO VALUES(1);

#### 5.3 DELETE视图
语法：
DELETE FROM view_name [statement];
示例：
DELETE FROM student_view WHERE s_id = 1;

### 6. 删除视图
使用DROP VIEW语句删除不需要的视图，语法如下：
DROP VIEW [IF EXISTS]
view_name[, view_name]
[RESTRICT | CASCADE]

示例：
DROP VIEW IF EXISTS student_view;