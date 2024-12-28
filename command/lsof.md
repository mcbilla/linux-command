lsof
===

显示Linux中的文件打开情况

## 补充说明

`lsof`是一个用来展示Linux系统中被进程打开的文件列表的命令。这个工具非常有用，因为在Unix、Linux中，几乎所有事物都以文件的形式存在，包括硬件设备、套接字、管道等。`lsof`可以帮助系统管理员监控系统中的文件使用情况，排查文件和进程相关的问题。

## 适用的Linux版本

`lsof`在大多数Linux发行版中都是可用的。对于少数不自带此命令的发行版，可以通过包管理器进行安装。在CentOS 7和CentOS 8中，安装命令分别如下：

- CentOS 7：

```shell
$ sudo yum install lsof
```

- CentOS 8：

```shell
$ sudo dnf install lsof
```

* 对于其他发行版，例如Ubuntu或Debian

```shell
$ sudo apt-get install lsof
```

## 命令语法

```shell
lsof [options]
```

## 选项

| 选项 | 描述                               |
| ---- | ---------------------------------- |
| -i   | 显示网络连接相关的文件             |
| -u   | 列出指定用户打开的文件             |
| -p   | 列出指定进程号所打开的文件         |
| +d   | 列出指定目录下被打开的文件         |
| -t   | 仅显示进程ID                       |
| -n   | 不将IP转换成hostname               |
| -l   | 不将用户ID转换成用户名             |
| -c   | 列出使用特定命令名的进程打开的文件 |
| -r   | 重复列出文件，直到被中断           |
| -a   | 同时使用多个条件进行匹配           |

## 示例

### 列出所有打开的文件

```shell
lsof
command     PID USER   FD      type             DEVICE     SIZE       NODE NAME
init          1 root  cwd       DIR                8,2     4096          2 /
init          1 root  rtd       DIR                8,2     4096          2 /
init          1 root  txt       REG                8,2    43496    6121706 /sbin/init
init          1 root  mem       REG                8,2   143600    7823908 /lib64/ld-2.5.so
init          1 root  mem       REG                8,2  1722304    7823915 /lib64/libc-2.5.so
```

lsof输出各列信息的意义如下：

标识 | 说明
:- | :-
`COMMAND` | 进程的名称
`PID` | 进程标识符
`PPID` | 父进程标识符（需要指定-R参数）
`USER` | 进程所有者
`PGID` | 进程所属组
`FD` | 文件描述符，应用程序通过它识别该文件

FD文件描述符列表：

标识 | 说明
:- | :-
`cwd`  | 表示当前工作目录，即：应用程序的当前工作目录，这是该应用程序启动的目录，除非它本身对这个目录进行更改
`txt`  | 该类型的文件是程序代码，如应用程序二进制文件本身或共享库，如上列表中显示的 /sbin/init 程序
`lnn`  | 库引用 (AIX);
`er`   | FD 信息错误（参见名称栏）
`jld`  | jail 目录 (FreeBSD);
`ltx`  | 共享库文本（代码和数据）
`mxx`  | 十六进制内存映射类型编号xx
`m86`  | DOS合并映射文件
`mem`  | 内存映射文件
`mmap` | 内存映射设备
`pd`   | 父目录
`rtd`  | 根目录
`tr`   | 内核跟踪文件 (OpenBSD)
`v86`  | VP/ix 映射文件
`0`    | 表示标准输出
`1`    | 表示标准输入
`2`    | 表示标准错误

一般在标准输出、标准错误、标准输入后还跟着文件状态模式：

标识 | 说明
:- | :-
`u` | 表示该文件被打开并处于读取/写入模式
`r` | 表示该文件被打开并处于只读模式
`w` | 表示该文件被打开并处于写入模式
`空格` | 表示该文件的状态模式为 unknow，且没有锁定
`-` | 表示该文件的状态模式为 unknow，且被锁定

同时在文件状态模式后面，还跟着相关的锁：

标识 | 说明
:- | :-
`N`     | 对于未知类型的Solaris NFS锁
`r`     | 用于部分文件的读取锁定
`R`     | 对整个文件进行读取锁定
`w`     | 对文件的一部分进行写锁定(文件的部分写锁)
`W`     | 对整个文件进行写锁定(整个文件的写锁)
`u`     | 用于任何长度的读写锁
`U`     | 对于未知类型的锁
`x`     | 对于文件部分的SCO OpenServer Xenix锁
`X`     | 对于整个文件的SCO OpenServer Xenix锁
`space` | 如果没有锁


TYPE文件类型

标识 | 说明
:- | :-
`DIR` | 表示目录
`CHR` | 表示字符类型
`BLK` | 块设备类型
`UNIX` |  UNIX 域套接字
`FIFO` | 先进先出 (FIFO) 队列
`IPv4` | 网际协议 (IP) 套接字
`DEVICE` | 指定磁盘的名称
`SIZE` | 文件的大小
`NODE` | 索引节点（文件在磁盘上的标识）
`NAME` | 打开文件的确切名称
`REG` | 常规文件

### 列出特定用户打开的文件

```shell
$ lsof -u username
```

### 显示所有网络连接

```shell
$ lsof -i
```

### 列出使用某个端口的所有进程

```shell
$ lsof -i :80
```

### 列出某个进程打开的文件

```shell
$ lsof -p PID
```

### 列出某个目录下所有被打开的文件

```shell
$ lsof +d /path/to/directory
```

### 列出所有的IPv4网络文件

```shell
$ lsof -i 4
```

### 列出所有的IPv6网络文件

```shell
$ lsof -i 6
```

### 显示特定端口的网络信息

```shell
[linux$ lsof -i :22
```

### 列出所有unix套接字文件

```shell
$ lsof -U
```

### 列出没有与任何文件关联的进程

```shell
$ lsof -d '^mem'
```

### 监控文件系统活动

```shell
$ lsof -r 2
```

### 查找哪个进程使用了某个文件

```shell
$ lsof /path/to/file
```

### 列出某个用户的所有活动网络连接

```shell
$ lsof -a -u username -i
```

### 查找打开文件数量最多的进程

```shell
$ lsof | awk '{print $2}' | sort | uniq -c | sort -n | tail -1
```

## 常见技巧

### 实时监控特定文件的使用情况

```shell
$ watch -n 1 'lsof | grep [filename]'
```

### 结合`grep`命令过滤输出

```shell
$ lsof | grep 'pattern'
```
