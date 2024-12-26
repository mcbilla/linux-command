vmstat
===

监测系统的虚拟内存、进程、CPU活动、块设备I/O等信息

## 补充说明

**vmstat命令** 的含义为显示虚拟内存状态（Viryual Memor Statics），但是它可以报告关于进程、内存、I/O等系统整体运行状态。通过该命令，我们可以获得系统的即时状态，包括内存使用情况、进程状态以及CPU的使用情况等。

## 适用的Linux版本

vmstat命令几乎在所有的Linux发行版中都是可用的。如果你发现系统中没有这个命令，可以通过包管理器安装它：

```shell
# 基于apt的发行版（如Debian、Ubuntu、Raspbian、Kali Linux等）
sudo apt-get update && sudo apt-get install procps

# 基于yum的发行版（如RedHat，CentOS 7等）
sudo yum update && sudo yum install procps-ng

# 基于dnf的发行版（如Fedora，CentOS 8等）
sudo dnf update && sudo dnf install procps-ng

# 基于apk的发行版（如Alpine Linux）
sudo apk add --update procps

# 基于pacman的发行版（如Arch Linux）
sudo pacman -Syu && sudo pacman -S procps-ng

# 基于zypper的发行版（如openSUSE）
sudo zypper ref && sudo zypper in procps

# 基于pkg的FreeBSD发行版
sudo pkg update && sudo pkg install procps

# 基于pkg的OS X/macOS发行版
brew update && brew install procps
```

##  命令语法

```shell
vmstat [options] [delay [count]]
```

*   delay：状态信息刷新的时间间隔；
*   count：刷新次数。如果不指定刷新次数，但指定了刷新时间间隔，这时刷新次数为无穷。

##  选项

```
-a	显示活跃和非活跃内存
-f	显示自系统启动以来发生的系统叉数量
-m	显示slabinfo
-n	显示每列一次，与延迟时间配合使用
-s	显示内存相关统计
-d	显示磁盘相关统计
-p	显示指定磁盘分区的统计
-S	设置显示单位（K、M、G），默认为K（1024 bytes）
-r	显示内存页面的统计
-t	显示时间戳信息
```

##  示例

### 显示基本的系统状态信息

```shell
$ vmstat
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 3029876 199616 690980    0    0     0     2    3    2  0  0 100  0  0

```

**字段说明：** 

**Procs（进程）**

*   **r**：运行队列中进程数量，这个值也可以判断是否需要增加 CPU。如果这个值长期超过 CPU 个数，说明出现了 CPU 瓶颈。
*   **b**：阻塞的进程数量。

**Memory（内存）**

*   **swpd**：使用虚拟内存大小，如果 swpd 的值不为0，但是 si、so 的值长期为0，这种情况不会影响系统性能。
*   **free**：空闲物理内存大小。
*   **buff**：用作缓冲的内存大小。
*   **cache**：用作缓存的内存大小，如果 cache 的值大的时候，说明 cache 处的文件数多，如果频繁访问到的文件都能被 cache 住，那么磁盘的读 IO（bi）会非常小。

**Swap**

*   **si**：每秒从交换区写到内存的大小，由磁盘调入内存。
*   **so**：每秒写入交换区的内存大小，由内存调入磁盘。

> 当内存够用的时候，这2个值都是0，如果这2个值长期大于0时，系统性能会受到影响，磁盘 IO 和 CPU 资源都会被消耗。有些朋友看到空闲内存（free）很少的或接近于0时，就认为内存不够用了，不能光看这一点，还要结合 si 和 so，如果 free 很少，但是 si 和 so 也很少（大多时候是0），那么不用担心，系统性能这时不会受到影响的。
>

**IO（现在的Linux版本块的大小为1kb）**

*   **bi**：每秒读取的块数
*   **bo**：每秒写入的块数

> 随机磁盘读写的时候，这2个值越大（如超出1024k)，能看到 CPU 在 IO 等待的值也会越大。
>

**System（系统）**

*   **in**：每秒中断数，包括时钟中断。
*   **cs**：每秒上下文切换数。这个值越大，说明 CPU 大部分时间都花在上下文切换上。

> 上面 2 个值越大，会看到由内核消耗的 CPU 时间会越大。
>

**CPU（以百分比表示）**

*   **us**：用户进程执行时间百分比(user time)。us 的值比较高时，说明用户进程消耗的 CPU 时间多，但是如果长期超 50% 的使用，那么我们就该考虑优化程序算法或者进行加速。

*   **sy**：内核系统进程执行时间百分比(system time)。sy 的值高时，说明系统内核消耗的 CPU 资源多，这并不是良性表现，我们应该检查原因。

*   **wa**：IO 等待时间百分比。wa 的值高时，说明 IO 等待比较严重，这可能由于磁盘大量作随机访问造成，也有可能磁盘出现瓶颈（块操作）。

*   **id**：空闲时间百分比

> 一般来说，us + sy + id =100
>

#### 

### 定时采样（常用）

```shell
# 每隔3s采样一次，一共采样10次
$ vmstat 3 10
```

### 以指定的单位显示（常用）

```shell
# 以MB为单位来显示
$ vmstat -S M

# 以MB为单位来显示，每隔3s采样一次，一共采样10次
$ vmstat -S M 3 10
```

### 显示活跃和非活跃内存

```shell
$ vmstat -a
```

### 查看内存使用的详细信息

```shell
$ vmstat -s
```

### 查看磁盘的读/写

```shell
$ vmstat -d
```

#### 
