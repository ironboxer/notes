Unlike Windows systems, where creating a new process and executing a new process image happen in a single step, Linux and other UNIX-like systems do them as two distinct steps.

The `fork` function makes an exact duplicate of the calling process and actually returns twice, once to the parent process and once to the child process. The `execvp` function (and other functions in the `exec` family) executes a new process image in the same process, overwriting the existing process image.

You can call `execvp` without calling `fork` first. If so, that just means the currently running program goes away and is replaced with the given program. However, `fork` is **the way** to create a new process.





https://stackoverflow.com/questions/54152633/creating-a-child-process-without-fork





---

In Unix whenever we want to create a new process, we fork the current process, creating a new child process which is exactly the same as the parent process; then we do an exec system call to replace all the data from the parent process with that for the new process.

Why do we create a copy of the parent process in the first place and not create a new process directly?



https://unix.stackexchange.com/questions/136637/why-do-we-need-to-fork-to-create-new-processes





---





http://www.robelle.com/smugbook/process.html#:~:text=In%20UNIX%20and%20POSIX%20you,except%20for%20a%20few%20details).





