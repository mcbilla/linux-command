nc
===

网络界的瑞士军刀，号称最强大的网络工具

## 补充说明

nc命令全称netcat，是一款功能强大的网络工具，可以用来在网络上读、写和重定向数据。它可以打开TCP或UDP连接，发送或接收数据包，监听任意端口，进行端口扫描，支持IPv4和IPv6。它被称为网络界的瑞士军刀，因为它可以完成很多网络相关的任务，如聊天、文件传输、代理、后门、端口转发等。nc命令也可以用来编写脚本或其他程序调用的可靠的后端工具。

## 适用的Linux版本

nc命令在大多数Linux发行版中都是默认可用的，或者可以通过包管理器安装。不过不同的发行版可能提供不同的版本或变体，如ncat、netcat、netcat-openbsd、netcat-traditional等。这些版本或变体可能有不同的选项或功能，所以在使用之前需要查看手册页或帮助信息来确定具体的用法。如果需要安装nc命令，可以根据不同的发行版使用不同的包管理器，如：

- 在Debian/Ubuntu上，可以使用apt-get命令安装netcat-openbsd或netcat-traditional包：

```shell
# 安装netcat-openbsd
sudo apt-get install netcat-openbsd

# 安装netcat-traditional
sudo apt-get install netcat-traditional
```

- 在CentOS/RHEL上，可以使用yum或dnf命令安装nmap-ncat包：

```shell
# 使用yum命令
sudo yum install nmap-ncat

# 使用dnf命令（CentOS 8/RHEL 8）
sudo dnf install nmap-ncat
```

- 在Fedora上，可以使用dnf命令安装nmap-ncat或netcat包：

```shell
# 安装nmap-ncat
sudo dnf install nmap-ncat

# 安装netcat
sudo dnf install netcat
```

- 在Arch Linux上，可以使用pacman命令安装gnu-netcat或openbsd-netcat包：

```shell
# 安装gnu-netcat
sudo pacman -S gnu-netcat

# 安装openbsd-netcat
sudo pacman -S openbsd-netcat
```

## 命令语法

```
nc [options] hostname port [port] ...
```

* options是指定一些选项来控制nc命令的行为
* hostname是指定要连接或监听的主机名或IP地址
* port是指定要连接或监听的端口号，可以是一个范围。

## 选项

| 选项 | 说明                                       |
| ---- | ------------------------------------------ |
| -4   | 强制使用IPv4                               |
| -6   | 强制使用IPv6                               |
| -c   | 执行指定的shell命令，并将其输出发送到网络  |
| -e   | 将指定的程序与网络连接关联                 |
| -g   | 设置路由器跃程通信网关（最多8个）          |
| -G   | 设置来源路由指针（必须是4的倍数）          |
| -h   | 显示帮助信息                               |
| -i   | 设置时间间隔（秒），用于发送数据和扫描端口 |
| -k   | 保持监听状态，并接受多个连接               |
| -l   | 使用监听模式，等待传入连接                 |
| -n   | 不进行域名解析                             |
| -o   | 将传输数据以16进制格式保存到指定文件       |
| -p   | 指定本地端口号                             |
| -r   | 随机选择本地和远程端口号                   |
| -s   | 指定本地IP地址                             |
| -u   | 使用UDP协议                                |
| -v   | 显示详细信息                               |
| -w   | 设置超时时间（秒）                         |
| -z   | 使用零输入/输出模式，仅用于扫描端口        |

## 示例

### 1、端口扫描（常用）
```shell
# 扫描单个端口，-v显示详细过程，-w 10表示等待超时最大时长为10s，-z表示使用端口扫描模式
$ nc -v -w 10 -z 127.0.0.1 7891
Connection to 127.0.0.1 7891 port [tcp/*] succeeded!

# 范围扫描，扫描1到1000的端口
$ nc -v -w 10 -z 127.0.0.1 1—1000
```

### 2、监听端口
```shell
# 监听tcp端口
$ nc -l 8080

# 监听udp端口
$ nc -l -u 1234
```

### 3、连接远程系统
```shell
# 连接远程系统
$ nc 192.168.1.100 80

# 测试某个远程主机 UDP 端口的连通性
$ nc -v -u 192.168.1.100 80
```

### 4、聊天系统
```shell
# HOST1运行
$ nc -l 8080

# HOST2运行
$ nc HOST1 8080
```
之后开始发送消息，这些消息会在服务器终端上显示出来。

### 5、发送文件
```shell
# 从 HOST1（Client） 发送文件到 HOST2（Server） 的 TCP 9899 端口
# HOST2：
$ nc -l 9899 > outputfile

# HOST1：
$ nc HOST2 9899 < inputfile
```

### 6、创建系统后门
```shell
# HOST1上执行
$ nc -l 10000 -e /bin/bash

# HOST2上执行
$ nc HOST1 10000
```
这样可以在HOST2上通过HOST1的10000端口，通过bash获取我们系统的完整访问权限，常用于黑客后门攻击。

### 6、端口转发
```shell
# 所有连接到 80 端口的连接都会转发到 8080 端口
$ nc -u -l  80 -c  'ncat -u -l 8080'
```

### 7、创建代理服务器
```shell
# 在本机 8888 端口创建 HTTP 代理
$ nc -l --proxy-type http localhost 8888 --proxy-auth username:password

# 测试 HTTP 代理服务器（支持隧道模式）
$ curl -x 'http://username:password@localhost:8888' httpbin.org/anything
```

### 8、通过代理服务器连接
```shell
# 通过 SOCKS5 1080 端口，连接 smtphost:25
$ nc --proxy socks5host --proxy-type socks5 --proxy-auth proxyusername:password smtphost 25

# SSH 通过 Ncat 指定代理，通过代理访问
$ ssh -o ProxyCommand="ncat --proxy-type http/socks4/socks5 --proxy proxy.net:端口 %h %p" user@server.net
```