### Stream



> There is another very important detail in the command line above, after the mandatory **STREAMS** option the ID requested for the key `mystream` is the special ID `>`. This special ID is only valid in the context of consumer groups, and it means: **messages never delivered to other consumers so far**.



>- If the ID is the special ID `>` then the command will return only new messages never delivered to other consumers so far, and as a side effect, will update the consumer group *last ID*.
>- If the ID is any other valid numerical ID, then the command will let us access our *history of pending messages*. That is, the set of messages that were delivered to this specified consumer (identified by the provided name), and never acknowledged so far with **XACK**.





>**XREADGROUP** is a *write command* because even if it reads from the stream, the consumer group is modified as a side effect of reading, so it can be only called in master instances.

XACK也是一样的， 不要被其名字或者表面现象蒙蔽





>consumer may have crashed before, so in the event of a restart we want to read again messages that were delivered to us without getting acknowledged.





>consumers may permanently fail and never recover
>
>Redis consumer groups offer a feature that is used in these situations in order to *claim* the pending messages of a given consumer so that such messages will change ownership and will be re-assigned to a different consumer. The feature is very explicit, a consumer has to inspect the list of pending messages, and will have to claim specific messages using a special command, otherwise the server will take the messages pending forever assigned to the old consumer, in this way different applications can choose if to use such a feature or not, and exactly the way to use it.





>```
>XCLAIM <key> <group> <consumer> <min-idle-time> <ID-1> <ID-2> ... <ID-N>
>```





>Basically we say, for this specific key and group, I want that the message IDs specified will change ownership, and will be assigned to the specified consumer name ``. However, we also provide a *minimum idle time*, so that the operation will only work if the idle time of the mentioned messages is greater than the specified idle time. This is useful because maybe two clients are retrying to claim a message at the same time:



​	

>However claiming a message, as a side effect will *reset its idle time*! And will *increment its number of deliveries counter*, so the second client will fail claiming it. In this way we avoid trivial re-processing of messages (even if in the general case you cannot obtain exactly once processing).

这句话你读懂了吗?



> 重设idle time之后, 其他等待的客户端的时间变长了, 间接的, 数量变多了。



>Claiming may also be implemented by a *separate process*: one that just checks the list of pending messages, and assigns idle messages to consumers that appear to be active. Active consumers can be obtained using one of the observability features of Redis streams. 



### Redis 的路由策略 VS Kafka的路由策略

> We could say that schematically the following is true:
>
> - If you use 1 stream -> 1 consumer, you are processing messages **in order**.
> - If you use N streams with N consumers, so that only a given consumer hits a subset of the N streams, you can scale the above model of 1 stream -> 1 consumer.
> - If you use 1 stream -> N consumers, you are load balancing to N consumers, however in that case, messages about the same logical item may be consumed **out of order**, because a given consumer may process message 3 faster than another consumer is processing message 4.



> So basically Kafka partitions are more similar to using N different Redis keys. While Redis consumer groups are a server-side load balancing system of messages from a given stream to N different consumers.



### 	MAXLEN 的参数



>```
>XADD mystream MAXLEN ~ 1000 * ... entry fields here ..
>```

>The `~` argument between the **MAXLEN** option and the actual count means, I don't really need this to be exactly 1000 items. It can be 1000 or 1010 or 1030, just make sure to save at least 1000 items. 

>保留 ***至少*** 1000个元素
>
>

### *

>Finally the special ID `*`, that can be used only with the [XADD](https://redis.io/commands/xadd) command, means to ***auto*** select an ID for us for the new entry.

### >

>ID is the *last delivered ID* of a consumer group.
>
>only related to consumer groups and only when the [XREADGROUP](https://redis.io/commands/xreadgroup) command is used



### Zero length streams

>
>
>The reason why such an asymmetry exists is because Streams may have associated consumer groups, and we do not want to lose the state that the consumer groups define just because there are no longer items inside the stream. Currently the stream is not deleted even when it has no associated consumer groups, but this may change in the future.