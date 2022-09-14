# Backtrace(moderate)

Backtrace 可以更好地用来帮助我们来debug，backtrace是一系列的在栈上的函数调用，这些函数都是出错的位置

## 任务

实现一个 backtrace() 函数 在 kernel/printf.c 中

并且在  sys_sleep 中插入 对该函数的 一个调用，然后 run bttest 来测试，该测试程序会调用 sys_sleep 函数

backtrace 应该使用 frame pointers 帧指针在 堆栈上行走，并打印每个堆栈帧中保存的返回地址

## hints

- gcc 编译器会保存 当前正在执行的 帧指针 到 寄存器 s0 当中去
- xv6 给每一个栈 分配了 一页 地址与 PAGE 对齐



#### think

- 这个 small lab 耽误了很久，但其实真的简单
- 这次实验再次向我证明了看 lab 的重要性
- 通过这次实验，了解了 riscv汇编，以及ｓｔａｃｋ　ｆｒａｍｅ到底是个什么东西，其实就是 一个栈上面分成了几块，用来保存 caller 的返回地址

