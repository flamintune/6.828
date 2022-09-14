# Speed up system calls (easy)
- 一些OS会通过一块只读区域在用户空间和内核来共享数据，从而来加快syscall 的速度
- 这样做当需要系统调用的时候，就会减少内核切换的需要，相当于加了个cache？
- 为了帮助理解如何向pagetable里面插入一个映射，第一个任务就是实现getpid系统调用的优化
  
***
md 这就是实验教程来着草
实验教程：当每个进程被创建时，usyscall 映射 一个制度也，在这个页面的开始，存储结构体usyscall
- 当每个进程被创建的时候，会在USYSCALL中映射一个只读的page
  (A va in kernel/memlayout.h)
- 在这个page的开头，会存储一个 struct usyscall 并且初始化该结构，以用来存储当前进程的PID
- ugetpid() 在用户空间提供了，同时能自动得使用 Usyscall 的映射
- 通过 run pgtbltest来测试用例

***

## hints
- 在 proc_pagetable() (kernel/proc.c) 中 执行映射
- 选择 许可位  允许用户空间来只读 page
- mappages() 可能会很有用
- 不要忘记在 allocproc()中分配以及初始化page
- 确保释放掉了 page in freeproc()


## problem
- 遇到了 panic : freewalk leaf
  
  原始大概是因为，虚拟内存部分没有free干净！


## 感悟
- 知道了怎样去映射一个页面，通过mappage,mappage的实现就是不断从CPU提供的分页硬件里面找到一个pte，并映射，这里由于是在qemu上面运行，所以的话实现是利用了一个函数来模拟 walk
- 以及对于内核里面的分配内存，一定要记得即时分配，否则就会panic
- 还有虚拟内存的模型，内核的虚拟内存一般是恒等映射，而进程的有自己独立的地址空间，最上面是一个蹦床页面，trapframe page，具体看各个操作系统自己的设计。