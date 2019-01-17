redis简易教程
==========

- [redis简易教程](#redis%E7%AE%80%E6%98%93%E6%95%99%E7%A8%8B)
  - [一.简介](#%E4%B8%80%E7%AE%80%E4%BB%8B)
  - [二.redis的安装](#%E4%BA%8Credis%E7%9A%84%E5%AE%89%E8%A3%85)
    - [2.1redis配置](#21redis%E9%85%8D%E7%BD%AE)
      - [语法](#%E8%AF%AD%E6%B3%95)
    - [2.2编辑配置](#22%E7%BC%96%E8%BE%91%E9%85%8D%E7%BD%AE)
      - [语法](#%E8%AF%AD%E6%B3%95-1)
  - [三.redis数据类型](#%E4%B8%89redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
    - [string](#string)
    - [Hash](#hash)
    - [Set(集合)](#set%E9%9B%86%E5%90%88)
    - [zset(sorted set:有序集合)](#zsetsorted-set%E6%9C%89%E5%BA%8F%E9%9B%86%E5%90%88)
  - [四.redis命令](#%E5%9B%9Bredis%E5%91%BD%E4%BB%A4)
      - [语法](#%E8%AF%AD%E6%B3%95-2)
    - [在远程服务上执行命令](#%E5%9C%A8%E8%BF%9C%E7%A8%8B%E6%9C%8D%E5%8A%A1%E4%B8%8A%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4)
      - [语法](#%E8%AF%AD%E6%B3%95-3)
      - [redis键（key）](#redis%E9%94%AEkey)
    - [Redis HyperLogLog](#redis-hyperloglog)
  - [五.redis发布订阅](#%E4%BA%94redis%E5%8F%91%E5%B8%83%E8%AE%A2%E9%98%85)
    - [5.1redis发布订阅架构](#51redis%E5%8F%91%E5%B8%83%E8%AE%A2%E9%98%85%E6%9E%B6%E6%9E%84)
    - [5.2redis发布订阅功能](#52redis%E5%8F%91%E5%B8%83%E8%AE%A2%E9%98%85%E5%8A%9F%E8%83%BD)
  - [六.redis事务](#%E5%85%ADredis%E4%BA%8B%E5%8A%A1)
    - [redis事务命令](#redis%E4%BA%8B%E5%8A%A1%E5%91%BD%E4%BB%A4)
  - [七.redis脚本](#%E4%B8%83redis%E8%84%9A%E6%9C%AC)
      - [语法](#%E8%AF%AD%E6%B3%95-4)
      - [示例](#%E7%A4%BA%E4%BE%8B)
      - [redis脚本命令：](#redis%E8%84%9A%E6%9C%AC%E5%91%BD%E4%BB%A4)
  - [八.redis连接](#%E5%85%ABredis%E8%BF%9E%E6%8E%A5)
      - [实例](#%E5%AE%9E%E4%BE%8B)
      - [redis 连接命令](#redis-%E8%BF%9E%E6%8E%A5%E5%91%BD%E4%BB%A4)
  - [九.redis数据备份与恢复](#%E4%B9%9Dredis%E6%95%B0%E6%8D%AE%E5%A4%87%E4%BB%BD%E4%B8%8E%E6%81%A2%E5%A4%8D)
      - [语法](#%E8%AF%AD%E6%B3%95-5)
      - [实例](#%E5%AE%9E%E4%BE%8B-1)
      - [恢复数据](#%E6%81%A2%E5%A4%8D%E6%95%B0%E6%8D%AE)
      - [Bgsave](#bgsave)
        - [实例](#%E5%AE%9E%E4%BE%8B-2)

## 一.简介
redis是一个开源的使用ANSI C语言编写，遵守BSD协议，支持网络，可基于内存亦可持久化的日志型，Key-Value数据库，并提供了多种语言的API。
它通常被称为数据结构服务器，因为值（Value）可以是字符串（String），哈希（Map），列表（List），集合（sets）和有序集合（sorted sets）等类型。

其相对于其他key-value有以下三个特点：
<li>redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
<li>redis支持数据的备份，即master-slave模式的数据备份。

redis的优势
<li>性能极高
<li>丰富的数据类型
<li>原子
<li>丰富的特性

## 二.redis的安装
在官网下载对应版本安装文件安装即可。
在win10下，一般安装在C:\Program Files\Redis，然后进入该文件夹，运行redis
```
redis-cli.exe -h 127.0.0.1 -p 6379
```

### 2.1redis配置
redis配置文件位于redis安装目录下，文件名为redis.conf,用户也可以通过CONFIGF命令来查看和设置配置项

#### 语法
```
CONFIG GET CONFIG_SETTING_NAME
```

**for example**:

`CONFIG GET loglevel`

使用*来获取所有的配置项：
**for example**：

`CONFIG GET *`

### 2.2编辑配置
你可以直接在redis.conf文件中进行修改，也可以使用CONFIG set命令来修改配置。

#### 语法
`CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE`

**for example**：

`CONFIG SET loglevel "notice"`

## 三.redis数据类型
redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。
### string
string是redis最基本的类型，你可以理解成Memcached一模一样的类型，一个key对应一个一个value。string类型是二进制安全的，意思是redis的string可以包含任何数据类型，比如jpg图片或者序列化对象。string类型的最大存储512mb。

**for example**

`SET name "runoob"`

`GET name`

在以上实例中我们使用了 Redis 的 SET 和 GET 命令。键为 name，对应的值为 runoob。

注意：一个键最大能存储512MB。

### Hash
Redis hash 是一个键值(key=>value)对集合。

Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。
**for example**

`HMSET myhash field1 "Hello" field2 "World"`

`HGET myhash field1`

`HGET myhash field2`

实例中我们使用了 Redis HMSET, HGET 命令，HMSET 设置了两个 field=>value 对, HGET 获取对应 field 对应的 value。

每个 hash 可以存储 $2^{32} -1$ 键值对（40多亿）

### Set(集合)
Redis的Set是string类型的无序集合。
集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)

**sadd 命令**
添加一个 string 元素到 key 对应的 set 集合中，成功返回此次加入的元素个数，如果元素已经在集合中返回 0，如果 key 对应的 set 不存在则返回错误。

`sadd key member [member]`

`smembers runoob`

### zset(sorted set:有序集合)
Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。
不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数(score)却可以重复。


**zadd命令**

`zadd key score member`

## 四.redis命令
redis命令用于在redis服务上执行操作。

要在redis服务上执行命令需要一个redis客户端。redis客户端在安装包中。

#### 语法
redis客户端的基本语法：
`redis-cli`

### 在远程服务上执行命令
#### 语法
`redis-cli -h host -p port -a password`

**for example:**

redis-cli -h 127.0.0.1 -p 6379 -a "mypass"

#### redis键（key）
redis键命令用于管理redis的键

### Redis HyperLogLog
Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素

**什么是基数**

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。

## 五.redis发布订阅
Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。

Redis 客户端可以订阅任意数量的频道。
### 5.1redis发布订阅架构
Redis提供了发布订阅功能，可以用于消息的传输，Redis的发布订阅机制包括三个部分，发布者，订阅者和Channel。

发布者和订阅者都是Redis客户端，Channel则为Redis服务器端，发布者将消息发送到某个的频道，订阅了这个频道的订阅者就能接收到这条消息。Redis的这种发布订阅机制与基于主题的发布订阅类似，Channel相当于主题。

### 5.2redis发布订阅功能

（1）发送消息 
Redis采用PUBLISH命令发送消息，其返回值为接收到该消息的订阅者的数量。

（2）订阅某个频道 
Redis采用SUBSCRIBE命令订阅某个频道，其返回值包括客户端订阅的频道，目前已订阅的频道数量，以及接收到的消息，其中subscribe表示已经成功订阅了某个频道。

（3）模式匹配 
模式匹配功能允许客户端订阅符合某个模式的频道，Redis采用PSUBSCRIBE订阅符合某个模式所有频道，用“”表示模式，“”可以被任意值代替。假设客户端同时订阅了某种模式和符合该模式的某个频道，那么发送给这个频道的消息将被客户端接收到两次，只不过这两条消息的类型不同，一个是message类型，一个是pmessage类型，但其内容相同。
（4）取消订阅 
Redis采用UNSUBSCRIBE和PUNSUBSCRIBE命令取消订阅，其返回值与订阅类似。 
由于Redis的订阅操作是阻塞式的，因此一旦客户端订阅了某个频道或模式，就将会一直处于订阅状态直到退出。在SUBSCRIBE，PSUBSCRIBE，UNSUBSCRIBE和PUNSUBSCRIBE命令中，其返回值都包含了该客户端当前订阅的频道和模式的数量，当这个数量变为0时，该客户端会自动退出订阅状态。

## 六.redis事务
redis事务可以一次执行多个命令，并且带有以下三个重要的保证：

+ 批量操作在发送exec命令前被放入队列缓存。
+ 收到exec命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
+ 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。

一个事务从开始到执行会经历以下三个阶段：

+ 开始事务，
+ 命令入队。
+ 执行事务。
  

**for example：**
以下是一个事务的例子，它先以MULTI开始一个事务，然后将多个命令入队到事务中，最后由EXEC命令触发事务，一并执行事务中的所有命令：

```
redis 127.0.0.1:6379> MULTI
OK

redis 127.0.0.1:6379> SET book-name "Mastering C++ in 21 days"
QUEUED

redis 127.0.0.1:6379> GET book-name
QUEUED

redis 127.0.0.1:6379> SADD tag "C++" "Programming" "Mastering Series"
QUEUED

redis 127.0.0.1:6379> SMEMBERS tag
QUEUED

redis 127.0.0.1:6379> EXEC
1) OK
2) "Mastering C++ in 21 days"
3) (integer) 3
4) 1) "Mastering Series"
   2) "C++"
   3) "Programming"
```
单个redis命令是原子性的，但redis没有在事务上增加任何维持原子性的机制，所以redis事务的执行并不是原子性的。
事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。

```
redis 127.0.0.1:7000> multi
OK
redis 127.0.0.1:7000> set a aaa
QUEUED
redis 127.0.0.1:7000> set b bbb
QUEUED
redis 127.0.0.1:7000> set c ccc
QUEUED
redis 127.0.0.1:7000> exec
1) OK
2) OK
3) OK
```

如果在 set b bbb 处失败，set a 已成功不会回滚，set c 还会继续执行。

### redis事务命令
| 序号 | 命令及描述                                                                                                         |
| :--- | :----------------------------------------------------------------------------------------------------------------- |
| 1    | discard 取消事务，放弃执行事务块内的所有命令                                                                       |
| 2    | exec执行所有事务块内的命令                                                                                         |
| 3    | multi标记一个事务块的开始                                                                                          |
| 4    | unwatch取消watch命令对所有key的监视。                                                                              |
| 5    | WATCH key [key ...] 监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。 |

## 七.redis脚本
redis脚本使用Lua解释器来执行脚本。redis2.6版本通过内嵌支持Lua环境。执行脚本的常用命令为EVAL

#### 语法
Eval命令的基本语法如下：
```
redis 127.0.0.1:6379> EVAL script numkeys key [key ...] arg [arg ...]
```
#### 示例
以下实例演示了redis 脚本工作过程：
```
redis 127.0.0.1:6379> EVAL "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}" 2 key1 key2 first second

1) "key1"
2) "key2"
3) "first"
4) "second"
```

#### redis脚本命令：
下表列出了redis脚本常用命令：

| 序号 | 命令及描述                                                                |
| :--- | :------------------------------------------------------------------------ |
| 1    | EVAL script numkeys key [key ...] arg [arg ...]执行 Lua 脚本。            |
| 2    | EVALSHA sha1 numkeys key [key ...] arg [arg ...]执行 Lua 脚本。           |
| 3    | SCRIPT EXISTS script [script ...]查看指定的脚本是否已经被保存在缓存当中。 |
| 4    | SCRIPT FLUSH从脚本缓存中移除所有脚本。                                    |
| 5    | SCRIPT KILL杀死当前正在运行的 Lua 脚本                                    |
| 6    | SCRIPT LOAD script将脚本 script 添加到脚本缓存中，但并不立即执行这个脚本  |

## 八.redis连接
redis连接命令主要是用于连接redis服务。
#### 实例
以下实例演示了客户端如何通过密码验证连接到redis服务，并检测服务是否在运行：
```
redis 127.0.0.1:6379> AUTH "password"
OK
redis 127.0.0.1:6379> PING
PONG
```

#### redis 连接命令
下表列出了redis连接的基本命令：
| 序号 | 命令及描述                                                                                      |
| :--- | :---------------------------------------------------------------------------------------------- |
| 1    | <a href='http://www.runoob.com/redis/connection-auth.html'>AUTH PASSWORD</a>验证密码是否正确    |
| 2    | <a href='http://www.runoob.com/redis/connection-echo.html'>ECHO message</a>打印字符串           |
| 3    | <a href='http://www.runoob.com/redis/connection-ping.html'>PING</a>查看服务是否运行             |
| 4    | <a href='http://www.runoob.com/redis/connection-quit.html'>QUIT</a>关闭当前连接                 |
| 5    | <a href='http://www.runoob.com/redis/connection-select.html'>SELECT index</a>切换到指定的数据库 |

## 九.redis数据备份与恢复
redis SAVE命令用于创建当前数据库的备份。
#### 语法
`redis 127.0.0.1:6379> SAVE `
#### 实例
```
redis 127.0.0.1:6379> SAVE 
OK
```
该命令将在redis安装目录中创建dump.rdb文件。

#### 恢复数据
如果需要恢复数据，只需要将备份文件（dump.rdb）移动到安装目录并启动服务即可。获取redis目录可以使用CONFIG命令，如下：
```
redis 127.0.0.1:6379> CONFIG GET dir
1) "dir"
2) "/usr/local/redis/bin"
```
#### Bgsave
创建 redis 备份文件也可以使用命令 BGSAVE，该命令在后台执行。
##### 实例
```
127.0.0.1:6379> BGSAVE

Background saving started
```






