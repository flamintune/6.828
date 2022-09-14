# Print a page table (easy)
- 为了帮助直观地看到riscv的pagetable，可能会对未来的debug有帮助，第二个任务就是写一个函数可以打印page table的内容

## 具体内容
- 实现函数 vmprint() 接受 pagetable_t 参数，并且打印pagetable按照下面的格式
- 在 exec.c 中插入 ,just before return argc 之前
  
  ```c
    if (p->pid == 1)
        vmprint(p->pagetable);
  ```

- 在返回 argc 之前，打印出第一个进程的 page table
- 通过 make grade 来验证正确
-  打印格式如下
第一行展示 vmprint 的参数
然后是每一行对饮一个 pte，包括一些次级页表，riscv是三级页表结构，用..表示所在层级

不要打印无效的页面 即 V == 0 的页面

## hints
- 可以把 vmprint() 放在 kernel/vm.c 里面
- 使用 macros 在 kernel/riscv.h 的末尾，傻逼了，这个是宏的意思
- freewalk函数 可以 会带来灵感
- 定义 vmprint 在 kernel/dfs.h 中 以便在 exec.c 中调用
- 使用 %p 在printf 中 来打印完整 64位十六进制pte和地址


## problem
- 在exec中调用 vmprint ，但是具体在which line捏
    
    看这个传递的参数嘛，它要传递一个 页表进去，就是说要等页表初始化完毕之后再传进去嘛。麻了，然而描述里面其实已经讲清楚惹得

- 怎样来实现 vmprint
  
  看freewalk(),已经阅读 xv6 book

## thinking
- 啊都是用的递归来做的捏，这可能是个有点疑惑的问题
- 然后就简单总结一下整个的流程吧
  - 1.遍历 pagetable 中的 512 个 pte，并检查它的flag标志位，根据 flag 标志位来打印
  - 2.然后如果遇到 RWX 的PTE，那么该PTE的PNN就是指向的下一级别的 pagetable 的地址