# Redis 学习笔记

## NoSQL

NoSQL == Not only SQL 非关系型数据库

* 优势：
  * 高并发读写
  * 海量数据高效读写
  * 高扩展性和高可用性
  * 数据模型灵活

* NoSQL 产品
  * Redis
  * MongoDB
  * CouchDB
  * Neo4J
  * HBase

* NoSQL 产品分类

| 分类 | 相关产品 | 典型应用 | 优势 | 劣势 |
| --- | --- | --- | -- | -- | 
| 键值存储 | Redis | 缓存，主要用于处理大量数据的高并发访问负载 | 查询快 | 结构乱 |
| 列存储 | HBase | 分布式文件系统 | 查找快，可扩展、分布式扩展 | 功能局限 |
| 文档型数据库 | MongoDB | Web 应用 | 数据结构要求不严格 | 查询性能不高，缺乏统一的查询语法 |
| 图形（Graph）数据库 | Neo4J | 社交网络、推荐一系统。专注于构建关系图谱 | -- | 不容易做分布式集群方案 |

## Redis

* 2009 年开发、开源。
* C 语言开发。
* 基于内存。
* 提供多种键值数据类型支持。
* 应用场景
  * 缓存。最常用。
  * 任务队列。
  * 网站访问统计。
  * 数据过期处理。
  * 排行榜。
  * 分布式集群架构中的 session 分离。
  * 取最新 N 个数据的操作。
  * 排重。
  * 实时系统。

## Redis 安装

> 安装在 CentOS 7.5

1. 下载、上传，解压

```c
// 上传
pscp C:\Users\a1560\Downloads\redis-5.0.2.tar.gz root@192.168.188.138:/home/hgytmp/

// 解压
$ tar -zxvf redis-5.0.2.tar.gz

```

2. 编译、安装、配置

```c
// 编译
$ make

// 安装
$ make PREFIX=/usr/local/redis install

// 配置
$ cp redis.conf /usr/local/redis/
// 修改 redis.conf 中的 daemonsize no 为 daemonsize yes

```

3. 启动、关闭

```c

// 启动
$ /usr/local/redis/bin/redis-server /usr/local/redis/redis.conf

// 查看
$ ps -ef | grep -i redis

// 关闭
$ /usr/local/redis/bin/redis-cli shutdown

```

4. 开放外部连接  

> 默认情况下：Jedis、Redis Desktop Manager 是连不上 redis-server 的。

（1） 修改 redis.conf 中的 bind 配置，重启 redis。bind 的默认是 127.0.0.1 只可本机访问，改为 bind 0.0.0.0 即所有地址均可访问。
（2） 关闭防火墙：systemctl stop firewalld.service


## Redis 的数据结构

* 五种数据类型：String \ Hash \ list \ set \ sorted set

* key 定义建议：不要太长、不要太短、统一命名规范

* String
  * 二进制存储
  * 值可以容纳 512 MB
  * 常用命令：赋值、取值、删除、数值增减、扩展命令

```c
// 赋值、取值示例
set hgy1 value1
get hgy1
getset hgy1 value1-change

// 删除
del hgy1

// 数值增减示例
incr N 
// 如果 N 存在并且能转成整型，则递增
// 如果 N 存在但是不能转成整型，则报错
// 如果 N 不存在，则创建 N, 赋值为 0 ，然后加 1

decr N
// 如果 N 存在并且能转成整型，则递增
// 如果 N 存在但是不能转成整型，则报错
// 如果 N 不存在，则创建 N, 赋值为 0 ，然后减 1

// 增加 5
incrby N 5

// 减去 5
decrby N 5

// 字符串追加（连接）
append N 123

```

* Hash
  * K-V 容器
  * 每一个 Hash 变量可以存储 4294967295 个键值对

```c
// 赋值
hset hgyhash username 张三丰
hset hgyhash age 18
hmset hgyhash2 username rose age 21

// 取值
hget hgyhash usernam
hmget hgyhash2 username age
hgetall hgyhash2

// 删除
hdel hgyhash2 username
del hgyhash2

// 数字增减
hincrby hgyhash age 5
```

* list
  * 有序、允许重复
  * ArrayList 使用方式
  * LinkedList 使用方式

```c
// 两端添加
lpush list1 a b c
lpush list1 1 2 3
rpush list2 a b c
rpush list2 1 2 3

// 查看列表
lrange list1 0 5
lrange list1 0 -1

// 弹出
lpop list1
rpop list2

// 获取元素个数
llen list2


```

* Set
  * 有序
  * 不允许重复
  * 每一个 Set 变量可以存储 4294967295 个元素
  * 集合运算

```c
// 赋值
sadd set1 a b c
sadd set1 1 2 3

// 取值
smembers set1

// 查看数量
scard set1

// 删除值
srem set1  1 2

// 集合运算
sdiff set1 set2
sinter set1 set2
sunion set1 set2
```

* Sorted-Set
  * 不允许重复
  * 自动排序

```c
// 赋值
zadd sort1 10 a 20 b 30 c

// 删除值
zrem sort1 a

// 查看
zrange sort1 0 -1
zcard sort1
zrange sort1 0 -1 withscores
```

* 常用 Keys 操作

```c
// 查看所有 key
keys *

// 匹配 key
keys *set*

// 查看 key 是否存在
exists set1

// 重命名key
rename set1 newset1

// 设置过期时间
expire newset1 1000

// 查看剩余时间
ttl newset1

// 查看类型
type newset1

```

## Redis 特性

* 多数据库：一个 redis 实例最多提供 16 个数据库，默认连接 0 号数据库。

```c
// 选择数据库
select 1

// 移动数据库0的数据 到 数据库1
move set2 1

```

* 事务：multi exec discard

## Redis 持久化

* RDB 方式
  * 在指定时间间隔内将内存中的数据写入磁盘

* AOF 方式
  * 以日志的形式记录 redis 的每一个操作

* 不持久化

* 同时使用 RDB 和 AOF






