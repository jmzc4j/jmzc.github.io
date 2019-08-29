---
title: Redis命令
date: 2019-08-28 17:01:17
tags: redis
categories: 学习笔记
---
### what is
- Redis全称为 Remote Dictionary Server;
- Redis是一个遵守BSD协议的开源的基于内存的数据结构存储服务；可以用做数据库、缓存和消息中间件；（官方）
- Redis是意大利人用C语言编写的一个高性能的（key/value）分布式的NoSQL数据库，支持多种数据类型并支持持久化；

### how to use it
#### 通用命令
- `dbsize`
- `select dbIndex`
- `flushdb`
- `flushall`
- `keys *`
- `exists key`
- `move key dbIndex`
- `expire key`：设置key的存活时间（秒单位）
- `ttl key`：查看key的剩余存活时长，返回值含义（-1，永恒；-2，过期），过期就会被从内存中删除该key
- `persist key`：去掉有效期设置
- `type key`

##### string 常用
- `set/get/del/append/strlen key`
- `incr/decr key`
- `incrby/decrby key increment`
- `getrange/setrange key startIndex endIndex`：获取key对应的指定范围的value，第一个索引为0，最后一个索引为-1
- `setex(with expire) key second value`
- `setnx(if not exist) key value`
- `mset/mget/msetnx key` ：添加、获取多个kv
- `getset`

##### list 常用
- `lpush/rpush/lrange`
- `lpop/rpop`
- `lIndex key`
- `lrem key count value`
- `ltrim key start end` ；截取start到end的元素再赋值给原来的key
- `rpoplpush srcList targetList`：当scr与target相同就构成了一个循环
- `lInsert key before/after v1 v2`
- `lset key index value`

##### set 常用
- `sadd `
- `smembers key`
- `sismember key v1`
- `scard`：获取元素个数
- `srem key value`
- `srandmember key num`：随机取出num个元素
- `spop key`
- `smove key1 key2 value-in-key1`：将key1中的某个值移动到key的集合中
- `sdiff key1 key2`：获取在key1集合但不在key2集合的元素
- `sinter key1 key2`
- `sunion key1 key2`

#### hash 常用
- `hset/hget/hdel/hmset/hmget/hgetall`
- `hlen`
- `hexists key in-key`
- `hkeys/hvals`
- `hincrby/hincrbyfloat`
- `hsetnx`

#### zset 常用
- `zadd/zrange`：zadd zset1 60 v1 70 v2 80 v3 90 v4 100 v5
- `zrangebyscore key startScore endScore`：默认取左闭右闭区间，可以使用左小括号来更改为开区间
- `zcard/zcount key score1 score2`
- `zrank key value`：获取value在key集合中的排名，即获取下标
- `zscore key value`
- `zrevrange`
- `zrevrank key value`
- `zrevrangebyscore key endScore startScore`
