ss
===

比 netstat 更高效好用的 socket 统计信息工具

## 补充说明

**ss命令** 用来显示处于活动状态的套接字信息。ss命令可以用来获取socket统计信息，它可以显示和netstat类似的内容。但ss的优势在于它能够显示更多更详细的有关TCP和连接状态的信息，而且比netstat更快速更高效。

当服务器的socket连接数量变得非常大时，无论是使用netstat命令还是直接`cat /proc/net/tcp`，执行速度都会很慢。而ss快的秘诀在于，它利用到了TCP协议栈中tcp_diag。tcp_diag是一个用于分析统计的模块，可以获得Linux 内核中第一手的信息，这就确保了ss的快捷高效。当然，如果你的系统中没有tcp_diag，ss也可以正常运行，只是效率会变得稍慢。

## 适用的Linux版本

ss 命令适用于几乎所有现代 Linux 发行版，通常包含在 iproute2 包中。如果系统上未安装 ss 命令，可以通过以下命令进行安装：

```shell
# 基于apt的发行版（如Debian、Ubuntu、Raspbian、Kali Linux等）
$ sudo apt-get update && sudo apt-get install iproute2

# 基于yum的发行版（如RedHat，CentOS 7等）
$ sudo yum update && sudo yum install iproute

# 基于dnf的发行版（如Fedora，CentOS 8等）
$ sudo dnf update && sudo dnf install iproute

# 基于apk的发行版（如Alpine Linux）
$ sudo apk add --update iproute2

# 基于pacman的发行版（如Arch Linux）
$ sudo pacman -Syu && sudo pacman -S iproute2

# 基于zypper的发行版（如openSUSE）
$ sudo zypper ref && sudo zypper in iproute2

# 基于pkg的FreeBSD发行版
$ sudo pkg update && sudo pkg install iproute2

# 基于pkg的OS X/macOS发行版
$ brew update && brew install iproute2
```

## 命令语法

```shell
ss [OPTIONS]
ss [OPTIONS] [FILTER]
```

## 选项

```shell
-h, --help      帮助信息
-V, --version   程序版本信息
-n, --numeric   不解析服务名称
-r, --resolve   解析主机名
-a, --all       显示所有套接字（sockets）
-l, --listening 显示监听状态的套接字（sockets）
-o, --options   显示计时器信息
-e, --extended  显示详细的套接字（sockets）信息
-m, --memory    显示套接¬字（socket）的内存使用情况
-p, --processes 显示使用套接字（socket）的进程
-i, --info      显示 TCP内部信息
-s, --summary   显示套接字（socket）使用概况
-4, --ipv4      仅显示IPv4的套接字（sockets）
-6, --ipv6      仅显示IPv6的套接字（sockets）
-0, --packet    显示 PACKET 套接字（socket）
-t, --tcp       仅显示 TCP套接字（sockets）
-u, --udp       仅显示 UCP套接字（sockets）
-d, --dccp      仅显示 DCCP套接字（sockets）
-w, --raw       仅显示 RAW套接字（sockets）
-x, --unix      仅显示 Unix套接字（sockets）
-f, --family=FAMILY  显示 FAMILY类型的套接字（sockets），FAMILY可选，支持  unix, inet, inet6, link, netlink
-A, --query=QUERY, --socket=QUERY
      QUERY := {all|inet|tcp|udp|raw|unix|packet|netlink}[,QUERY]
-D, --diag=FILE     将原始TCP套接字（sockets）信息转储到文件
 -F, --filter=FILE  从文件中都去过滤器信息
       FILTER := [ state TCP-STATE ] [ EXPRESSION ]
```

## 示例

### 实例1：列出所有打开的套接字

```shell
$ ss -a
```

这条命令会显示系统中所有状态的套接字。

### 实例2：列出所有TCP套接字

```shell
$ ss -ta
```

使用`-t`选项来只显示TCP套接字，结合`-a`选项可以查看所有TCP套接字的状态。

### 实例3：列出所有监听状态的TCP端口

```shell
$ ss -lt
```

加上`-l`选项，`ss`命令将显示当前系统上处于监听状态的 TCP 套接字。

### 实例4：列出所有监听状态的UDP端口

```shell
$ ss -lu
```

使用`-u`选项来只显示UDP套接字，结合`-l`选项则仅显示监听状态的UDP端口。

### 实例5：显示每个套接字的进程信息

```shell
$ ss -lp
```

`-p`选项用于显示每个套接字关联的进程信息。这对于确定哪个进程正在监听或使用特定端口非常有帮助。

### 实例6：查找监听特定端口（例如80端口）的进程

```shell
$ ss -lntp | grep ':80'
```

此命令组合使用了`-lnt`选项来列出所有TCP监听套接字并不解析服务名称，`-p`用于显示进程信息。然后通过`grep`命令过滤出监听80端口的记录。

### 实例7：仅显示有关TCP套接字的统计信息

```shell
$ ss -s
```

使用`-s`选项可以查看当前套接字的统计摘要。这包括不同类型套接字的总数、已建立的连接数、已关闭的连接数等。

### 实例8：显示所有IPv4和IPv6的TCP和UDP套接字

```shell
$ ss -tua -A inet,inet6
```

- `A`选项后面可以指定套接字地址族，`inet`对应IPv4，而`inet6`对应IPv6。结合`tua`可以列出所有TCP和UDP套接字。

### 实例9：监控TCP套接字的状态变化

```shell
$ watch -n 1 ss -tp
```

`watch`命令可以用来周期性地执行`ss`命令。这个例子中，`-n 1`表示每1秒执行一次`ss -tp`，从而可以动态监控TCP套接字的状态变化。

### 实例10：显示所有处于TIME-WAIT状态的TCP连接

```shell
$ ss -o state time-wait
```

使用`-o`选项可以显示计时器信息，而`state time-wait`过滤条件可以筛选出所有处于TIME-WAIT状态的TCP连接。

### 实例11：列出所有ESTABLISHED状态的连接并显示其计时器信息

```shell
$ ss -t -o state established
```

## 注意事项

* 使用ss命令时通常需要具有root权限，尤其是在使用-p选项查看套接字关联的进程信息时。
* ss命令具有强大的过滤器，可以使用复杂的条件来过滤显示的套接字。
* 某些选项可能在各个Linux发行版的ss版本中表现略有不同，查阅相应的手册页(man ss)可以获得更准确的信息。
