# sleep (easy)

## hints

- hot to pass the arguments by echo.c . grep.c rm.c

  - grep.c

    首先需要了解grep是用来干嘛的，用来做搜索的

    然后就是还处理了正则表达式

    还有就是根据argc的值来做出不同的处理

  - rm.c

- 忘记传参要报错误信息

- 传进来的是字符，可以用atoi转换成数字

- 使用系统调用 sleep

- 查看系统调用sleep的代码

  - argint() xv6内置函数，作用：传递参数from user to kernel
  - acquire and release 🔒机制。。

## 提交

- 首先创建sleep.c，然后在makefile里面的UPROGS添加sleep，然后make qemu编译自己的xv6，然后可以人工手动测试 sleep，然后就是通过提供的grade来测评

  make grade 会开始测试整个lab所有的test

  make GRADEFLAGS=sleep grade 只会测试sleep对应的test

