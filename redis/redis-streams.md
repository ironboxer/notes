---
marp: false
theme: uncover
---

# Redis5中的Streams

> https://redis.io/topics/streams-intro

---

# Streams是什么

- List
- Pub/Sub

- Kafka
- Log

支持分组, 多播, 可持久化的消息队列

---

# Streams基本操作-增删改查

1. xadd 追加消息
2. xdel 删除消息
3. xrange 获取消息列表，会自动过滤已经删除的消息
4. xlen 消息长度
5. del 删除 Stream

---

# 评价消息队列消息投送的服务质量(Qos)

- Qos(0) At most once    最多一次    List
- Qos(1) At least once   至少一次    Streams
- Qos(2) Exactly once    仅仅一次    RabbitMQ?

---


# Streams作为消息队列的结构示意图

![](https://user-gold-cdn.xitu.io/2018/6/1/163bae206a809d56?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


![](https://user-gold-cdn.xitu.io/2018/6/1/163bbe4d6f601f8e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

---

# Streams作为消息队列的优劣势

Pros:
- 简单,强大
- 成本低, 易部署

Cons:
- Qos(1)
- 最坏情况下丢失1秒的数据 

--