pstack
===

分析进程调用栈

## 补充说明

`pstack`（process stack）命令用于显示一个或多个进程的线程的栈跟踪信息。通过这个命令，我们可以得知进程在执行时调用了哪些函数，以及它们的调用顺序。这对于诊断程序中的死锁或者性能瓶颈非常有帮助。

## 适用的Linux版本

`pstack` 命令通常包含在`gdb`或其他调试工具包中，并不是所有Linux发行版都预装了`pstack`。如果在系统中未找到该命令，您可能需要安装它。

对于基于Debian的系统（如Ubuntu），使用以下命令安装：

```bash
$ sudo apt-get install gdb
```

对于Red Hat系列，包括CentOS 7和CentOS 8，安装方式有所不同：

- CentOS 7:

```bash
$ sudo yum install gdb
```

- CentOS 8:

```bash
$ sudo dnf install gdb
```

## 基本语法

基本语法格式为：

```bash
pstack PID
```

其中 `PID` 是要检查的进程的进程ID。由于 `pstack` 是一个相对简单的命令，它并不包含大量的选项

## 示例

### 实例1：显示特定进程的调用栈

如果我们想查看进程ID为1234的进程的调用栈，我们可以使用：

```bash
$ pstack 1234
```

### 实例2：将pstack输出重定向到文件

有时候，调用栈的输出可能非常长，不便于在终端中查看，我们可以将输出重定向到文件中：

```bash
$ pstack 1234 > stack_trace.txt
```

### 实例3：使用pstack和grep组合搜索特定函数

如果你想要查找调用栈中是否存在特定的函数名称，可以将`pstack`的输出通过管道传递给`grep`命令：

```bash
$ pstack 1234 | grep -i function_name
```

这里的`function_name`应该替换为你想搜索的函数名称。这个命令会返回所有包含该函数名称的调用栈行。

### 实例4：批量查看多个进程的调用栈

如果你有一系列进程的PID，并希望快速查看他们的调用栈，你可以使用循环：

```bash
$ for pid in 1234 2345 3456; do pstack $pid; done
```

这个循环会依次对每个PID执行`pstack`命令。

### 实例5：结合ps和pstack查看特定程序的调用栈

如果你不知道进程的PID，但是知道程序的名称，可以先用`ps`命令配合`grep`找到进程，然后用`xargs`执行`pstack`：

```bash
$ ps -ef | grep -i program_name | grep -v grep | awk '{print $2}' | xargs -I {} pstack {}
```

这里的`program_name`是你要查找的程序名称。

### 实例6：监视进程调用栈的变化

对于一个长时间运行的进程，了解其调用栈如何随时间变化可能很有用。可以使用`watch`命令结合`pstack`来完成这个任务：

```bash
$ watch -n 5 pstack 1234
```

这个命令会每5秒运行一次`pstack`，不断地显示进程的调用栈信息。

### 实例7：将pstack输出用于进一步分析

如果你需要对调用栈输出进行进一步的分析或处理，你可以将其输出到一个文件，然后使用其他工具进行处理：

```bash
$ pstack 1234 > pstack_output.txt
```

然后，你可以使用文本处理工具如`awk`, `sed`, 或者`python`脚本来分析`pstack_output.txt`文件。

### 实例8：结合awk提取特定的调用栈信息

在某些情况下，你可能只对调用栈中的特定部分感兴趣。你可以使用`awk`来提取这些信息：

```bash
$ pstack 1234 | awk '/Thread/,/End of stack trace/'
```

这个命令会打印出每个线程的开始到"End of stack trace"之间的行。

### 实例9：结合`pstack`和`awk`来汇总调用栈次数

如果你想要知道哪些函数在调用栈中出现的次数最多，可以使用`awk`来计数：

```bash
$ pstack $(pgrep program_name) | awk -F'(' '{print $2}' | sort | uniq -c | sort -nr
```

这个命令首先通过`pgrep`获取`program_name`的进程ID，然后提取调用栈中的函数名称，对它们进行排序和去重，最后按出现次数从多到少排序。

### 实例10：使用`pstack`和`tail`查看最新的调用栈条目

如果你只对最新的几个调用栈条目感兴趣，可以结合使用`pstack`和`tail`：

```bash
$ pstack 1234 | tail -n 10
```

这会显示进程1234的最后10行调用栈信息。

### 实例11：监控多个进程并输出到各自的文件

如果你需要监控多个进程的调用栈，并且希望将每个进程的输出保存到单独的文件中，可以使用以下命令：

```bash
$ for pid in $(pgrep program_name); do pstack $pid > "stack_trace_$pid.txt" & done
```

这将为`pgrep`找到的每个`program_name`进程创建一个`pstack`调用，并将输出重定向到一个包含PID的文件中。

### 实例12：过滤出调用栈中的特定库函数

如果你在调试中只关心特定的库函数，可以使用`grep`来过滤出包含这些库函数的调用栈行：

```bash
$ pstack 1234 | grep '/lib/'
```

这将只显示调用栈中包含`/lib/`路径的行，通常这些是库函数的调用。

### 实例13：结合`pstack`和`sed`来格式化输出

有时候，你可能需要对`pstack`的输出进行一些格式化，以便更容易地阅读或处理。你可以使用`sed`来实现：

```bash
$ pstack 1234 | sed 's/#[0-9]*  //g'
```

这个命令会从`pstack`输出中删除所有的堆栈索引号（如`#1`，`#2`等）。

### 实例14：结合`pstack`和`cut`来简化输出

如果调用栈信息太详细，你只需要函数名和地址，可以使用`cut`来简化输出：

```bash
$ pstack 1234 | cut -d ' ' -f 2-
```

这个命令会删除每行的第一个字段（通常是线程ID）。

### 实例15：使用`pstack`分析进程在特定时间的状态

对于长时间运行且状态变化的进程，可以在特定时间点捕获它的状态：

```bash
$ date && pstack 1234
```

这个命令将显示当前时间，然后打印出进程1234的调用栈，有助于你将状态与时间点对应起来。

## 使用pstack排查进程耗时过多的问题

这个命令在排查进程问题时非常有用，比如我们发现一个服务一直处于work状态（如假死状态，好似死循环），使用这个命令就能轻松定位问题所在；可以在一段时间内，多执行几次pstack，若发现代码栈总是停在同一个位置，那个位置就需要重点关注，很可能就是出问题的地方；

**示例1：查看bash程序进程栈**

```
/opt/app/tdev1$ps -fe| grep bash
tdev1   7013  7012  0 19:42 pts/1    00:00:00 -bash
tdev1  11402 11401  0 20:31 pts/2    00:00:00 -bash
tdev1  11474 11402  0 20:32 pts/2    00:00:00 grep bash
/opt/app/tdev1$pstack 7013
#0  0x00000039958c5620 in __read_nocancel () from /lib64/libc.so.6
#1  0x000000000047dafe in rl_getc ()
#2  0x000000000047def6 in rl_read_key ()
#3  0x000000000046d0f5 in readline_internal_char ()
#4  0x000000000046d4e5 in readline ()
#5  0x00000000004213cf in ?? ()
#6  0x000000000041d685 in ?? ()
#7  0x000000000041e89e in ?? ()
#8  0x00000000004218dc in yyparse ()
#9  0x000000000041b507 in parse_command ()
#10 0x000000000041b5c6 in read_command ()
#11 0x000000000041b74e in reader_loop ()
#12 0x000000000041b2aa in main ()
```

或者类似这样执行

```
$ sudo pstack $(pgrep -uroot php-fpm)
[sudo] password for guanyy:
#0  0x000000380d8e86f3 in __epoll_wait_nocancel () from /lib64/libc.so.6
#1  0x00000000007ec4a4 in fpm_event_epoll_wait ()
#2  0x00000000007e1517 in fpm_event_loop ()
#3  0x00000000007dc887 in fpm_run ()
#4  0x00000000007e3bd8 in main ()
```

**实例2：ps -eLo pid,lwp,pcpu |grep pid**

利用一个最简单的命令就能够定位到哪段程序可能存在性能问题：获取进程ID，通过pstack命令查看里边的各个线程id以及对应的线程现在正在做什么事情，分析多组数据就可以获得哪些线程里有慢操作影响了服务器的性能，从而得到解决方案

![](pstack/430613-20171018111412396-604927870.png)

由此可以判断出来在LWP 30222这个线程产生了性能问题，执行时间长达31.4毫秒的时间，再观察无非就是下面的几个语句出现的问题，只需要简单排查就知道了问题瓶颈。

![](pstack/430613-20171018111533724-1346211898.png)

> "NativeThread ID"所指的本地线程是指该java虚拟机所对应的虚拟机中的本地线程，java代码是依附于java虚拟机的本地线程执行的，当启动一个线程时，是创建一个native本地线程，本地线程才是真实的线程实体，为了更加深入理解本地线程和java线程的关系，可以通过以下方式将java虚拟机的本地线程打印出来：
>
> 1、使用 `ps -ef|grep java` 获得 java 进程id
>
> 2、使用 `pstack<java pid>` 获得 java 虚拟机本地线程的堆栈
>
> 从操作系统打印出来的虚拟机的本地线程看，本地线程数量和java线程数量是相同的，说明二者是一一对应的关系。

## Linux pstack命令的注意事项

- 要使用 `pstack`，您可能需要具有 root 权限或对进程有足够的权限。
- `pstack` 命令在某些系统上可能不可用，如果遇到 bash: pstack: command not found，需按照上面的方法进行安装。
