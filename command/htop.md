htop
===

[非内部命令]一个交互式的进程查看器，可以动态观察系统进程状况

## 补充说明

htop 是一个 Linux 下的交互式的进程浏览器，可以用来替换Linux下的top命令。

与Linux传统的top相比，htop更加人性化。它可让用户交互式操作，支持颜色主题，可横向或纵向滚动浏览进程列表，并支持鼠标操作。

与top相比，htop有以下优点：

- 可以横向或纵向滚动浏览进程列表，以便看到所有的进程和完整的命令行。
- 在启动上，比top 更快。
- 杀进程时不需要输入进程号。
- htop 支持鼠标操作。

htop 官网：http://htop.sourceforge.net/

## 适用的Linux版本

在大多数的 Linux 发行版中你不会找到预安装的 `htop`，但作为最流行的实用程序之一，你会在几乎每个 Linux 发行版的默认存储库中找到 `htop`。

* Debian/Ubuntu 系统

```shell
sudo apt install htop
```

* Fedora 系统

```shell
sudo dnf install htop
```

* CentOS/RedHat 系统

```shell
sudo yum install htop
```
如果你想避免从源代码构建包，还有一个 Snap 包可用：

```shell
sudo snap install htop
```

如果你使用的是其它的发行版或者想从源代码构建，你可以使用 `wget` 下载并安装：
这需要你下载并安装 `wget` `cmake`

```shell
wget https://link.zhihu.com/?target=https%3A//hisham.hm/htop/releases/2.2.0/htop-2.2.0.tar.gz

tar -zxvf htop-2.2.0.tar.gz

cd htop-2.2.0/

./configure

make

make install
```

当然你也可以随时参考你可以随时参考 [htop 的 GitHub](https://link.zhihu.com/?target=https%3A//github.com/htop-dev/htop) 页面以获得详细说明。

**说明**：htop源码安装方式默认安装到 `/usr/local` 目录下，如果想安装到其它路径，在执行 configure 时通过 `—prefix` 指定，格式为：`./configure --prefix=/some/path`

##  命令语法

```shell
htop [options]
```

## 选项

```shell
-C --no-color               使用单色配色方案
-d --delay=DELAY            设置更新之间的延迟，在十秒
-s --sort-key=COLUMN        纵列排序(try --sort-key=help for a list)
-u --user=USERNAME          只显示一个指定用户的进程
-p --pid=PID,[,PID,PID...]  只显示给用户
-h --help                   打印此命令帮助
-v --version                打印版本信息
```

参数示例

- -C 选项：设置界面为无颜色。

- -d 选项 : 设置刷新时间，单位为秒。如，htop -d 10命令会每10秒刷新一次。

- -s 选项 : 按指定的列排序。如，htop -s PID命令会按PID 列的大小排序来显示。

- -u 选项 : 显示指定的用户的进程信息。如，htop -u test命令会只显示出用户名为test的相关进程。

##  常用操作

```shell
上下键或PgUP， PgDn : 移动选中进程  
左右键或Home， End : 移动列表  
Space(空格) : 标记/取消标记一个进程。命令可以作用于多个进程，例如 "kill"，将应用于所有已标记的进程  

F1：查看htop使用说明
F2：设置
F3：搜索，等同 /
F4：过滤，只显示符合条件的结果，等同 \
F5：显示树形结构，可以查看进程的父子关系。
F6：选择排序方式，可以选择排序的列。
F7：提高选中进程的优先级，需要超级用户权限。
F8：降低选中进程的优先级，需要超级用户权限。
F9：杀死选中进程，需要超级用户权限。
F10：退出htop

/：搜索字符
h：显示帮助
l：显示进程打开的文件: 如果安装了lsof，按此键可以显示进程所打开的文件
u：显示所有用户，并可以选择某一特定用户的进程
U：取消标记所有的进程
s：将调用strace追踪进程的系统调用
t：显示树形结构
H：显示/隐藏用户线程
I：倒转排序顺序
K：显示/隐藏内核线程    
M：按内存占用排序
P：按CPU排序    
T：按运行时间排序
```

## 示例

### 查看系统资源和进程

运行htop命令不带任何选项，就可以查看系统的资源使用情况和所有的进程。

```shell
$ htop
```

![image-20241226160111801](https://s1.locimg.com/2024/12/26/5c492aff94a4c.png)

* **CPU**：显示了系统的CPU使用情况，每个CPU核心对应一个彩色的条形图，颜色的含义是：

  - 蓝色：显示低优先级(low priority)进程使用的CPU百分比。

  - 绿色：显示用于普通用户(user)拥有的进程的CPU百分比。

  - 红色：显示系统进程(kernel threads)使用的CPU百分比。

  - 橙色：显示IRQ时间使用的CPU百分比。

  - 洋红色(Magenta)：显示Soft IRQ时间消耗的CPU百分比。

  - 灰色：显示IO等待时间消耗的CPU百分比。

  - 青色：显示窃取时间(Steal time)消耗的CPU百分比。

* **Mem/Swp**：分别代表物理内存（Memory）和交换内存（Swap）的使用情况，也有多种颜色

  - 绿色：已使用的内存。

  - 蓝色：缓冲区。

  - 黄色：缓存。

* **Tasks, thr, running**：计算机上运行的 31 个任务(tasks)被分解为 97 个线程(thread)，其中只有 1 个进程处于运行(running)状态。其他状态分类有：

  - R：Running，表示进程(process)正在使用CPU

  - S：Sleeping:，通常进程在大多数时间都处于睡眠状态，并以固定的时间间隔执行小检查，或者等待用户输入后再返回运行状态。

  - T/S：Traced/Stoped，表示进程正在处于暂停的状态

  - Z：Zombie or defunct，已完成执行但在进程表中仍具有条目的进程。

* **Load Average**：表示系统的负载，负载是指在一定时间内系统的平均任务数，运行时间是指系统自上次启动以来的时间。三个值分别代表系统在最后1分钟，最近5分钟和最后15分钟的平均负载 (0.00 0.01 0.05)

* **Uptime**：表示这个系统一共运行了多长的时间，这里一共运行了2小时36分50秒

* 再下面显示了系统的所有进程，每个进程占一行，每列显示了进程的不同属性，可以通过F2键来自定义显示的列，也可以通过鼠标或方向键来选择进程。默认的列有：

  - PID – 描述进程的ID号

  - USER – 描述进程的所有者

  - PRI – 描述Linux内核查看的进程优先级

  - NI – 描述由用户或root重置的进程优先级

  - VIR – 它描述进程正在使用的虚拟内存 （virtual memory）

  - RES – 描述进程正在消耗的物理内存（physical memory）

  - SHR – 描述进程正在使用的共享内存（shared memory）

  - S – 进程的状态，有以下几种：
    - R：运行中。
    - S：睡眠中。
    - D：不可中断的睡眠中。
    - Z：僵尸状态。
    - T：停止状态。
    - I：空闲状态。

  - CPU％ – 描述每个进程消耗的CPU百分比

  - MEM％ – 描述每个进程消耗的内存百分比

  - TIME+ – 显示自流程开始执行以来的时间

  - Command –它与每个进程并行显示完整的命令执行 (比如/usr/lib/R)

### 按照CPU使用率排序进程

运行htop命令时，可以使用-s或--sort-key选项来指定排序的列，也可以在交互界面中使用F6键来选择排序的列。

```shell
$ htop -s PERCENT_CPU
```

### 只显示指定用户的进程

运行htop命令时，可以使用-u或--user选项来指定只显示某个用户的进程，也可以在交互界面中使用F4键来过滤某个用户的进程。

```shell
$ htop -u root
```

### 切换树状视图

运行htop命令时，可以使用F5键来切换树状视图，也可以在设置界面中勾选Tree view选项来开启树状视图。树状视图可以显示进程的父子关系，方便查看进程的层级结构。

```shell
$ htop
```

然后按F5键，或者按F2键，进入设置界面，选择Display options，勾选Tree view，按回车键确认。

