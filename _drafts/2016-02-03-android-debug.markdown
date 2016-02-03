---
layout: post
title: "Android Studio Debug(调式)"
categories: Android
---

1. 一些概念
Step Over
Step Into
Force Step Into
Step Out
Run to Cursor

Frames 函数调用堆栈. 一个frame表示一次函数调用
Evaluate Expression

Condition: 
Condition是一个boolean值或者一个boolean结果的表达式
只有当前线程满足Condition条件时，该线程才会在该断点处悬挂（Suspend），如果不满足条件的线程不会悬挂于该断点处。
Condition中可以是当前语句块中已有的变量的使用，也可以是Java代码的调用（如：Thread.currentThread().getName().equals("Thread$1")，这样只有线程名字为“Thread$1”的线程在该断点处悬挂（Suspend））

线程切换：
在Frames中切换线程

Reference
IntelliJ IDEA Debugging: https://www.jetbrains.com/idea/help/debugging.html
Evaluating Expressions: https://www.jetbrains.com/idea/help/evaluating-expressions.html
Debug Tool Window. Threads: https://www.jetbrains.com/idea/help/debug-tool-window-threads.html