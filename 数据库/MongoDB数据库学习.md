# MongoDB数据库学习

标签（空格分隔）： NoSQL 数据库

---
[TOC]

MongoDB是属于文档数据库类型，是NoSQL（Not Only SQL）的一种。
### 1. SQL和NoSQL的区别
SQL是一种结构化的标准数据库语言，使用起来必须事先定义表，执行严格的SQL语句进行数据库的操作。NoSQL，即不仅仅是SQL的意思。使用起来很方便，类似于文档操作，非常符合程序员的思维习惯。
####1.1 关系型数据库遵循ACID原则。
1. 原子性（Atomic）
原子性是指一个事务中的所有操作要么成功，要么失败，任一条操作失败，整个操作都会回滚到原先状态。
2. 一致性（Consistency）
一致性是指事务的操作不会影响数据库的一致性原则。
3. 独立性（Isolation）
独立性是指事务之间的操作是相互独立的，不会相互影响
4. 持久性（Durability）
持久性是指事务一旦提交之后，数据库就会保持修够后的状态。
####1.2 非关系型数据库
NoSQL指非关系型数据库，也指不仅仅是SQL。NoSQL用于超大规模数据的存储。数据存入NoSQL数据库无需特定的模式，并且方便进行横向扩展。
####1.3 RDBMS vs NoSQL
1. RDBMS
- 高度组织化结构数据
- 结构化查询语言
- 数据和关系都存储在单独的表中
- 数据操纵语言，数据定义语言
- 严格的一致性
- 事务性
2. NoSQL
- 代表着不仅仅是SQL
- 没有声明性查询语言
- 没有预定义的模式
- 键值对存储、列存储、文档存储、图行数据库
- 最终一致性
- CAP定理
- 高性能、高可用性和可伸缩性
####1.4 CAP定理
在计算机科学中，CAP定理(CAP theorem)，又被称作布鲁尔定理(Brewer's theorem)，它指出对于一个分布式计算系统来说，不可能同时满足以下三点：

- 一致性(Consistency)所有节点在同一时间具有相同的数据
- 可用性(Availability)保证每个请求不管成功或者失败都有响应
- 分隔容忍(Partition tolerance)系统中任意信息的丢失或失败不会影响系统的继续运作
CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性、可用性和分区容错这三个需求，最多只能同时较好的满足两个。
因此，根据CAP原理将NoSQL数据库分成了满足CA原则、满足CP原则和满足AP原则三大类：

- CA - 单点集群，满足一致性，可用性系统，通常在扩展性不太强大。
- CP - 满足一致性，分区容忍性的系统，通常性能不是特别高。
- AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。
####1.5 NoSQL的优点和缺点
1. 优点
- 高可扩展行
- 分布式计算
- 低成本
- 架构的灵活性，半结构化数据
- 没有复杂的关系
2. 缺点
- 没有标准化
- 有限的查询功能
- 最终一致是不直观的程序
####1.6 NoSQL数据库分类
|类型|部分代表|特点|
|:---:|:---:|:---:|
|列存储|Hbase<br/>Cassandra<br/>Hypertable|按列存储数据的。最大的特点是方便存储结构化和半结构化数据，方便做数据压缩，对针对某一列或某几列的查询有非常大的IO优势|
|文档存储|MongoDB<br/>CouchDB|文档存储一般用类似json的格式存储，存储的内容是文档型。这样也就有机会对某些字段建立索引，实现关系数据库的某些功能|
|key-value存储|Tokyo Cabinet/Tyrant<br/>Berkeley DB<br/>Memcache DB<br/>Redis|可以通过key快速查询到对应value。一般来说，存储不管value的格式，全部存入。|
|图存储|Neo4J<br/>FlockDB|图形关系的最佳存储。使用传统关系数据库来解决，性能低下，而且设计使用不方便。|
|对象存储|db4o<br/>Versant|通过类似面对对象语言的语法操作数据库，通过对象的方式存取数据|
|xml数据库|Berkeley DB XML<br/>BaseX|高效的存储XML数据，并支持XML的内部查询方法，比如XQuery，Xpath|
###2. MongoDB简介
MongoDB是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。MongoDB旨在为WEB应用提供可扩展的高性能数据存储解决方案。MongoDB将数据存储为一个文档，数据结构类似json格式。
{
&nbsp;&nbsp;name: "zhugp",
&nbsp;&nbsp;age: 26,
&nbsp;&nbsp;job: "C++ programer",
&nbsp;&nbsp;groups: ["news", "sports"]
}
####2.1 主要特点
- MongoDB是一个面向文档存储的数据库，操作起来比较简单和容易。
- MongoDB记录中设置任何属性的索引来实现更快的排序。
- 可扩展性强
- 可用于分布式系统中
- 支持丰富的查询表达式
- 支持运行服务器端执行脚本
- 支持多种编程语言
- 安装简单
####2.2 MongoDB下载
[下载地址](https://www.mongodb.com/download-center#community)，选择与系统对应的版本。
Mac有两种安装方式：
1. 使用curl命令安装
\# 进入 /usr/local
cd /usr/local

\# 下载
sudo curl -0 https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.4.2.tgz

\# 解压
sudo tar -zxvf mongodb-osx-x86_64-3.4.2.tgz

\# 重命名为 mongodb 目录
sudo mv mongodb-osx-x86_64-3.4.2 mongodb

安装完成后，添加MongoDB的安装目录到PATH路径中
export PATH=/usr/local/mongodb/bin:$PATH
2. 使用brew安装

- 最简单的安装方法
> sudo brew install mongodb

- 安装支持TLS/SSL
> sudo brew install mongodb --with-openssl

- 安装最新开发版本
> sudo brew install mongodb --devel

####2.3 运行MongoDB
1. 首先创建一个数据存储目录/data/db
> sudo mkdir -p /data/db

2. 启动mongodb，默认数据库目录/data/db
> sudo mongod
\# 如果没有创建全局路径PATH，需要进入一下目录
cd /usr/local/mongodb/bin
sudo ./mongod

3. 打开终端执行一下命令：
> cd /usr/local/mongodb/bin
./mongod

> 注：如果你的数据库目录不是/data/db，可以通过--dbpath来指定
通过brew安装方式，mongodb安装目录为/usr/local/opt/mongodb/bin

###3. MongoDB概念解析
|SQL术语/概念|MongoDB术语/概念|解释|
|:---:|:---:|:---|
|databases|databases|数据库|
|table|collection|数据库表/集合|
|row|document|数据库记录行/文档|
|column|field|数据字段/域|
|index|index|索引|
|table joins||表连接，MongoDB不支持|
|primary key|primary key|主键，MongoDB自动将_id字段作为主键|
####3.1 数据库
一个mogodb中可以建立多个数据库。默认数据库为“db”，存储在data目录中。
MongoDB的单个实例可以容纳多个独立的数据库，每个都有自己的集合和权限，不同的数据库存放在不同的文件中。
1. “show dbs”命令可以显示所有数据的列表
2. “db”命令可以显示当前数据库对象或集合
3. “use”命令可以连接到一个指定的数据库

数据库通过名字来标识，必须是满足如下条件的任意UTF-8字符串
- 不能是空字符串
- 不能含有‘’(空格)、.、$、/、\和\0(空字符)
- 应全部小写
- 最多64字节
- 不能是admin、local或者config
####3.2 文档
文档是一组键值(key-value)对(即BSON)。MongoDB的文档不需要设置相同的字段，相同的字段不需要设置相同的数据类型。
一个简单的文档例子：
{"name":"zhugp","sex":""}

需要注意的是：
1. 文档中的键/值对是有序的
2. 文档中值不仅可以在双引号里面的字符串，还可以是其他几种类型
3. MongoDB区分类型和大小写
4. MongoDB的文档不能有重复的键
5. 文档的键是字符串，除少数情况外，可以使用任意UTF-8字符

文档命名规范：
- 不能含有\0(空字符)。这个字符用来表示键的结尾
- .和$有特别的意义，只有在特定环境下才能使用
- 以下划线‘_’开头的键是保留的
####3.3 集合
集合就是MongoDB文档组，类似于关系数据库的表。集合存于数据库中，没有固定的结构。通常情况下我们插入的数据会有一定的关联性。
例如，插入如下的文档到集合中：
{"site":"www.baidu.com"}
{"site":"www.google.cn","name":"google"}
{"site":"www.runoob.com","name":"菜鸟教程","num":5}
当第一个文档插入时，集合就会被创建。

1. 合法的集合名
- 集合名不能是空字符串
- 集合名不能含有\0字符，这个字符表示集合名的结尾
- 集合名不能以"system."开头，这是为系统集合保留的前缀
- 用户创建的集合名字不能含有保留字符
2. capped collections
Capped collections就是固定的collection。
Capped collections是高性能自动的维护对象的插入顺序。非常适合记录日志的功能。和标准的collection不同，你必须显示的创建capped collection，指定一个collection的大小，单位时字节。collection的空间是提前分配的。
> db.createCollection("mycoll", {capped:true, size:100000})

- 在capped collection中，你能添加新的对象
- 可以进行更新数据，对象存储空间不会增加。
- 数据库不运行删除。使用drop方法删除collection所有的行
- 删除之后，你必须显示的重新创建这个collection
- 32位机器上，capped collection最大存储为1e9个字节
####3.4 元数据
数据库的信息是存储在集合中。它们使用了系统的命名空间：
> dbname.system.*

在MongoDB数据库中名字空间是包含多种信息的特殊集合，如下表：
|集合命名空间|描述|
|:---|:---|
|dbname.system.namespaces|列出所有名字空间|
|dbname.system.indexs|列出所有索引|
|dbname.system.profile|包含数据库概要信息|
|dbname.system.users|列出所有可访问数据库的用户|
|dbname.system.sources|包含复制对端的服务器信息|
对于修改系统集合对象有如下限制。
{system.users}是可以修改的。
{system.profile}是可以删除的。
插入数据时，{system.indexs}会创建索引。
####3.5 MongoDB数据类型
下表为MongoDB中常用的几种类型。
|数据类型|描述|
|:---|:---|
|String|字符串，存储数据常用的数据类型。编码统一为UTF-8|
|Integer|整型数值。用于存储数值，分32位或64位|
|Boolean|布尔值。用于存储布尔值（真/假）|
|Double|双精度浮点值。用于存储浮点值|
|Min/Max keys|将一个值与BSON元素的最低值和最高值相对比|
|Array|用于将数组或列表或多个值存储位一个键|
|Timestamp|时间戳。记录文档修改或添加的具体时间|
|Object|用于内嵌文档|
|Null|用于创建空值|
|Symbol|符号，基本上等同字符串类型。不同的是，一般用特殊符号类型语言|
|Date|日期时间。用于UNIX时间格式来存储当前日期或时间。或创建Date对象，传入年月日信息|
|Object ID|对象ID，用于创建文档的ID|
|Binary Data|二进制数据。用于存储二进制数据|
|Code|代码类型。用于在文档中存储JavaScript代码|
|Regular expressio|正则表达式类型。用于存储正则表达式|
###4. MongoDB连接
使用MongoDB shell来连接MongoDB服务器。
####4.1 标准的URI连接语法：
> mongodb://[username:password@]host1[:port1][, host2[:port2],...[,hostN[:postN]]][/[database][?options]]

- mongodb:// 这是固定的格式，必须要指定
- username:password@ 可选项，如果设置，在连接数据库服务器之后，驱动尝试登陆这个数据库
- host1 必须指定一个host，指定了连接服务器的地址
- postX 可选的指定端口，默认27017
- /database 如果指定username:password@，连接并验证登陆指定数据库。若不指定，默认连接test数据库
- ?options 是可选项。如果不使用/database前面需要加/。所有连接选项都是键值对，每个键值对通过&或;隔开
标准的连接格式包含多个选项(options)，如下
|选项|描述|
|:---|:---|
|replicaSet=name|验证replica set的名称|
|slaveOk=true\|false|true:在connect=direct模式下，驱动会连接第一台机器。在connect=replicaSet模式下，驱动会发送所有的写请求并且把读取操作分布在其他从服务器<br>false:在connect=direct模式下，驱动会自动寻找主服务器。在connect=replicaSet模式下，驱动仅仅连接主服务器，并且所有的读写命令都连接到主服务器|
|safe=true\|false|true:在执行更新操作之后，驱动都会发送getLastError命令来确保更新成功<br>false:在每次更新之后，驱动不会发送getLastError命令来确保更新成功|
|w=n|驱动添加{w:n}到getLastError命令.应用于safe=true|
|wtimeoutMS=ms|驱动添加{wtimeoutMS:ms}到getLastError命令.应用于safe=true|
|fsync=true\|false|true:驱动添加{fsync:true}到getLastError命令.应用于safe=true<br>false:驱动不会添加getLastError命令中|
|journal=true\|false|如果设置为true，同步到journal(在提交到数据库前写入到实体中).应用于safe=true|
|connectTimeoutMS=ms|可以打开连接的时间|
|socketTimeoutMS=ms|发送和接受socket的时间|
####4.2 实例
1. 连接本地数据库服务器，默认端口
> mongodb://localhost

2. 指定用户名和密码登陆本地admin数据库
> mongodb://fired:foobar@localhost

3. 指定用户名和密码登陆本地baz数据库
> mongodb://fired:foobar@localhost/baz

4. 连接replica pair，服务器1为example1.com，服务器2为example2.com
> mongodb://example1.com:27017,example2.com:27017

5. 连接replica set三台服务器(端口27017，27018和27019)
> mongodb://localhost,localhost:27018,localhost:27019

6. 连接replica set三台服务器，写入操作应用在主服务器并且分布查询到从服务器
> mongodb://host1,host2,host3/?slaveOk=true

7. 直接连接第一个服务器，无论是replica set一部分或者主服务器或者从服务器
> mongodb://host1,host2,host3/?connect=direct;slaveOk=true

8. 安全模式连接到localhost
> mongodb://localhost/?safe=true

9. 安全模式连接到localhost，并且等待至少两个复制服务器成功写入，超市时间设置为2秒
> mongodb://host1,host2,host3/?safe=true;w=2;wtimeoutMS=2000

### 5. MongoDB 创建数据库
#### 5.1 语法
MongoDB创建数据库的语法格式如下：
> use DATABASE_NAME

如果数据不存在，则创建数据库，否则切换到指定数据库。
#### 5.2 实例
以下实例创建数据库zhugp
> \> use zhugp
switched to db zhugp
\> db
zhugp
\>

查看所有的数据库，使用show dbs命令：
> \> show dbs
local 0.001GB
test  0.001GB
\>

新创建的数据库zhugp并不在数据库的列表中，要显示它，需要添加一些数据。
> \> db.zhugp.insert({'name':'zhugp'})
WriteResult({'nInserted':1})
\> show dbs
local 0.001GB
zhugp 0.001GB
test  0.001GB
\>

MongoDB中默认的数据库为test，如果你没有创建新的数据库，集合将存放在test数据库中。
### 6. MongoDB删除数据库
#### 6.1 语法
MongoDB删除数据库的语法格式如下：
> db.dropDatabase()

删除当前数据库，默认为test，使用db命令可以查看当前数据库
#### 6.2 实例
删除数据库zhugp。

1. 首先，查看所有数据库：
> show dbs
local 0.0001GB
zhugp 0.0001GB
test  0.0001GB

2. 切换到数据库zhugp
> \> use zhugp
switched to db zhugp
\>

3. 执行删除命令
> db.dropDatabase()
{'dropped' : 'zhugp', 'ok' : 1}

4. 查看是否成功删除数据库
> show dbs
local 0.0001GB
test  0.0001GB

#### 6.3 删除集合
1. 集合删除语法格式如下：
> db.collection.drop()

2. 删除实例
> \> use zhugp
switched to db zhugp
\> show tables
name
\> db.name.drop()
true
\> show tables
\>

### 7. MongoDB插入文档
文档的数据结构和JSON基本一样。所有存储在集合中的数据都是BSON格式。
BSON是一种类json的一种二进制形式的存储格式，简称Binary JSON。
#### 7.1 插入文档
MongoDB使用insert()或save()方法向集合中插入文档，语法如下：
> db.COLLECTION_NAME.insert(document)

#### 7.2 实例
以下文档可以存储在MongoDB的zhugp数据库中的col集合中：

1. 先打开zhugp数据库
> use zhugp

2. 插入文档
> db.col.insert({"name":"zhugp", "sex":"m"})

col是集合名。如果该集合不在数据库中，MongoDB会自动创建该集合并插入文档。
查看以插入文档：
> db.col.find()

也可以将数据定义为一个变量，如下：
> document = ({"name":"zhugp","sex":"f"});

然后执行插入操作：
> db.col.insert(document)

插入文档，你也可以使用db.col.save(document)命令。如果不指定_id字段，save方法类似于insert方法。如果指定_id字段，则会更新该_id的数据
### 8. MongoDB更新文档
MongoDB使用update()和save()方法更新集合中的文档。两个函数存在一些差异
#### 8.1 update()方法
update()方法用于更新已经存在的文档。语法格式如下：
> db.collection.update(
[query],
[update],
{
upset: [boolean],
multi: [boolean],
writeConcern: [document]
}
)

参数说明：
- query：update的查询条件，类似sql update查询内的where后面的
- update：update的对象和一些更新的操作符(如$,$inc...)等，也可以理解为sql update查询内set后面的
- upset：可选，这个参数的意思是，如果不存在update的记录，是否插入。true为插入，默认false，不插入。
- multi：可选，mongodb默认是false，只更新找到的第一条记录，如果这个参数为true，就把条件查出的多条记录全部更新。
- writeConcern：可选，抛出异常的级别
#### 8.2 实例

1. 先在集合col插入一条数据：
> db.col.insert({
"title":"MongoDB 教程",
"description":"MongoDB是一个NoSQL数据库",
"by":"菜鸟教程",
"url":"http://www.runoob.com",
"likes":100
})

2. 使用update()方法更新title
> db.col.update({"title":"MongoDB 教程"},{$set:{"title":"MongoDB"}})

如果你要修改多条相同的文档，则需要设置multi参数为true。
> db.col.update({"title":"MongoDB 教程"},{$set:{"title":"MongoDB"}},{multi:true})

#### 8.3 save()方法
save()方法通过传入的文档来替换已有文档。语法格式如下：
> db.collection.save(
[document],
{
writeConcern:[document]
}
)

参数说明：

- document：文档数据
- writeConcern：可选，抛出异常的级别

#### 8.4 实例
数据库中已经存在这样一条数据：
> {"_id":ObjectId("5a69cbb31eb97d9eafd1a59c"),"title":"python","text":"hello world"}

下面通过ID替换文档数据：
> db.col.save({"_id":ObjectId("5a69cbb31eb97d9eafd1a59c"),"title":"asio","text":"hello asio"})

通过find命令来查看替换后的数据：
> db.col.find().pretty()

### 9. MongoDB删除文档
MongoDB remove()函数是用来移除集合中的数据。在执行remove()函数前，先执行find()命令来判断执行的条件是否正确，是一个好的习惯。
#### 9.1 语法
remove()方法的基本语法格式如下所示：
> db.collection.remove(
[query],
[justOne]
)

如果你的MongoDB是2.6版本以后的，语法格式如下：
> db.collection.remove(
[query],
{
justOne:[boolean],
writeConcern:[document]
}
)

参数说明：

- query：可选，删除的文档的条件
- justOne：可选，如果设置为true或1，则只删除一个文档
- writeConcern：可选，抛出异常的级别

#### 9.2 实例
1. 先执行两次相同的插入操作
> db.col.insert({'title':'MongoDB','url':'http://www.runoob.com'})

2. 使用find()查看数据
> db.col.find()

3. 移除刚刚插入的两天数据
> db.col.remove({'title':'MongoDB'})

如果你只想删除第一条找到的记录，设置justOne为1或true，如下：
> db.colltion.remove(deletion_criteria, 1)

如果你想删除所有数据，可以使用如下方式(类似常规SQL的truncate命令)
> db.col.remove()

#### 9.3 deletOne()和deleteMany()
1. 删除集合下全部文档
> db.collection.deleteMany()

2. 删除集合下特定条件的全部文档
> db.collection.deleteMany(deletion_criteria)

3. 删除集合下特定条件的一个文档
> db.collection.deleteOne(deletion_criteria)

### 10. MongoDB查询文档
MongoDB查询文档使用find()方法。find()方法以非结构化的方式来显示所有文档。

#### 10.1 语法
MongoDB查询数据的语法格式如下：
> db.collection.find(query, projection)

- query：可选，使用查询操作符指定查询条件
- projection：可选，使用投影操作符指定返回的键。查询时返回文档中的所有参数，只需要省略该参数即可（默认省略）

如果你需要以易读的方式来读取数据，可以使用pretty()方法。语法格式如下：
> db.col.find().pretty()

pretty()方法格式化的方法来显示所有文档。
#### 10.2 实例
查询集合col的数据：
> db.col.find().pretty()

除了find()方法之外，还有一个findOne()方法，它只返回一个文档。
#### 10.3 MongoDB与RDBMS Where语句对比
|操作|格式|范例|RDBMS类似语句|
|---|---|---|---|
|等于|{key:value}|db.col.find({"by":"zhugp"})|where by = 'zhugp'|
|小于|{key:{\$lt:value}}|db.col.find({"by":{\$lt:"zhugp"}})|where by < 'zhugp'|
|小于或等于|{key:{\$lte:value}}|db.col.find({"by":{\$lte:"zhugp"}})|where by <= 'zhugp'|
|大于|{key:{\$gt:value}}|db.col.find({"by":{\$gt:"zhugp"}})|where by > 'zhugp'|
|大于或等于|{key:{\$gte:value}}|db.col.find({"by":{\$gte:"zhugp"}})|where by >= 'zhugp'|
|不等于|{key:{\$ne:value}}|db.col.find({"by":{\$ne:"zhugp"}})|where by != 'zhugp'|
#### 10.4 MongoDB AND条件
MongoDB的find()方法可以传入多个键(key)，每个键以逗号隔开，即常规SQL的AND条件。
语法格式如下：
> db.col.find({key1:value1,key2:value2}).pretty()
示例：
> db.col.find({'by':'zhugp','title':'Mongo'}).pretty()

#### 10.5 MongoDB OR条件
MongoDB OR条件语句使用了关键字$or，语法格式如下：
> db.col.find({\$or:[{key1:value1},{key2:value2}]}).pretty()
示例：
> db.col.find({\$or:[{'by':'zhugp','title':'Mongo'}]}).pretty()

### 11. MongoDB条件操作符
#### 11.1 描述
条件操作符用于比较两个表达式并从mongodb集合中获取数据。
MongoDB中条件操作符有：

- (>) 大于 - \$gt
- (<) 小于 - \$lt
- (>=) 大于等于 - \$gte
- (<=) 小于等于 - \$lte

在空数据库zhugp上，集合名为col，插入如下数据：
> db.col.insert({
title:'PHP教程',
desc:'PHP是一种创建动态交互性站点的强有力的服务器端脚本语言',
by: '菜鸟教程',
url: 'http://www.runoob.com',
tags: ['php'],
likes: 200
})

> db.col.insert({
title:'java教程',
desc:'java是由Sun Microsystems公司于1995年推出的高级程序设计语言',
by: '菜鸟教程',
url: 'http://www.runoob.com',
tags: ['Java'],
likes: 150
})

> db.col.insert({
title:'MongoDB教程',
desc:'MongoDB是一个Nosql数据库',
by: '菜鸟教程',
url: 'http://www.runoob.com',
tags: ['mongodb'],
likes: 100
})

#### 11.2 MongoDB(>)大于操作符-\$gt
如果你想获取col集合中likes大于100的数据，你可以使用以下命令：
> db.col.find({'likes' : {\$gt : 100}})

类似于sql语句：
> select * from col where likes > 100;

#### 11.3 MongoDB(>=)大于等于操作符-\$gte
如果你想获取col集合中likes大于等于100的数据，你可以使用以下命令：
> db.col.find({'likes' : {\$gte : 100}})

类似于sql语句：
> select * from col where likes >= 100;

#### 11.4 MongoDB(<)小于操作符-\$lt
如果你想获取col集合中likes小于150的数据，你可以使用以下命令：
> db.col.find({'likes' : {\$lt : 150}})

类似于sql语句：
> select * from col where likes < 150;

#### 11.5 MongoDB(<=)小于等于操作符-\$lte
如果你想获取col集合中likes小于等于150的数据，你可以使用以下命令：
> db.col.find({'likes' : {\$lte : 150}})

类似于sql语句：
> select * from col where likes <= 150;

### 12. MongoDB \$type操作符
#### 12.1 描述 
\$type操作符是基于BSON类型来检索集合中匹配的数据类型，并返回结果。
MongoDB中可以使用的类型如下表所示：
|类型|数字|备注|
|---|---|---|
|Double|1||
|String|2||
|Object|3||
|Array|4||
|Binary data|5||
|Undefined|6|已废弃|
|Object id|7||
|Boolean|8||
|Date|9||
|Null|10||
|Regular Expression|11||
|JavaScript|13||
|Symbol|14||
|JavaScript(with scope)|15||
|32-bit integer|16||
|Timestamp|17||
|64-bit integer|18||
|Min key|255|Query with -1|
|Max key|127||
使用上一节插入数据库zhugp，集合col中的数据。
#### 12.2 MongoDB操作符-\$type实例
获取col集合中title为String的数据：
> db.col.find({'title' : {\$type :  2}})

### 13. MongoDB Limit与Skip方法
#### 13.1 MongoDB Limit()方法
使用MongoDB的limit方法，可以从MongoDB查询数据中读取指定数目的记录条数。

1. 语法
limit()方法基本语法如下所示：
> db.COLLECTION_NAME.find().limit(NUMBER)

2. 实例
查询集合col的两条记录：
> db.col.find({"title":"mongodb"}).limit(2)

#### 13.2 MongoDB Skip()方法
使用skip方法来跳过指定数量的数据。

1. 语法
skip方法脚本语法格式如下：
> db.COLLECTION_NAME.find().limt(NUMBER).skip(NUMBER)

2. 实例
显示集合中的第二条数据：
> db.col.find().limit(1).skip(1)

#### 13.3 MongoDB sort()方法
在MongoDB中使用sort方法对数据进行排序，sort方法可以通过指定排序的字段，并使用1和-1来指定排序的方式，1为升序，-1为降序

1. 语法
sort方法基本语法如下所示：
> db.COLLECTION_NAME.find().sort({key:1})

2. 实例
col集合中，按字段title的降序排列：
> db.col.find().sort({'title':-1})

### 14. MongoDB 索引
索引的作用是提高查询效率。没有索引的查询必须从前往后扫描集合中的每个文件并选择符合查询条件的记录。
这种查询方式在数据量大时，查询效率非常低。
索引是特殊的数据结构，存储在一个易于遍历读取的数据集合中。查询时可以快速的定位要查询的记录。

#### 14.1 创建索引的方法
MongoDB使用ensureIndex()方法来创建索引。
1. 语法
ensureIndex()方法基本语法格式如下：
> db.collection.ensureIndex({key:1})

语法中的key是要创建的索引字段，1为指定升序创建索引，-1为指定降序创建索引
2. 实例
> db.col.ensureIndex({'title':1})

ensureIndex()也可以使用多个字段创建索引（关系数据库中称复合索引）
> db.col.ensureIndex({'title':1,'desc':-1})

#### 14.2 ensureIndex()接收可选参数
可选参数列表如下：
|参数|类型|描述|
|---|---|---|
|background|Boolean|true后台创建索引，默认false|
|unique|Boolean|true创建唯一索引，默认false|
|name|string|索引名称。默认字段名和排序顺序为索引名称|
|dropDups|Boolean|true创建唯一索引时删除重复记录，默认false|
|sparses|Boolean|对文档中不存在的字段数据不启用索引。true在索引字段中不会查询出不包含对应字段的文档，默认false|
|expireAfterSeconds|integer|指定一个以秒为单位的数值，完成TTL设定，设定集合的生存时间|
|v|index<br>version|索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本|
|weights|document|索引权重值，数值在1到99,999之间，表示该索引相对于其他索引字段的得分权重|
|default_language|string|对于文本索引，该参数决定了停用词及词干和词器的规则列表。默认英语|
|language_override|string|对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认language|
#### 14.3 实例
在后台创建索引：
> db.col.ensureIndex({'title':1}, {background: true})

### 15. MongoDB 聚合
MongoDB中聚合(aggregate)主要用于处理数据(诸如统计平均值，求和等)，并返回计算后的数据结果。

#### 15.1 aggregate()方法
MongoDB中聚合的方法使用aggregate()

1. 语法
aggregate()方法的基本语法格式如下：
> db.collection.aggregate(AGGREGATE_OPERATION)

2. 实例
> db.col.aggregate([{\$group : {_id : "\$user", num_tutorial : {\$sum : 1}}}])
{
    result : [
        {
            "_id" : "runoob",
            "num_tutorial" : 2
        },
        {
            "_id" : "asio",
            "num_tutorial" : 1
        }
    ],
    "ok" : 1
}

以上实例类似sql语句：select user, count(*) from col group by user;
下面展示一些聚合的表达式：
|表达式|描述|实例|
|---|---|---|
|\$sum|计算总和|db.col.aggregate([{\$group:{_id:"\$user",num:{\$sum:1}}}])|
|\$avg|计算平均值|db.col.aggregate([{\$group:{_id:"\$user",num:{\$avg:1}}}])|
|\$min|计算最小值|db.col.aggregate([{\$group:{_id:"\$user",num:{\$min:1}}}])|
|\$max|计算最大值|db.col.aggregate([{\$group:{_id:"\$user",num:{\$max:1}}}])|
|\$push|在结果文档中插入到一个数组中|db.col.aggregate([{\$group:{_id:"\$user",num:{\$push:1}}}])|
|\$addToSet|在结果文档中插入到一个数组中，不创建副本|db.col.aggregate([{\$group:{_id:"\$user",num:{\$addToSet:1}}}])|
|\$first|获取第一个文档数据|db.col.aggregate([{\$group:{_id:"\$user",num:{\$first:1}}}])|
|\$last|获取最后一个文档数据|db.col.aggregate([{\$group:{_id:"\$user",num:{\$last:1}}}])|
#### 15.2 管道的概念
管道在Unix和Linux中一般用于将当前命令的输出结果作为下一个命令的参数。
MongoDB的聚合管道将MongoDB文档在一个管道处理完毕后将结果传递给下一个管道处理。管道操作是可以重复的。
表达式：处理输入文档并输出。表达式是无状态的，只能用于计算当前聚合管道的文档，不能处理其他的文档。
集合框架常用的几个操作：
- \$project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
- \$match：用于过滤数据，只输出符合条件的文档。\$match使用MongoDB的标准查询操作。
- \$limit：用来限制MongoDB聚合管道返回的文档数
- \$skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
- \$unwind：将文档中某一个数组类型字段拆分成多条，每条包含数组中的一个值。
- \$group：将集合中的文档分组，可用于统计结果
- \$sort：将输入文档排序后输出
- \$geoNear：输出接近某一地理位置的有序文档

#### 15.3 管道操作实例
1. \$project
> db.col.aggregate({\$project:{title:1,author:1}});
这样，结果中就只有_id,title,author三个字段了，默认情况下是包含_id字段的。如果不想包含_id字段，
> db.col.aggregate({\$project:{_id:0,title:1,author:1}});

2. \$match
> db.col.aggregate([{\$macth:{score:{\$gt:70,$lte:90}}},{\$group:{_id:null,count:{\$sum:1}}}]);
\$match用于获取分数大于79小于或等于90的记录，然后将符合条件的记录送到下一阶段\$group管道操作符进行处理。

3. \$skip
> db.col.aggregate({\$skip:5});
经过\$skip管道操作符处理后，前5个文档被过滤掉。

### 16. MongoDB复制（副本集）
MongoDB复制是将数据同步在多个服务器的过程。复制提供了数据的冗余备份，并在多个服务器上存储数据副本，提高了数据的可用性，并可以保证数据的安全。复制还允许你从硬件故障和服务中断中恢复数据。
#### 16.1 复制的优势
- 保障数据的安全性
- 数据高可用性（24*7）
- 灾难恢复
- 无需停机维护（如备份，重建索引，压缩）
- 分布式读取数据
#### 16.2 MongoDB复制的原理
mongodb的复制至少需要两个节点。一个主节点，负责处理客户端请求；其余都是从节点，负责复制主节点上的数据。
mongodb各个节点常见的搭配方式为：一主一从、一主多从。
主节点记录在其上的所有操作，从节点定期轮询主节点获取这些操作，然后对自己的数据副本执行这些操作，使从节点的数据与主节点一致。
MongoDB复制结构图如下：

##### 16.2.1 副本集的特征：
- N个节点的集群
- 任何节点可作为主节点
- 所有写入操作都在主节点上
- 自动故障转移
- 自动恢复
#### 16.3 MongoDB副本集设置
1. 启动MongoDB服务器
使用--replSet选项来启动mongoDB。基本语法格式如下：

> mongod --port "PORT" --dbpath "DB_DATA_PATH" --replSet "INSTANCE_NAME"
实例
mongod --port 27017 --dbpath "/usr/local/db" --replSet rs0

以上实例会启动一个名为rs0的MongoDB实例，端口号27017。
启动后打开命令提示框并连接上mongoDB服务。
在MongoDB客户端使用命令rs.initiate()来启动一个新的副本集。
使用rs.conf()来查看副本集的配置
查看副本集状态使用rs.status()
#### 16.4 副本集添加成员
使用rs.add()方法来添加副本集的成员

1. 语法格式如下：
> rs.add(HOST_NAME: PORT)

2. 实例
假设已经启动了名为mongod1.net，端口号27017的Mongo服务。在客户端命令窗口使用rs.add()命令将其添加到副本集中，命令如下所示：
> rs.add("mongod1.net:27017")

MongoDB中，只能通过主节点将mongo服务添加到副本集中，判断当前运行的Mongo服务是否为主节点，可以使用命令db.isMaster()。
MongoDB的副本集，主从在主机宕机后所有服务将停止，而副本集在主机宕机后，副本接管主节点成为主节点，不会出现宕机的情况。

### 17. MongoDB分片
在MongoDB里面存在另一种集群，就是分片技术，可以满足MongoDB数据量大量增长的需求。
当MongoDB存储海量的数据时，一台机器可能不足以存储数据，也可能不足以提供可接受的读写吞吐量。这时可以通过在多台机器上分割数据，使得数据库系统能存储和处理更多的数据。
#### 17.1 需要使用分片的地方
- 复制所有的写入操作到主节点
- 延迟的敏感数据会在主节点查询
- 单个副本集限制在12个节点
- 当请求量巨大时会出现内存不足
- 本地磁盘不足
- 垂直扩展价格昂贵
#### 17.2 MongoDB分片
分片三个主要组件：

- Shard：
用于存储实际的数据块，实际生产环境中一个shard server角色可由几台机器组成一个replica set承担，防止主机单点故障
- Config Server：
mongod实例，存储了整个ClusterMetadata，其中包括chunk信息
- Query Routers：
前端路由，客户端由此接入，且让整个集群看上去像单一数据库，前端应用可以透明使用。
### 18. MongoDB备份与恢复
#### 18.1 MongoDB数据备份
在MongoDB中，使用mongodump命令来备份MongoDB数据。该命令可以导出所有数据到指定目录中。
mongodump命令可以通过参数指定导出的数据量级转存的服务器。

1. 语法
mongodump命令脚本语法如下：
> mongodump -h dbhost -d dbname -o dbdirectory

- h:MongoDB所在服务器地址，如127.0.0.1，也可以指定端口号：127.0.0.1:27017
- d:需要备份的数据库实例。
- o:备份的数据库存放位置。目录必须存在。

2. 实例
本地使用27017启动你的服务器。打开命令提示符窗口，进入MongoDB的安装目录的bin目录输入命令：
> mongodump

执行该命令后，客户端会连接到ip为127.0.0.1端口号27017的MongoDB服务上，并备份所有数据到bin/dump目录中。

mongodump命令可选参数列表如下：
|语法|描述|实例|
|---|---|---|
|mongodump --host HOST --port PORT|该命令将备份所有MongoDB数据|mongodump --host test --port 27017|
|mongodump --dbpath PATH --out BACKUP||mongodump --dbpath /data/db --out /data/backup|
|mongodump --collection COLLECTION --db DB_NAME|该命令将备份指定数据库的集合|mongodump --collection col --db runoob|
#### 18.2 MongoDB数据恢复
mongodb使用mongorestore命令来恢复备份的数据

1. 语法
mongorestore命令脚本语法如下：
> mongorestore -h [host][:port] -d dbname [path]

- --host[:port],-h[:port]:
MongoDB所在服务器地址，默认localhost:27017
- -db,-d:
需要恢复的数据库实例
- --drop:
恢复数据库时，先删除的当前数据库
- [path]:
mongorestore最后一个参数，设置备份数据所在位置
- --dir:
指定备份的目录。不能同时指定[path]和[--dir]选项

2. 实例
> mongorestore

### 19. MongoDB 监控
安装部署并允许MongoDB服务后，必须要了解MongoDB的运行情况，查看MongoDB性能。
MongoDB中提供了mongostat和mongotop两个命令来监控MongoDB的运行情况。

#### 19.1 mongostat命令
mongostat是mongodb自带的状态检测工具，在命令行下使用。它会间隔固定时间获取mongodb的当前运行状态。
启动你的Mongod服务，进入MongoDB安装目录下的bin目录，然后输入mongostat
> cd /mongod/bin
\> mongostat

#### 19.2 mongotop命令
mongotop也是mongodb下一个内置工具。mongotop提供了一个方法，用来跟踪一个MongoDB的实例，查看那些操作费时。mongotop提供每个集合的水平的统计目录。
启动你的Mongod服务，进入MongoDB安装目录下的bin目录，然后输入mongotop
> cd /mongod/bin
\> mongotop

带参实例：
> mongotop 10

后面的10是[sleeptime]参数，可以不使用。等待的时间长度，以秒为单位。
> mongotop --locks

报告每个数据库的锁的使用，输入结果字段说明：
- ns：包含数据库命名空间，后者结合了数据库名称和集合
- db：包含数据库的名称。
- total：mongod话费的时间工作在这个命名空间提供总额
- read：提供大量的时间，这mongod话费在执行读操作
- write：提供这个命名空间进行写操作





