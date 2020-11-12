---
title: "CPython字节码"
author: 刘涛涛
marp: false
theme: uncover
output: powerpoint_presentation

---

CPython 中的字节码

> Python 是一种通用编程语言, CPython是其官方实现, CPython是一种解析+编译型的动态语言
> CPython并不直接通过解析语法树运行, 而是运行编译好的二进制代码, 这种二进制代码也叫做 - 字节码
> CPython是一种基于栈的编程语言,类似的编程语言还有Java(早期), Ruby, JavaScript...

---

三种栈

- call stack 负责调度函数
- evaluation stack (data stack) 负责调度函数中的语句
- block stack 负责记录异常发生时的上下文

---