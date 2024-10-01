sar
===

系统运行状态统计工具

## 补充说明

**sar** 是 Linux 的系统运行状态统计工具，它将指定的操作系统状态计数器显示到标准输出设备。sar 工具将对系统当前的状态进行取样，然后通过计算数据和比例来表达系统的当前运行状态。它的特点是可以连续对系统取样，获得大量的取样数据。取样数据和分析的结果都可以存入文件，使用它时消耗的系统资源很小。

## 适用的 Linux 版本

sar 命令在大多数 Linux 发行版中都可以使用，但需要先安装 sysstat 包。不同的发行版可能有不同的安装命令，您可以参考下面的示例：

```shell
# 基于 apt 的发行版（如 Debian、Ubuntu、Raspbian、Kali Linux 等）：
$ sudo apt install sysstat

# 基于 yum 的发行版（如 RedHat，CentOS 7 等）：
$ sudo yum install sysstat

# 基于 dnf 的发行版（如 Fedora，CentOS 8 等）：
$ sudo dnf install sysstat

# 基于 apk 的发行版（如 Alpine Linux）：
$ sudo apk add --update sysstat

# 基于 pacman 的发行版（如 Arch Linux）：
$ sudo pacman -S sysstat

# 基于 zypper 的发行版（如 openSUSE）：
$ sudo zypper in sysstat

# 基于 pkg 的 FreeBSD 发行版
$ sudo pkg install sysstat

# 基于 pkg 的 OS X/macOS 发行版：
$ brew install sysstat

```

##  命令语法

```shell
sar [options] [interval] [count]
```

*   options：指定要显示的统计数据的选项。
*   interval：每次报告的间隔时间（秒）。
*   count：显示报告的次数。

如果省略 interval 和 count，sar 会显示系统启动以来的平均统计数据。

如果只省略 count，sar 会无限次地显示统计数据，直到按 Ctrl+C 中断。

##  选项

```shell
-A: 显示所有的报告信息；
-b: 显示I/O速率；
-B: 显示换页状态；
-c: 显示进程创建活动；
-d: 显示每个块设备的状态；
-e: 设置显示报告的结束时间；
-f: 从指定文件提取报告；
-i: 设状态信息刷新的间隔时间；
-n: 报告网络统计信息。
-P: 报告每个CPU的状态；
-R: 显示内存状态；
-u: 显示CPU利用率；
-v: 显示索引节点，文件和其他内核表的状态；
-w: 显示交换分区状态；
-x: 显示给定进程的状态。
-r: 以分页方式显示输出，每页最多显示 100 行。  
-o: 输出选项，指定要显示的列。例如，`-o mrk,prt,cvg` 将显示 CPU 使用率、进程标识符、磁盘使用率 和 网络流量。  
-t: 时间戳选项，指定要在输出中添加时间戳。  
-s: 统计选项，指定要显示的统计数据的类型。例如，`-s us,ms` 将显示 CPU 使用率的 us 和 ms 时间段的平均值。  
-c: 选项用于指定要发送的命令。例如，`-c ls` 将显示当前目录中的文件和子目录列表。
```

##  示例

### 实例1：显示 CPU 利用率的统计数据

如果想查看 CPU 的利用率，可以使用 `sar -u` 命令，它会显示每个 CPU 的用户态、系统态、空闲态和等待 I/O 的百分比。如果不指定 interval 和 count，sar 会显示系统启动以来的平均 CPU 利用率。例如：

```shell
$ sar -u
Linux 5.10.15-1-MANJARO (linux) 	2024-01-26 	_x86_64_	(4 CPU)

11:52:13        CPU     %user     %nice   %system   %iowait    %steal     %idle
11:52:13        all      3.02      0.00      1.08      0.09      0.00     95.81
Average:        all      3.02      0.00      1.08      0.09      0.00     95.81
```

如果指定 interval 和 count，sar 会显示每隔 interval 秒的 CPU 利用率，共显示 count 次。例如，sar -u 5 3 会显示每隔 5 秒的 CPU 利用率，共显示 3 次。例如：

```shell
$ sar -u 5 3
Linux 5.10.15-1-MANJARO (linux) 	2024-01-26 	_x86_64_	(4 CPU)

11:52:13        CPU     %user     %nice   %system   %iowait    %steal     %idle
11:52:18        all      2.90      0.00      1.10      0.10      0.00     95.90
11:52:23        all      3.10      0.00      1.05      0.08      0.00     95.77
Average:        all      3.04      0.00      1.07      0.09      0.00     95.79
```

如果想查看每个 CPU 的利用率，可以使用 `sar -u -P ALL` 命令，它会显示每个 CPU 的用户态、系统态、空闲态和等待 I/O 的百分比，以及所有 CPU 的平均值。例如：

```shell
$ sar -u -P ALL
Linux 5.10.15-1-MANJARO (linux) 	2024-01-26 	_x86_64_	(4 CPU)

11:52:13        CPU     %user     %nice   %system   %iowait    %steal     %idle
11:52:13          0      3.03      0.00      1.09      0.09      0.00     95.79
```

### 实例2：显示内存利用率的统计数据

如果想要查看内存的利用率，可以使用 `sar -r` 命令，它会显示内存使用率和空闲内存量。例如：

```shell
$ sar -r
```

### 实例3：显示磁盘I/O的统计数据

如果你想要查看磁盘I/O的使用情况，可以使用 `sar -d` 命令，它会显示设备的活动状况。例如：

```shell
$ sar -d
```

### 实例4：显示网络统计数据

如果你想查看网络使用情况，可以使用 `sar -n DEV` 命令，其中 'DEV' 指的是设备名，它会显示接收和发送的字节数。例如：

```shell
$ sar -n DEV
```

### 实例5：显示所有CPU的负载平均值

案例中将显示所有 CPU 的负载平均值，此操作可通过 "sar -q" 实现。例如：

```shell
$ sar -q
```

### 实例6：查看可用交换空间大小

通过 `sar -S` 命令，您可以查看系统可用的交换空间大小。例如：

```shell
$ sar -S
```

### 实例7：查看运行队列和加载平均值

通过 `sar -q` 命令，您可以查看系统的运行队列和加载平均值。例如：

```shell
$ sar -q
```

### 实例8：查看设备的数据传输速率

通过 `sar -b` 命令，您可以查看设备的数据传输速率。例如：

```shell
$ sar -b
```

### 实例9：显示特定时间段的CPU利用率

通过指定开始时间和结束时间，可以查看特定时间段的 CPU 利用率，如8:00至12:00。前十一点四十五开始，到十二点结束的数据，可以使用 `sar -u -s 11:45:00 -e 12:00:00`。例如：

```shell
$ sar -u -s 08:00:00 -e 12:00:00
```

### 实例10：每10分钟收集一次系统数据

可以设置 sar 命令每10分钟收集一次系统数据，持续2个小时，可以使用 `sar -o output.file 600 12`。例如：

```shell
$ sar -o output.file 600 12
```

### 实例11：从已保存的 sar 文件中获取信息

将由 `sar -o output.file 600 12` 产生的数据读取出来，可以使用 `sar -f output.file`：

```shell
$ sar -f output.file
```

### 实例12：查看所有网络设备的统计数据

查看所有网络适配器的统计数据，包括所接收和发送的数据包数量，可以使用 `sar -n DEV`。例如：

```shell
$ sar -n DEV
```

## Linux sar 命令的注意事项

1. 在使用 sar 命令前，请确保已经正确安装了 sysstat 包，否则可能会出现 "bash: sar: command not found" 的错误提示。如果遇到这种情况，请按照步骤安装 sysstat。
2. 记住，在运行带有参数的 sar 命令时，参数的顺序是很重要的，弄错了可能会导致命令无法正确执行。
3. sar 命令提供的信息可能会根据你的系统类型和配置有所不同，不是所有的选项都适用于每一种系统。
4. sar 命令是一个占用资源较小的命令，可以在生产环境中放心使用。但是，当你需要进行频繁或密集的系统性能监控时，应注意系统资源的使用情况。