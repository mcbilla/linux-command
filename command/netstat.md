netstat
===

查看网络系统状态信息

## 补充说明

netstat的全称是network statistics（网络统计），是一个用来显示网络状态的命令，它可以显示网络连接、路由表、接口统计、伪装连接、多播成员等信息，是Linux和Unix-like系统中的一个常用工具，也可以在Windows和Mac上使用。netstat可以帮助网站管理员和内容创建者分析和优化他们的网站性能和安全。

###  适用的Linux版本

netstat 命令在大多数Linux发行版中都是可用的，但是在一些较新的版本中，它可能被ss命令所取代。ss命令是socket statistics（套接字统计）的缩写，它提供了更多的功能和更快的速度。如果你的Linux系统没有安装netstat命令，你可以使用以下方法来安装它：

* Debian、Ubuntu

```shell
sudo apt-get install net-tools
```

* CentOS7

```shell
sudo yum install net-tools
```

* CentOS8

```shell
sudo dnf install net-tools
```

* Manjaro

```shell
sudo pacman -S net-tools
```

## 命令语法

```shell
netstat [OPTION]
```

##  选项

```shell
-a或--all：显示所有的网络连接和监听端口
-A<网络类型>或--<网络类型>：列出该网络类型连线中的相关地址
-c或--continuous：持续显示网络状态，每隔一秒刷新一次
-C或--cache：显示路由器配置的快取信息
-e或--extend：显示扩展的信息，如用户ID和进程ID
-F或--fib：显示FIB
-g或--groups：显示多重广播功能群组组员名单
-h或--help：在线帮助
-i或--interfaces：显示网络接口的统计信息
-l或--listening：只显示监听状态的套接字
-M或--masquerade：显示伪装的网络连线
-n或--numeric：直接使用ip地址，而不通过域名服务器
-N或--netlink或--symbolic：显示网络硬件外围设备的符号连接名称
-o或--timers：显示计时器
-p或--programs：显示每个套接字对应的进程ID和程序名
-r或--route：显示Routing Table
-s或--statistice：显示网络工作信息统计表
-t或--tcp：只显示TCP协议的连接
-u或--udp：只显示UDP协议的连接
-v或--verbose：显示指令执行过程
-V或--version：显示版本信息
-w或--raw：只显示RAW协议的连接
-x或--unix：此参数的效果和指定"-A unix"参数相同
--ip或--inet：此参数的效果和指定"-A inet"参数相同
```

##  示例

### 默认参数使用

```shell
$ netstat

Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State      
tcp        0    268 192.168.120.204:ssh         10.2.0.68:62420             ESTABLISHED 
udp        0      0 192.168.120.204:4371        10.58.119.119:domain        ESTABLISHED 
Active UNIX domain sockets (w/o servers)
Proto RefCnt Flags       Type       State         I-Node Path
unix  2      [ ]         DGRAM                    1491   @/org/kernel/udev/udevd
unix  4      [ ]         DGRAM                    7337   /dev/log
unix  2      [ ]         DGRAM                    708823 
unix  2      [ ]         DGRAM                    7539   
unix  3      [ ]         STREAM     CONNECTED     7287   
unix  3      [ ]         STREAM     CONNECTED     7286   
```

netstat 的输出结果可以分为两个部分：

* Active Internet connections，称为有源TCP连接，其中 `Recv-Q` 和 `Send-Q` 指的是接收队列和发送队列。这些数字一般都应该是0。如果不是则表示软件包正在队列中堆积。这种情况只能在非常少的情况见到。
* Active UNIX domain sockets，称为有源Unix域套接口(和网络套接字一样，但是只能用于本机通信，性能可以提高一倍)。

字段说明：

* Proto：显示连接使用的协议
* RefCnt：表示连接到本套接口上的进程号
* Types：显示套接口的类型
* State：显示套接口当前的状态，共有12中可能的状态，前面11种是按照TCP连接建立的三次握手和TCP连接断开的四次挥手过程来描述的：
  1. LISTEN：首先服务端需要打开一个socket进行监听，状态为 LISTEN，侦听来自远方TCP端口的连接请求 ；

  2. SYN_SENT：客户端通过应用程序调用connect进行active open，于是客户端tcp发送一个SYN以请求建立一个连接，之后状态置为 SYN_SENT，在发送连接请求后等待匹配的连接请求；

  3. SYN_RECV：服务端应发出ACK确认客户端的 SYN，同时自己向客户端发送一个SYN，之后状态置为，在收到和发送一个连接请求后等待对连接请求的确认；

  4. ESTABLISHED：代表一个打开的连接，双方可以进行或已经在数据交互了， 代表一个打开的连接，数据可以传送给用户；

  5. FIN_WAIT1：主动关闭(active close)端应用程序调用close，于是其TCP发出FIN请求主动关闭连接，之后进入FIN_WAIT1状态， 等待远程TCP的连接中断请求，或先前的连接中断请求的确认；

  6. CLOSE_WAIT：被动关闭(passive close)端TCP接到FIN后，就发出ACK以回应FIN请求(它的接收也作为文件结束符传递给上层应用程序)，并进入CLOSE_WAIT， 等待从本地用户发来的连接中断请求；

  7. FIN_WAIT2：主动关闭端接到ACK后，就进入了 FIN-WAIT-2，从远程TCP等待连接中断请求；

  8. LAST_ACK：被动关闭端一段时间后，接收到文件结束符的应用程 序将调用CLOSE关闭连接，这导致它的TCP也发送一个 FIN,等待对方的ACK.就进入了LAST-ACK，等待原来发向远程TCP的连接中断请求的确认；

  9. TIME_WAIT:在主动关闭端接收到FIN后，TCP 就发送ACK包，并进入TIME-WAIT状态，等待足够的时间以确保远程TCP接收到连接中断请求的确认；

  10. CLOSING：比较少见，等待远程TCP对连接中断的确认；

  11. CLOSED：被动关闭端在接受到ACK包后，就进入了closed的状态，连接结束，没有任何连接状态；

  12. UNKNOWN：未知的Socket状态；
* Path：表示连接到套接口的其它进程使用的路径名

### 列出所有端口 (包括监听和未监听的)

```shell
netstat -a     #列出所有端口
netstat -at    #列出所有tcp端口
netstat -au    #列出所有udp端口                             
```

### 列出所有处于监听状态的 Sockets

```shell
netstat -l        #只显示监听端口
netstat -lt       #只列出所有监听 tcp 端口
netstat -lu       #只列出所有监听 udp 端口
netstat -lx       #只列出所有监听 UNIX 端口
```

### 显示每个协议的统计信息

```shell
netstat -s    #显示所有端口的统计信息
netstat -st   #显示TCP端口的统计信息
netstat -su   #显示UDP端口的统计信息
```

### 显示 PID 和进程名称

```shell
netstat -pt
```

`netstat -p`可以与其它开关一起使用，就可以添加 `PID/进程名称` 到netstat输出中，这样debugging的时候可以很方便的发现特定端口运行的程序。

### 找出程序占用的端口

并不是所有的进程都能找到，没有权限的会不显示，使用 root 权限查看所有的信息。

```shell
netstat -ap | grep ssh
```

找出运行在指定端口的进程：

```shell
netstat -an | grep ':80'
```

### 通过端口找进程ID

```bash
netstat -anp|grep 8081 | grep LISTEN|awk '{printf $7}'|cut -d/ -f1
```

### 不显示主机，端口和用户名(host, port or user)

当你不想让主机，端口和用户名显示，使用`netstat -n`。将会使用数字代替那些名称。同样可以加速输出，因为不用进行比对查询。

```shell
netstat -an
```

如果只是不想让这三个名称中的一个被显示，使用以下命令:

```shell
netsat -a --numeric-ports
netsat -a --numeric-hosts
netsat -a --numeric-users
```

### 持续输出netstat信息

```shell
netstat -c   #每隔一秒输出网络信息
```

### 显示系统不支持的地址族(Address Families)

```shell
netstat --verbose
```

在输出的末尾，会有如下的信息：

```shell
netstat: no support for `AF IPX' on this system.
netstat: no support for `AF AX25' on this system.
netstat: no support for `AF X25' on this system.
netstat: no support for `AF NETROM' on this system.
```

### 显示核心路由信息

```shell
netstat -r

netstat -rn  #显示数字格式，不查询主机名称。
```

### 显示网络接口列表

```shell
netstat -i   #显示网络接口列表

netstat -ie  #显示详细信息，类似于ifconfig
```

### IP和TCP分析

查看连接某服务端口最多的的IP地址：

```shell
netstat -ntu | grep :80 | awk '{print $5}' | cut -d: -f1 | awk '{++ip[$1]} END {for(i in ip) print ip[i],"\t",i}' | sort -nr
```

TCP各种状态列表：

```shell
netstat -nt | grep -e 127.0.0.1 -e 0.0.0.0 -e ::: -v | awk '/^tcp/ {++state[$NF]} END {for(i in state) print i,"\t",state[i]}'
```

查看phpcgi进程数，如果接近预设值，说明不够用，需要增加：

```shell
netstat -anpo | grep "php-cgi" | wc -l
```
