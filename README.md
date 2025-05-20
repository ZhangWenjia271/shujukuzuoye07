# shujukuzuoye07  
## 非关系型数据库Redis的安装和简单操作  

### 1. Redis的简介  
Redis是一个key-value存储系统。和Memcached类似，它支持存储的value类型相对更多，包括string（字符串）、list（列表）、set（集合）、zset（sorted set—有序集合）和hash（哈希）类型。  
Redis会周期性地把更新的数据写入磁盘或者把修改操作写入追加的记录文件。  
Redis是一个高性能的key-value数据库。Redis的出现，很大程度补偿了memcached这类key/value存储的不足，在部分场合可以对关系数据库起到很好的补充作用。它提供了Java，C/C++，C#，PHP，JavaScript，Perl，Object-C，Python，Ruby，Erlang等客户端，使用很方便。  


### 2. Redis的安装和启动  
安装Redis后，在Redis的安装目录下打开cmd窗口，然后执行下方命令来启动Redis服务：  
```bash
redis-server.exe redis.windows.conf
```
![屏幕截图 2025-05-19 223733](https://github.com/user-attachments/assets/d244f4d5-35cc-47fc-acc5-62b6bc40c977)

默认端口为6379，出现图上的图标说明Redis服务启动成功。  

为了方便，建议把Redis路径配置到系统变量Path值中，这样就省得再输路径了。  

![屏幕截图 2025-05-19 223438](https://github.com/user-attachments/assets/57862baf-ad11-4c79-af6a-83d23585a0f4)

接下来使用`redis-cli.exe`命令来打开Redis客户端进行连接（注意：如果出现连接不成功，要注意服务打开以后，需要另启一个cmd窗口到Redis所在的目录执行命令，原来的Redis启动窗口不要关闭，不然就无法访问服务端了）。  
链接成功后，在命令中输入`ping`命令来检测Redis服务器与Redis客户端的连通性，返回`PONG`则说明连接成功了。  

![屏幕截图 2025-05-19 224228](https://github.com/user-attachments/assets/42397c83-d80d-4e85-bf42-5a5742f21acd)

如果连接成功，到此Redis的安装和部署也就完成了。  


### 3. Redis的数据的简单操作  
对于Redis来说，其数据库特点为：  
- 数据库没有名称，默认拥有16个数据库，通过0-15来表示；  
- `Select 3`表示选择3号数据库；  
- `Select 0`表示选择0号数据库；  
- 登录到Redis客户端，如果没有做任何select操作，默认使用0号数据库；  

Redis键值对说明：  
- redis是key-value的数据结构；  
- 每一条数据都是一个键值对；  
- 键的类型是字符串；  
- 值的类型有五种：  
  - 字符串 string；  
  - 哈希 hash；  
  - 列表 list；  
  - 无序集合 set；  
  - 有序集合zset（sorted set）。  

对于Redis中的五种不同数据类型，其有对应不同的增删改查等操作命令，大致如下：  
![787452-20180322151943060-1380422799](https://github.com/user-attachments/assets/c8acc9ae-3bc5-4463-833f-88cb757ffeef)


#### （1）字符串string  
- 字符串（string）是Redis最基本的类型；  
- String的增加和修改：`set 键 值`（如果键不存在就是添加，如果存在就是修改值）；  
- String设置多个键值对：`mset 键1 值1 键2 值2 键3 值3`；  
- String追加值：`append 键 值`；  
- String获取值：`get 键`（如果键不存在，显示空行）；  
- String获取多个值：`mget 键1 键2`；  
- String删除：  
  - 【1】删除键：`del 键`；  
  - 【2】删除多个键：`del 键1 键2`；  
- String查找键：`keys 键`；而`keys *`表示显示所有键；  
- String判断键是否存在：`exists 键`（存在返回1，不存在返回0）；  
- String查看键对应的value类型：`type 键`。  

具体操作截图如下：  
![屏幕截图 2025-05-20 163943](https://github.com/user-attachments/assets/fd6d18e5-e6b9-407c-a8d5-4f3a20d78843)


#### （2）哈希hash  
- hash用于存储“键值”对集合；  
- 每个hash中的键可以理解为字段（field），一个字段（field）对应一个值（value）；  
- hash中的值（value）类型为字符串（string）；  
- 同一个hash中字段名（field）不可重复；  
- hash的增加和修改：`hset 键 字段 值`；  
- hash设置多个值：`hmset 键 字段1 值1 字段2 值2`；  
- hash查看键类型：`type 键`；  
- hash获取一个字段的值：`hget 键 字段`（如果键不存在，显示空行）；  
- hash获得多个字段的值：`hmget 键 字段1 字段2`；  
- hash获取所有字段的值：`hvals 键`；  
- hash获取所有的字段包括值：`hgetall 键`；  
- hash删除指定字段：`hdel 键 字段名`；  
- hash删除键：`del 键`。  

具体操作截图如下：  
![屏幕截图 2025-05-20 171246](https://github.com/user-attachments/assets/0cd66535-e2db-4521-9ca6-1c84969d95e5)


#### （3）列表list  
- 列表中的值（value）为字符串（string）；  
- 列表中的每个值按照添加的顺序排列。  
- list的增加：  
  - `lpush 键 值1 值2`（从列表左侧添加值）；  
  - `rpush 键 值1 值2`（从列表右侧添加值）；  
  - `linsert 键 before/after 值 插入的值`（从指定值前或后插入值）；  
- list的获取：返回列表里指定范围内的值：`lrange 键 start stop`  
  - 索引从左侧开始，第一个值的索引为0；  
  - 索引可以是负数，表示从尾部开始计数，如：-1表示最后一个值；  
  - start，stop为要获取值的索引；  
- list的修改：修改指定索引的值：`lset 键 索引 值`；  
- list的删除：删除指定的值：`lrem 键 count 值`  
  - 列表中前count次出现的值移除  
  - `count > 0` 从头往尾删；  
  - `count < 0` 从尾往头删；  
  - `count = 0` 删除所有值。  
  例如：  
  - 从键luser2列表左侧开始删除2个h1：`lrem luser2 2 h1`；  
  - 从键luser2 列表右侧开始删除1个h1：`lrem luser2 -1 h1`；  
  - 从键luser2列表中删除所有h0：`lrem luser2 0 h0`。  

具体操作截图如下：  
![屏幕截图 2025-05-20 193902](https://github.com/user-attachments/assets/56af4898-2adb-4964-ab3a-8b5e8e05c3fd)
![屏幕截图 2025-05-20 193734](https://github.com/user-attachments/assets/db2c3754-8695-45c7-9d0f-a927b6ad45d8)


#### （4）集合 set  
- 无序集合中值（value）类型为字符串（string）；  
- 集合里不允许有重复的值；  
- 说明：对于集合里的值只能添加与删除，不能修改；  
- set中添加值：`sadd 键 值1 值2 值3`；  
- set中查询值：`smembers 键`；  
- set中删除特定的值：`srem 键 值`。  

具体操作截图如下：  
![屏幕截图 2025-05-20 201934](https://github.com/user-attachments/assets/3663095c-b451-4ae7-9a73-782c3fed16f9)


#### （5）有序集合 zset  
- 有序集合中值（value）类型为字符串（string）；  
- 集合里不允许有重复的值；  
- 每个值都会关联一个分数（score），分数（score）可以为负数，通过分数（score）将值从小到大排序；  
- 说明：对于有序集合里的值只能添加与删除不能修改；  
- zset中添加值：`zadd 键 score1 值1 score2 值2 score3 值3`；  
- zset中获取值：  
  - 【1】返回指定范围内的值：`zrange 键 start stop (withscores)`  
    - start，stop为值的下标索引；  
    - 第一个值的索引为0；  
    - 索引可以是负数，表示从尾部开始计数，-1表示最后一个值；  
    - `withscores`：同时获取值对应的分数（score）。  
  - 【2】通过score获取值：`zrangebyscore 键 min max`  
    - min代表score的起始值；  
    - max代表score的末尾值；  
  - 【3】通过值得到score：`zscore 键 值`；  
- zset中删除值：  
  - 【1】删除指定的值：`zrem 键 值`；  
  - 【2】通过score删除值：`zremrangebyscore 键 min max`  
    - min是要删除score的最小值  
    - max是要删除score的最大值  

具体操作截图如下：
![屏幕截图 2025-05-20 210923](https://github.com/user-attachments/assets/8986574a-78d2-4662-940b-2b774bd576c7)

