TCP三次握手的过程， 其实走的就是请求/响应的模式，说明一点，如果想要实现可靠通信，就要采用请求/响应的模式.

TCP之所以是三次握手，是因为中间的ACK和SYN 合并在一起发送了。

### TCP 的设计要求
**在异步的网络之上建立同步的点对点对话模型**

所谓**异步**体现在以下几个方面:
- 数据收发的顺序不一致, 先发送的数据可能会后到
- 通信的双方都可以同时连发几个数据包, 也可以连续接受几个数据包


### 思路应该是这样的：
1. 我们要实现可靠通信
2. 实现可靠通信的最常用方式就是 请求/响应 模式
3. 所以TCP的连接建立的过程就是采用 请求/响应 模式

### TCP 连接建立的过程如下:
1. A向B发送连接建立的请求
2. B接收到该请求，返回接受响应
3. B向A发送连接建立的请求
4. A接受到该请求，返回接受响应

观察发现：如果B当前是可以接受新的连接请求的情况下，2.3步是可以合并为一步的，所以TCP建立连接的消息发送次数从4次变成了3次。

这里有一个问题，命名1，2步就已经明确了A，B都是存在的，可以建立连接，那么为什么还要3，4步呢？

原因就是 TCP通信是双向的，全双工的。

双向意味着 A，B之间可以相互发送/接受消息，全双工意味着可以同时做这两件事情。

1，2步成功，意味着A可以向B发送消息，即A写消息，B读消息
3，4步成功，意味着B可以向A发送消息，即B写消息，A读消息

1，2步中，B返回一个响应，意味着B成功接受到了A发送的消息，说明两点：1）A发送消息成功了，2）B接受消息成功了，但是这并不意味着B可以向A发送消息，并且A可以成功接受B发送的消息。

### 消息队列
消息队列的可靠性包括很多方面, 其中包括消息的确认机制:
1. 自动确认 - 不可靠
2. 手工确认 - 可靠但是性能受限
3. 批量确认 - 可靠性和性能的权衡


参考
https://juejin.im/post/5b29d2c4e51d4558b80b1d8c
