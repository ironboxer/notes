### what's nohup



> On [Unix-like](https://www.computerhope.com/jargon/u/unix-like.htm) operating systems, the **nohup** command executes another command, and instructs the system to continue running it even if the session is disconnected.



## Description

When using the [command shell](https://www.computerhope.com/jargon/s/shell.htm), prefixing a command with **nohup** prevents the command from being aborted automatically when you [log out](https://www.computerhope.com/jargon/s/signoff.htm) or exit the shell.

The name **nohup** stands for "no hangup." The hangup (**HUP**) [signal](https://www.computerhope.com/unix/signals.htm), which is normally sent to a [process](https://www.computerhope.com/jargon/p/process.htm) to inform it that the user has logged off (or "hung up"), is intercepted by **nohup**, allowing the process to continue running.



进程组中的组长进程在退出的时候会向该进程组中的其他进程发送一个SIGHUP的信号，通知其他进程自己退出了，你们要不要退出自己决定。

使用 nohup 启动的进程会忽略这个HUP信号，或者是收到该信号的时候不执行退出的操作。

而不使用nohup启动的前台进程在收到HUP信号的时候会自动退出。



**Linux系统中常用信号：**
 （1）**SIGHUP：**用户从终端注销，所有已启动进程都将收到该进程。系统缺省状态下对该信号的处理是终止进程**。
 （2）**SIGINT：**程序终止信号。程序运行过程中，按`Ctrl+C`键将产生该信号。
 （3）**SIGQUIT：**程序退出信号。程序运行过程中，按`Ctrl+\\`键将产生该信号。
 （4）**SIGBUS和SIGSEGV：**进程访问非法地址。
 （5）**SIGFPE：**运算中出现致命错误，如除零操作、数据溢出等。
 （6）**SIGKILL：**用户终止进程执行信号。shell下执行`kill -9`发送该信号。
 （7）**SIGTERM：**结束进程信号。shell下执行`kill 进程pid`发送该信号。
 （8）**SIGALRM：**定时器信号。
 （9）**SIGCLD：**子进程退出信号。如果其父进程没有忽略该信号也没有处理该信号，则子进程退出后将形成僵尸进程。





https://www.computerhope.com/unix/unohup.htm

https://www.maketecheasier.com/nohup-and-uses/

https://www.jianshu.com/p/f64cd61d196c

