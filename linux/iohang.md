### IOHANG



删除一个大文件的时候 有可能引起IOHANG.

尤其是在阿里云上。



IO HANG是个什么鬼？简单的说，就是服务器磁盘读写过慢，导致线程和进程挂起。大量读写线程/进程挂起导致服务器宕机......



---

如果出现了这种故障，就先禁止nginx的日志



完全禁止nginx日志输出到磁盘的方式(重定向)

```
access_log   /dev/null
error_log    /dev/null
```







![image-20200929211746767](iohang.assets/image-20200929211746767.png)





---



什么是linux的 /dev/null ?



One such example is /dev/null. It’s a **special file** that’s present in every single Linux system. However, unlike most other virtual files, instead of reading, it’s **used to write**. **Whatever you write to /dev/null will be discarded, forgotten into the void**. It’s known as the **null device** in a UNIX system.



这篇文章

https://linuxhint.com/what_is_dev_null/

