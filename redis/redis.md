---
title: "进击的Redis"
author: 刘涛涛
marp: false
theme: uncover
output: powerpoint_presentation
---

# 进击的Redis

Redis的持续演进

---

# 什么是Redis
 
> Remote Dictionary Server
> 远程字典服务器

- 独立部署
- 存储服务

---

# Redis的标签

- 缓存
- 消息队列
- 分布式锁
- 高性能

---

# Redis中的常用数据结构

- String
- List
- Hash
- Set
- Sorted Set

---

# Redis中的高级数据结构

- BitMap
- GeoHash
- HyperLogLog
- Streams
- BloomFilter

---

# Redis中的常见应用场景

- 缓存
- 分布式锁
- 消息队列
- 限流

---
# 缓存

缓存设计
- 缓存雪崩
- 缓存穿透

不要设置缓存的过期时间都集中在同一个时间 大量的key删除命令是阻塞的

```
redis-cli -h 172.16.0.95 -p 9379 -a '4AHnmeur!W' -n 1 --scan --pattern 'subscription:cache:bidding:*' | xargs redis-cli -h 172.16.0.95 -p 9379 -a '4AHnmeur!W' -n 1 del
```
unlink
https://redis.io/commands/unlink


---

# Redis实现分布式锁

申请锁
```
redis> SET lockname uuid EX 60
"OK"
```
---

# Redis实现分布式锁

释放锁
```
if redis.call("get",KEYS[1]) == ARGV[1]
then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

---

分布式锁在公司项目中的应用

Mercury爬虫项目种子调用/监控查询
https://bitbucket.org/meta-sota/mercury/src/master/mercury/libs/lock.py

---

# Reids实现消息队列
如何保证消息队列的可靠性

消息队列的服务质量(Qos)
- At most once (0)
- At least once (1)
- Exactly once (2).

Redis原生的blpop的服务质量 at most once
Mercury爬虫系统中的消息队列服务质量 at least once
https://bitbucket.org/meta-sota/mercury/src/develop/mercury/models/job.py

---


# Reids实现消息队列

入队
```
rpush queue element
```

出队

```
blpop queue1 queue2 10
```

---

# Redis作为消息队列的应用场景

- celery Broker: https://docs.celeryproject.org/en/stable/getting-started/brokers/redis.html
- ELK 缓冲队列: 
https://www.elastic.co/guide/en/beats/filebeat/current/redis-output.html
https://www.elastic.co/guide/en/logstash/current/plugins-inputs-redis.html

---

# Redis限流 - 简单限流

> 一个用户一分钟内最多访问某个页面10次

```Python

client = redis.Redis()

def is_allow_access(pageid, userid, period=60, maxcount=10):
    key = f"acl:{pageid}:{userid}"
    now = int(time.time() * 1000)
    client.zadd(key, now, now)
    client.zremrangebyscore(key, 0, now - period * 1000)
    count = client.zcard(key)
    client.expire(key, period + 1)
    return count <= maxcount
```

---
# Redis限流 - 简单限流

> 一个用户一分钟内最多访问某个页面10次

开启管道 - 命令之间不能有任何依赖

```Python

client = redis.Redis()

def is_allow_access(pageid, userid, period=60, maxcount=10):
    key = f"acl:{pageid}:{userid}"
    now = int(time.time() * 1000)
    with client.pipeline() as pipe:
        pipe.zadd(key, now, now)
        pipe.zremrangebyscore(key, 0, now - period * 1000)
        count = pipe.zcard(key)
        pipe.expire(key, period + 1)
        _, _, count, _ = pipe.execute()
        return count <= maxcount
```
---

Redis 限流 - 漏斗限流

漏斗限流: 消费速度恒定, 生产速度是变量
令牌桶限流: 生产速度恒定, 消费速度是变量

https://github.com/brandur/redis-cell

```
CL.THROTTLE <key> <max_burst> <count per period> <period> [<quantity>]
```

```
CL.THROTTLE user123 15 30 60 1
               ▲     ▲  ▲  ▲ ▲
               |     |  |  | └───── apply 1 token (default if omitted)
               |     |  └──┴─────── 30 tokens / 60 seconds
               |     └───────────── 15 max_burst
               └─────────────────── key "user123"
```

```
127.0.0.1:6379> CL.THROTTLE pageid:userid 15 30 60 1
1) (integer) 0   # 0 表示允许，1表示拒绝
2) (integer) 16  # 漏斗容量capacity
3) (integer) 15  # 漏斗剩余空间left_quota
4) (integer) -1  # 如果拒绝了，需要多长时间后再试(漏斗有空间了，单位秒)
5) (integer) 2   # 多长时间后，漏斗完全空出来(left_quota==capacity，单位秒)
```

---
# Redis 统计 - HyperLogLog

- pfadd
- pfcount
- pfmerge

---

# Redis 统计 - HyperLogLog

```
127.0.0.1:6379> pfadd a 1 2 3
(integer) 1
127.0.0.1:6379> pfadd a 4 5 6
(integer) 1
127.0.0.1:6379> pfcount a
(integer) 6
127.0.0.1:6379> pfadd b 7 8 9
(integer) 1
127.0.0.1:6379> pfcount b
(integer) 3
127.0.0.1:6379> pfmerge c a b
OK
127.0.0.1:6379> pfcount c
(integer) 9
```

---

# Redis - Cuckoofilter

https://github.com/kristoff-it/redis-cuckoofilter

---

# Redis的安全问题

> 利用Redis的RDB文件把入侵者的计算机公钥存储在Redis所在机器上 无需验证即可登录

https://juejin.im/book/5c70dbbe51882562046911bc/section/5cad7788e51d456e5633ddac

---

# Redis 6 Features

- RESP3 - 更好的语义
- ACLs  - 更好的权限控制
- SSL   - 更好的安全性
- Client Side caching - 客户端缓存
- More ...

---

# Redis 6 - Client Side caching

配合客户端实现缓存



---