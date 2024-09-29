free
===

显示物理内存和交换空间的使用情况

## 补充说明

**free命令** 是一个用来显示系统物理内存和交换空间的使用情况的命令。它可以提供关于总量、已用、空闲、共享、缓冲和缓存的内存的信息。它还可以显示可用的内存，这是一个估计值，表示可以用来满足应用程序内存请求的内存量。free命令的输出可以帮助你了解系统的内存管理机制，以及如何优化内存的使用。 其实 free 命令中的信息都来自于 /proc/meminfo 文件。 /proc/meminfo 文件包含了更多更原始的信息，只是显示不太直观。

## Linux free命令适用的Linux版本

free命令是一个通用的Linux命令，它可以在大多数的Linux发行版中使用，包括Ubuntu, Debian, Fedora, CentOS, Red Hat, SUSE, Arch等。 如果你的系统没有安装free命令，你可以使用以下命令来安装它：

- 对于基于Debian的系统，如Ubuntu，你可以使用apt命令：

```bash
$ sudo apt update
$ sudo apt install procps
```

- 对于基于Red Hat的系统，如CentOS，你可以使用yum或dnf命令：

```bash
$ sudo yum install procps-ng
```

或者

```bash
$ sudo dnf install procps-ng
```

free 命令是 procps 或 procps-ng 软件包的一部分，这些软件包提供了一些用于监控系统资源和进程的命令，如 ps, top, vmstat 等。

##  命令语法 

```shell
free [options]
```

options 说明：

- -b, --bytes: 以字节为单位显示内存。
- -k, --kilo: 以千字节为单位显示内存（默认）。
- -m, --mega: 以兆字节为单位显示内存。
- -g, --giga: 以吉字节为单位显示内存。
- —tera: 以太字节为单位显示内存。
- -h, --human: 自动调整显示单位为最短的三位数，并显示单位。使用的单位有B（字节），K（千字节），M（兆字节），G（吉字节），T（太字节）。
- -c, --count: 循环显示c次输出，与-s选项一起使用。
- -l, --lohi: 显示详细的低内存和高内存统计信息。
- -o, --old: 禁用显示缓冲调整后的行。
- -s, --seconds: 每隔s秒连续显示输出，s可以是小数。
- -t, --total: 在输出中添加一行显示列的总计。
- --help: 显示帮助信息并退出。
- -V, --version: 显示版本信息并退出。

##  实例 

### 不带任何选项的free命令，以千字节为单位显示内存使用情况

```shell
$ free
              total        used        free      shared  buff/cache   available
Mem:        8048372     2593004     1366712      658380     4088656     4494976
Swap:             0           0           0
```

**第一行Mem：** 

* total 系统总的可用物理内存大小
* used 已被使用的物理内存大小
* free 还有多少物理内存可用，是真正尚未被使用的物理内存数量。
* shared 被共享使用的物理内存大小
* buff/cache 被 buffer 和 cache 使用的物理内存大小
* available 还可以被应用程序使用的物理内存大小。这是从应用程序的角度看到的可用内存，一般来说 `available = free + buffer + cache`。

**第二行Swap**：交换分区，也就是我们通常所说的虚拟内存。当系统物理内存吃紧时，Linux 会将内存中不常访问的数据保存到 Swap 区上。内核提供了一个叫做 `swappiness` 的参数，用于配置需要将内存中不常用的数据移到 Swap 中的紧迫程度。数值越小表示尽量不要使用 Swap 。

> buffers 和 cached 都是缓存，两者有什么区别呢？
>
> 磁盘的操作有逻辑级（文件系统）和物理级（磁盘块）两种方式。为了提高磁盘存取效率，Linux 采取了两种Cache 方式： Page Cache 和 Buffer Cache，分别针对逻辑和物理数据进行缓存。
>
> * Page Cache 是针对文件系统 inode 的缓存，在文件层面上的数据会缓存到 page cache。文件的逻辑层需要映射到实际的物理磁盘，这种映射关系由文件系统来完成。当 page cache 的数据需要刷新时，page cache 中的数据交给 buffer cache，因为Buffer Cache就是缓存磁盘块的。但是这种处理在2.6版本的内核之后就变的很简单了，没有真正意义上的 cache 操作。
> * Buffer Cache 是针对磁盘块的缓存，也就是在没有文件系统的情况下，直接对磁盘进行操作的数据会缓存到 buffer cache 中，例如，文件系统的元数据都会缓存到 buffer cache 中。
>
> 所以 Linux 系统只要不用 Swap 的交换空间，就不用担心自己的内存太少。一般情况下，少量地使用 Swap 也是不影响系统性能的。如果常常 Swap 用很多，可能你就要考虑加物理内存了，这也是 Linux 看内存是否够用的标准。
>

### 使用-h选项的free命令，以人类可读的格式显示内存使用情况

```bash
$ free -h
              total        used        free      shared  buff/cache   available
Mem:          7.7Gi       2.5Gi       1.3Gi       643Mi       3.9Gi       4.3Gi
Swap:            0B          0B          0B
```

### 使用-t选项的free命令，显示内存和交换空间的总计

```bash
$ free -t
              total        used        free      shared  buff/cache   available
Mem:        8048372     2593004     1366712      658380     4088656     4494976
Swap:             0           0           0
Total:      8048372     2593004     1366712
```

### 使用-s选项的free命令，每隔2秒显示一次内存使用情况，直到按下Ctrl+C停止

```bash
$ free -s 2
              total        used        free      shared  buff/cache   available
Mem:        8048372     2593004     1366712      658380     4088656     4494976
Swap:             0           0           0
              total        used        free      shared  buff/cache   available
Mem:        8048372     2593004     1366712      658380     4088656     4494976
Swap:             0           0           0
              total        used        free      shared  buff/cache   available
Mem:        8048372     2593004     1366712      658380     4088656     4494976
Swap:             0           0           0
^C
```

### 使用-l选项的free命令，显示低内存和高内存的统计信息

```
[linux@bashcommandnotfound.cn ~]$ free -l
              total        used        free      shared  buff/cache   available
Mem:        8048372     2593004     1366712      658380     4088656     4494976
Low:        8048372     2593004     1366712
High:             0           0           0
Swap:             0           0           0
```

## Linux free命令的注意事项

- free命令的输出可能会随着系统的运行而变化，因为内核会根据需要动态地分配和回收内存。
- free命令的输出中，空闲的内存（free）并不代表系统可以使用的内存，因为缓冲和缓存的内存也可以被回收。因此，可用的内存（available）是一个更准确的指标，它表示系统可以分配给应用程序的内存量，而不会导致交换。
- free命令的输出中，共享的内存（shared）是指由tmpfs文件系统使用的内存，它是一种基于内存的文件系统，用于存储临时文件，如/run, /dev/shm等。
- free命令的输出中，缓冲和缓存的内存（buff/cache）是指由内核用于提高系统性能的内存。缓冲（buffers）是指用于存储磁盘块的内存，缓存（cache）是指用于存储文件系统元数据和文件内容的内存。
- 如果你想要释放缓冲和缓存的内存，你可以使用以下命令，但是这并不会提高系统的性能，反而可能会降低，因为内核会重新分配缓冲和缓存的内存：

```bash
$ sudo sync
$ sudo echo 3 > /proc/sys/vm/drop_caches
```

- 如果你想要查看内存的详细信息，你可以使用以下命令，它们会显示更多的内存相关的参数和统计数据：

```bash
$ cat /proc/meminfo
$ vmstat -s
```

