---
marp: false
theme: uncover
---

# Redis中的一些技术详解

> 剖析Redis命令的一些实现

---

# Redis 管道

使用redis的管道可以提高并发性能
![Redis pipe](/Users/lttzzlll/Documents/notes/images/57F1A005-74F5-4EA2-A339-FE02A60B0B4E.png)

---

一条简单的命令经历了什么
```
key = "a"
value = redis.get(key)
```

socket的读写/内核态用户态的切换/网络传输

```
"$3\r\nget\r\n$1a".encode("utf-8")
```

```
socket.send(key)
socket.recv()
```

---

![内核](https://user-gold-cdn.xitu.io/2018/8/28/1657e7a5a0a24ce3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

---

### HyperLogLog 实现

---

### 

---