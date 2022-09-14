# Detecting which pages have been accessed (hard)

一些 gc 可以受益于 一些有关 哪一些page被访问了的信息。在这个实验中，需要给  xv6 添加一个新特性，检测并报告给用户空间通过检查 pagetable 里面的 access bit，walke会标记这些bits在PTE里面的，当它解决了一个TLB miss之后

## 任务描述
-  实现函数 pgaccess()，一个系统调用用来报告那些页面被访问了的
-  这个系统调用 has 3 arguments
    
 -  要检查的用户第一页的va 虚拟地址
 -  要检查的 page number （❌） 是页数总共的页数
 -  用户地址buffer，用来保存结果，bitmask，掩码
  
 -  通过 pgtbltest来验证

## hints

- 开始实现 sys_pgaccess()
- 需要解析参数 使用 argaddr() 或者 argint()
- 对于输入的 bitmask，用一个临时性的在内核中的buffer然后再copy到用户空间去可能会比较简单一些
- ~~设置一个 上面的界限 在page number 可以被扫描的地方~~ 
  可以设置可以扫描的页数上限。
- walk函数在找到正确的pte上非常有用
- 你可能会需要define PTE_A,the access bit in kernel/riscv.h，翻阅 riscv 手册来决定它的value
- 保证及时清楚PTE_A 在检查它是否被设置之后。否则决定page是否被访问是不可能的，是因为上一次 pgaccess 被调用的
- vmprint() 在debug的时候会排上用处


## problem
- pgaccess() 应该写在哪个文件里面？
  大概率是 vm.c 
- pgaccess() 参数怎么写？
  主要是参数的数据类型！完蛋热，不想写了，好热...
  那算了 今天就到这
  妈个蛋，又是英语不好带来的误解题意
- 然后是最困难的一个大方面 —— 实现 pgaccess 函数
  - 首先是位置的问题，放在vm里面，可能不行，或者说需要改一下，多传一个参数进去？
- 对于walk 函数的 仔细分析
  
  - PX(level,va) (((uint64) (va)) >> PXSHIFT(level)) & PXMASK
  - PXSHIFT(level) PGSHIIFT + (9*level)
    
    这里的pgshift = 12, 是 page 的偏移量的位数，44 + 12 = 64，12 + 9 * level  level -> 1,3  ===> 21,30,39 得到每一层的移位次数后面再 & pxmask 1 1111 1111 取最后9位，得到某一层的pagetable的地址
  - 所以是先从level2 开始，逐渐到level0，如果没有找到的话，就回退出，找得到的话，，就会修改pagetable的值，并且遍历二级页表

- md，真是大无语了，因为没有初始化，导致排查了半天，nmmd