# Alarm(hard)

## 任务

在本次练习中，需要给xv6 加上一个特性，会周期性的 提醒 一个进行它占用CPU的时间。这可能对于 计算有关的 进程 —— 那些想要限制他们的占用的CPU 时间的进程。或者对于那些想要计算，但需要花费定期的行为。更通俗来讲，你需要实现一个原生的用户层次的  中断/错误 处理程序。你可以使用一些东西 在应用中处理 page faults。alarmtest and usertests

你需要 添加一个  新的系统调用 sigalarm(interval,handler) 如果一个应用程序调用了 sigalarm(n,fn)，那么 在 每过 n 个 ticks 的CPU tiime，这个程序会继续运行。内核应该暂停 应用程序要执行的函数 fn。当 fn returns，这个应用程序应该会在它离开的地方重新执行。

a tick 是一个 相当随意的单位时间，通过由 多久 硬件计时器产生一次中断 决定。如果一个应用程序 call sigalarm(0,0)，这个内核应用暂定产生 定期的 alarm calls

user/alarmtest.c  加到 makefile 里面去，必须要提案加号系统调用才会编译成功

alarmtest 会 调用 sigalarm(2,periodic) 在 test0 中，请求 kernel 强制执行 periodiic /2ticks，并且 之后自旋等待，你看可以看见汇编 alarmtest中，可能会帮助debug



## 步骤

- 添加 系统调用  sigalarm(interval,handler) sigreturn