### Python 后台线程的设置

在Python中如果开启一个线程的时候设置了 deamon = True, 那么在主线程退出的时候, 该子线程也会退出.


```Python
import threading

def print_something(something):
    print(something)


t = threading.Thread(target=print_something, args=("hello",))
t.daemon = True
t.start()
print("thread started")
```


让子线程中的任务休眠一段时间, 因为主线程运行完整个进程就会退出, 所以不等待子线程中的任务执行完, 整个进程就结束了.

```Python
import time
import threading

def print_something(something):
    time.sleep(10)
    print(something)


t = threading.Thread(target=print_something, args=("hello",))
t.daemon = True
t.start()
print("thread started")
```

所以如果想让子线程中的任务执行完后主线程再退出, 就需要让主线程等待子线程执行完后再退出.

```Python
import time
import threading

def print_something(something):
    time.sleep(10)
    print(something)


t = threading.Thread(target=print_something, args=("hello",))
t.start()
print("thread started")
t.join()
```

```join```的作用

```
In [20]: ?threading.Thread.join
Signature: threading.Thread.join(self, timeout=None)
Docstring:
Wait until the thread terminates.
```
