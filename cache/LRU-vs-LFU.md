### LRU VS LFU

Least recently used VS least frequently used



一个基于访问顺序

一个基于访问频率



Let's consider a constant stream of cache requests with a cache capacity of 3, see below:

```
A, B, C, A, A, A, A, A, A, A, A, A, A, A, B, C, D
```

If we just consider a **Least Recently Used (LRU)** cache with a HashMap + doubly linked list implementation with O(1) eviction time and O(1) load time, we would have the following elements cached while processing the caching requests as mentioned above.

```
[A]
[A, B]
[A, B, C]
[B, C, A] <- a stream of As keeps A at the head of the list.
[C, A, B]
[A, B, C]
[B, C, D] <- here, we evict A, we can do better! 
```

When you look at this example, you can easily see that we can do better - given the higher expected chance of requesting an A in the future, we should not evict it even if it was least recently used.

```
A - 12
B - 2
C - 2
D - 1
```

**Least Frequently Used (LFU)** cache takes advantage of this information by keeping track of how many times the cache request has been used in its eviction algorithm.



> LRU is a cache eviction algorithm called **least recently used cache**.

Look at this [resource](https://apacheignite.readme.io/v1.0/docs/evictions)

> LFU is a cache eviction algorithm called **least frequently used cache**.
>
> It requires three data structures. One is a hash table that is used to cache the key/values so that given a key we can retrieve the cache entry at O(1). The second one is a double linked list for each frequency of access. The max frequency is capped at the cache size to avoid creating more and more frequency list entries. If we have a cache of max size 4 then we will end up with 4 different frequencies. Each frequency will have a double linked list to keep track of the cache entries belonging to that particular frequency. The third data structure would be to somehow link these frequencies lists. It can be either an array or another linked list so that on accessing a cache entry it can be easily promoted to the next frequency list in time O(1).
>
> 

https://stackoverflow.com/questions/17759560/what-is-the-difference-between-lru-and-lfu