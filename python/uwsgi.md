### uwsgi 的缺点

uwsgi必须要和部署的python项目在一台机器上，而论其他的性能，又比不过nginx，所以，要他干嘛呢？

https://serverfault.com/questions/590819/why-do-i-need-nginx-when-i-have-uwsgi



### uWSGI是什么

>
>
>uWSGI是一个Web服务，它实现了WSGI协议，此外还实现了uwsgi协议与http协议。
>
>需要区分一下WSGI、uWSGI与uwsgi三者的差别。
>
>- WSGI是一种协议
>- uwsgi同样也是一种协议，与WSGI没有什么关系
>- uWSGI是Web服务，它实现了WSGI协议与uwsgi协议
>
>uwsgi协议是uWSGI特有的，它用于定义传输信息的类型，每个uwsgi包的前4字节都用于记录传输信息类型的描述。
>
>

### uwsgi 协议历史分析

http://www.beiliangshizi.com/?p=503

https://juejin.im/post/5db45073518825645015338d



### uwsgi 的fork机制

>
>
>uWSGI is “Perlish” in a way, there is nothing we can do to hide that. Most of its choices (starting from “There’s more than one way to do it”) came from the Perl world (and more generally from classical UNIX sysadmin approaches).
>
>有时候其他语言/平台上使用这些方法会导致不在意料中的行为发生。
>
>当你开始学习 uWSGI 的时候一个你可能会面对的”问题”之一就是它的 fork() 使用。
>
>默认情况下 uWSGI 在第一个 spawned 的进程里加载你的应用，然后在这个进程里面调用 fork() 多次。
>
>这意味这你的应用被单独加载一次然后被复制。
>
>虽然这个方法加速了服务器的启动，但有些应用可能会因为这个技术造成一些问题(特别是这些在启动的 时候初始化数据库连接的，因为连接的文件描述符会在字进程中继承)。
>
>如果你确定应不应该使用 uWSGI 野蛮的预-fork方式，那就使用 –lazy-apps 选项禁用掉它。 它将会强制你的应用在每个 worker 里都会完整加载一次。



### uwsgi的调用方式

![image-20200509203912113](/Users/lttzzlll/Documents/notes/uwsgi.assets/image-20200509203912113.png)



也是父进程调用子进程的方式

https://blog.ixxoo.me/uwsgi-fork-trap.html



https://blog.csdn.net/pzqingchong/article/details/79522215