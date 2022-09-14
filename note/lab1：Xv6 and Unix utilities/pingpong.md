# pingpong(easy)

description：ping-pong a bte between two processses over a pair of pipes

parent send a byte to the child --> the child should print “<pid>:receving ping” ,and write the byte on the pipe to the parent

parent should read the byte from child ,and print "<pid>:received pong"



## hints

- use pipe to create a pipe

  a pipe  is a connection between two processes

  pipe is one-way communication only  that one proc wirte to the  pipe | one proc read to the pipe

  int pipe(int fds[2])  // fd[0] 管道的读  fd[1] 管道的写

- use fork to create a child

- use read and write to RW to the pipe

- 