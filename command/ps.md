ps
===

报告当前系统的进程状态

## 补充说明

ps 命令是 process status 的缩写，用于显示当前系统的进程状态。进程是程序在运行时的实例，每个进程都有一个唯一的进程标识符（PID），以及一些相关的属性，如用户、优先级、内存占用、CPU使用率等。ps命令可以查看这些信息，帮助用户了解系统的运行情况，以及进行进程管理和优化。

## 适用的Linux版本

ps命令是一个标准的Linux命令，几乎所有的Linux发行版都支持它。不同的发行版可能会提供不同的ps版本，例如procps-ng或BSD ps。这些版本之间可能会有一些语法和选项的差异，但是基本功能和用法都是相同的。用户可以通过`ps --version`命令查看自己系统上的ps版本。

## 基本语法

```bash
ps [options]
```

options 参数支持三种语法格式：

- **UNIX 风格**：options 前必须有 `-` 连字符。
- **BSD 风格**：options前不能有 `-` 连字符。推荐使用。
- **GNU 风格**：options前有两个 `-` 连字符，也就是长参数格式。

```bash
# UNIX风格
ps -ef

# BSD风格
ps aux

# GNU风格
ps --pid 1234
```

如果不指定任何参数，ps命令默认只显示当前终端下属于当前用户的进程。

### UNIX风格

Unix风格的参数是从贝尔实验室开发的AT&T Unix系统上原有的ps命令继承下来的。

- -A	显示所有进程
- -N	显示与指定参数不符的所有进程
- -a	显示除控制进程 session leader")和无终端进程外的所有进程
- -d	显示除控制进程外的所有进程
- -e	显示所有进程
- -C cmdlist	显示包含在cmdlist列表中的进程
- -G grplist	显示组ID在grplist列表中的进程
- -U userlist	显示属主的用户ID在userlist列表中的进程
- -g grplist	显示会话或组ID在grplist列表中的进程"
- -p pidlist	显示PID在pidlist列表中的进程
- -s sesslist	显示会话ID在sesslist列表中的进程
- -t ttylist	显示终端ID在ttylist列表中的进程
- -u userlist	显示有效用户ID在userlist列表中的进程
- -F	显示更多额外输出(相对-f参数而言)
- -o format	显示默认的输出列以及format列表指定的特定列
- -M	显示进程的安全信息
- -C	显示进程的额外调度器信息
- -f	显示完整格式的输出
- -j	显示任务信息
- -l	显示长列表
- -o format	仅显示由format指定的列
- -Y	不要显示进程标记 (process flag,表明进程状态的标记)
- -Z	显示安全标签 (security context)"信息
- -H	用层级格式来显示进程(树状,用来显示父进程)
- -n namelist	定义了WCHAN列显示的值
- -W	采用宽输出模式,不限宽度显示
- -L	显示进程中的线程
- -V	显示ps命令的版本号

### BSD风格

Unix 和 BSD 类型的参数有很多重叠的地方。使用其中某种类型参数得到的信息也同样可以使用另一种获得。大多数情况下，你只要选择自己所喜欢格式的参数类型就行了

- T	显示跟当前终端关联的所有进程
- a	显示跟任意终端关联的所有进程
- g	显示所有的进程,包括控制进程
- r	仅显示运行中的进程
- x	显示所有的进程，甚至包括未分配任何终端的进程
- U userlist	显示归userlist列表中某用户ID所有的进程
- p pidlist	显示PID在pidlist列表中的进程
- t ttylist	显示所关联的终端在ttylist列表中的进程
- o format	除了默认输出的列之外,还输出由format指定的列
- X	按过去的Linux i386寄存器格式显示
- Z	将安全信息添加到输出中
- j	显示任务信息
- l	采用长模式
- o format	仅显示由format指定的列
- S	采用信号格式显示
- u	采用基于用户的格式显示
- V	采用虚拟内存格式显示
- N namelist	定义在WCHAN列中使用的值
- O order	定义显示信息列的顺序
- S	将数值信息从子进程加到父进程上，比如CPU和内存的使用情况
- C	显示真实的命令名称(用以启动进程的程序名称)
- e	显示命令使用的环境变量
- f	用分层格式来显示进程，明哪些进程启动了哪些进程
- h	不显示头信息
- k sort	指定用以将输出排序的列
- n	和WCHAN信息一起显示出来，用数值来表示用户ID和组ID
- W	为较宽屏幕显示宽输出
- H	将线程按进程来显示
- m	在进程后显示线程
- L	列出所有格式指定符
- V	显示ps命令的版本号

### GNU风格

GNU开发人员在这个新改进过的 ps 命令中加入了另外一些参数。其中一些GNU长参数复制了现有的Unix或BSD类型的参数，而另一些则提供新功能。

- --deselect	显示所有进程，命令行中列出的进程
- —Group grplist	显示组ID在grplist列表中的进程
- —User userlist	显示用户ID在userlist列表中的进程
- —group grplist	显示有效组ID在grplist列表中的进程
- —pid pidlist	显示PID在pidlist列表中的进程
- —ppid pidlist	显示父PID在pidlist列表中的进程
- —sid sidlist	显示会话ID在sidlist列表中的进程
- —tty ttylist	显示终端设备号在ttylist列表中的进程
- —user userlist	显示有效用户ID在userlist列表中的进程
- —format format	仅显示由format指定的列
- —context	显示额外的安全信息
- —cols n	将屏幕宽度设置为n列
- —columns n	将屏幕宽度设置为n列
- --cumulative	包含已停止的子进程的信息
- —forest	用层级结构显示出进程和父进程之间的关系
- —headers	在每页输出中都显示列的头
- —no-headers	不显示列的头
- —lines n	将屏幕高度设为n行
- —rows n	将屏幕高度设为n排
- —sort order	指定将输出按哪列排序
- —width n	将屏幕宽度设为n列
- —help	显示帮助信息
- —info	显示调试信息
- —version	显示ps命令的版本号

## 示例

### 使用UNIX风格打印出所有的进程

```bash
$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 10:22 ?        00:00:02 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root         2     0  0 10:22 ?        00:00:00 [kthreadd]
root         4     2  0 10:22 ?        00:00:00 [kworker/0:0H]
```

显示的完整格式输出信息有：

- UID: 启动这些进程的用户。
- PID: 进程的进程 ID。
- PPID: 父进程的进程号 (如果该进程是由另一个进程启动的)
- C: 进程生命周期中的 CPU 利用率
- STIME: 进程启动时的系统时间
- TTY: 进程启动时的终端设备
- TIME: 运行进程需要的累计 CPU 时间
- CMD: 启动的程序名称

使用 `-l` 的输出的信息还会增加一些：

- F : 内核分配给进程的系统标记
- S : 进程的状态，linux下有五种状态
  - O 代表正在运行
  - S 代表在休眠
  - R 代表可运行，正等待运行
  - Z 代表僵化, 进程已结束但父进程已不存在
  - T 代表停止
- PRI : 进程的优先级 (越大的数字代表越低的优先级)
- NI : 谦让度值用来参与决定优先级
- ADDR : 进程的内存地址
- SZ : 假如进程被换出, 所需交换空间的大致大小
- WCHAN : 进程休眠的内核函数的地址

### 使用BSD风格打印出所有的进程

```bash
$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.2 128036  3796 ?        Ss   10:22   0:02 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root         2  0.0  0.0      0     0 ?        S    10:22   0:00 [kthreadd]
```

在 BSD 风格下的 l 参数输出的信息基本与 Unix 一致，多了一些字段：

- VSZ：进程在内存中的大小，以千字节 (KB) 为单位。
- RSS：进程在未换出时占用的物理内存。
- STAT：代表当前进程状态的双字符状态码。STAT 列用的比较多，能输出更详细的进程状态码。双字符状态码能比 Unix 风格输出的单字符状态码更清楚地表示进程的当前状态。第一个字符采用了和 Unix 风格的 `-l` 参数的 `s` 列相同的值，表明进程是在休眠、运行还是等待；第二个字符进一步说明进程的状态：
  - < : 该进程运行在高优先级上
  - N : 该进程运行在低优先级上
  - L : 该进程有页面锁定在内存中
  - s : 该进程是控制进程
  - l : 该进程是多线程的
  - \+ : 该进程运行在前台

### 按照指定字段排序输出结果

例如按照CPU使用率降序排序

```bash
# 使用--sort选项，后跟排序规则，以`-`开头表示降序，以`+`开头表示升序，后跟字段名
$ ps --sort -pcpu -ef
```

### 以树状结构显示进程间关系

可以看出哪些进程是父进程，哪些是子进程

```bash
# 使用--forest选项，可以与其他选项组合使用
$ ps --forest -ef
```

### 自定义格式显示信息

例如只显示进程的PID、用户和命令

```bash
# 使用-o选项，后跟字段列表，字段名不区分大小写
$ ps -o pid,user,cmd -ef
```

### 查看线程（LWP）

linux没有线程的概念，通过轻量级进程（LWP）来实现线程的功能，用户进程作为一个管理进程来管理下面的LWP。

```bash
# 查看指定进程对应的LWP
$ ps -Lf pid
```

