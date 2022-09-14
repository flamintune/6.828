# Sysinfo

## 实现 sysinfo 系统调用
- 收集正在运行的系统的信息
- 该系统调用需要带一个指针参数,struct sysinfo* p
- 在内核里面，要填充该指针指向的数据结构的字段
  - freemem field 空闲内存的字节数
  - nproc field 状态不是 UNUSED 的进程数量

-  通过 sysinfotest 来验证正确性

## hints
- 在user/user.h 中可能需要提前声明 struct sysinfo 的存在
- sysinfo需要拷贝到用户控件，实现方法具体 参考sys_fstat()(kernel/sysfile.c)和filestat(kernel/file.c) 看看他们是怎样使用copyout的
- 为了收集空闲内存的 数量，请在kalloc.c中添加一个func
- 为了收集进程的数量，在proc.c中添加一个函数

## 碰到的问题
- 如何向内核态传入一个指针
  - 指针大小为4个字节，所以我想就传一个int试一下
    - 看来是不行的！
    - 那么采取addr那个试一下，虽然好像是64位的
- 如何统计空闲内存的字节数？
  - 完全不知道。。
  - 这个时候就要看 xv6 book 了里面介绍了 run 这个数据结构用来存储空闲页
- 写到kernel/proc.c kernel./klloc.c的函数在sysproc中无法调用怎么办
  - 因为没有在 kernel/def.h 头文件中定义该函数，所以没有找到该函数
- 是否有必要在计算空闲内存的字节数/进程数加锁？
  - 在计算空闲内存的时候肯定是要加的，因为如果不加的话，其他系统调用也会对该数据结构kmem进行操作，从而导致空闲内存的数量上的变化，同理那计算进程的时候也是需要加的