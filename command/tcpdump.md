tcpdump
===

抓包工具，用于捕获和分析网络流量

## 补充说明

tcpdump是一个用于捕获和分析网络流量的命令行工具。它是网络管理员用于排除网络问题和安全测试的最常用的工具之一。使用tcpdump，你不仅可以捕获TCP协议的流量，还可以捕获非TCP协议的流量，如UDP，ARP或ICMP。捕获的数据包可以写入文件或标准输出。tcpdump命令的最强大的特性之一是它可以使用过滤器，只捕获你感兴趣的数据。

## 适用的Linux版本

tcpdump在大多数Linux发行版和macOS上默认安装。如果你的电脑没有安装tcpdump，可以通过下面方式来安装：

* Ubuntu，Debian和Linux Mint

```shell
sudo apt install tcpdump
```

* CentOS7

```shell
sudo yum install tcpdump
```

* CentOS8，Fedora，AlmaLinux

```shell
sudo dnf install tcpdump
```

* Arch，Manjaro

```shell
sudo pacman -S tcpdump
```

##  命令语法

```shell
tcpdump [options] [proto] [dir] [type]
```

options 是可选参数选项，其他参数选项都属于**过滤器**，主要分为三类：

* **proto**：根据协议进行过滤，可识别的关键词有： tcp, udp, icmp, ip, ip6, arp, rarp,ether,wlan, fddi, tr, decnet。
* **dir**：根据数据流向进行过滤，可识别的关键字有：src, dst，同时你可以使用逻辑运算符进行组合，比如 src or dst。
* **type**：根据类型过滤，可识别的关键词有：host, net, port, portrange，这些词后边需要再接参数。默认为 host。

##  选项 

### 设置不解析域名提升速度

- `-n`：不把ip转化成域名，直接显示 ip，避免执行 DNS lookups 的过程，速度会快很多
- `-nn`：不把协议和端口号转化成名字，速度也会快很多。
- `-N`：不打印出host 的域名部分.。比如,，如果设置了此选现，tcpdump 将会打印'nic' 而不是 'nic.ddn.mil'.

### 过滤结果输出到文件

使用 tcpdump 工具抓到包后，往往需要再借助其他的工具进行分析，比如常见的 wireshark 。

而要使用wireshark ，我们得将 tcpdump 抓到的包数据生成到文件中，最后再使用 wireshark 打开它即可。

使用 `-w` 参数后接一个以 `.pcap` 后缀命令的文件名，就可以将 tcpdump 抓到的数据保存到文件中。

```
$ tcpdump icmp -w icmp.pcap
```

### 从文件中读取包数据

使用 `-w` 是写入数据到文件，而使用 `-r` 是从文件中读取数据。

读取后，我们照样可以使用上述的过滤器语法进行过滤分析。

```
$ tcpdump icmp -r all.pcap
```

### 控制详细内容的输出

- `-v`：产生详细的输出. 比如包的TTL，id标识，数据包长度，以及IP包的一些选项。同时它还会打开一些附加的包完整性检测，比如对IP或ICMP包头部的校验和。
- `-vv`：产生比-v更详细的输出. 比如NFS回应包中的附加域将会被打印, SMB数据包也会被完全解码。（摘自网络，目前我还未使用过）
- `-vvv`：产生比-vv更详细的输出。比如 telent 时所使用的SB, SE 选项将会被打印, 如果telnet同时使用的是图形界面，其相应的图形选项将会以16进制的方式打印出来（摘自网络，目前我还未使用过）

### 控制时间的显示

- `-t`：在每行的输出中不输出时间
- `-tt`：在每行的输出中会输出时间戳
- `-ttt`：输出每两行打印的时间间隔(以毫秒为单位)
- `-tttt`：在每行打印的时间戳之前添加日期的打印（此种选项，输出的时间最直观）

### 显示数据包的头部

- `-x`：以16进制的形式打印每个包的头部数据（但不包括数据链路层的头部）
- `-xx`：以16进制的形式打印每个包的头部数据（包括数据链路层的头部）
- `-X`：以16进制和 ASCII码形式打印出每个包的数据(但不包括连接层的头部)，这在分析一些新协议的数据包很方便。
- `-XX`：以16进制和 ASCII码形式打印出每个包的数据(包括连接层的头部)，这在分析一些新协议的数据包很方便。

### 过滤指定网卡的数据包

- `-i`：指定要过滤的网卡接口，如果要查看所有网卡，可以 `-i any`

### 过滤特定流向的数据包

- `-Q`： 选择是入方向还是出方向的数据包，可选项有：in, out, inout，也可以使用 --direction=[direction] 这种写法

### 其他常用的一些参数

- `-A`：以ASCII码方式显示每一个数据包(不显示链路层头部信息). 在抓取包含网页数据的数据包时, 可方便查看数据
- `-l` : 基于行的输出，便于你保存查看，或者交给其它工具分析
- `-q` : 简洁地打印输出。即打印很少的协议相关信息, 从而输出行都比较简短.
- `-c` : 捕获 count 个包 tcpdump 就退出
- `-s` : tcpdump 默认只会截取前 `96` 字节的内容，要想截取所有的报文内容，可以使用 `s number`， `number` 就是你要截取的报文字节数，如果是 0 的话，表示截取报文全部内容。
- `-S` : 使用绝对序列号，而不是相对序列号
- `-C`：file-size，tcpdump 在把原始数据包直接保存到文件中之前, 检查此文件大小是否超过file-size. 如果超过了, 将关闭此文件,另创一个文件继续用于原始数据包的记录. 新创建的文件名与-w 选项指定的文件名一致, 但文件名后多了一个数字.该数字会从1开始随着新创建文件的增多而增加. file-size的单位是百万字节(nt: 这里指1,000,000个字节,并非1,048,576个字节, 后者是以1024字节为1k, 1024k字节为1M计算所得, 即1M=1024 ＊ 1024 ＝ 1,048,576)
- `-F`：使用file 文件作为过滤条件表达式的输入, 此时命令行上的输入将被忽略.

### 对输出内容进行控制的参数

- `-D` : 显示所有可用网络接口的列表
- `-e` : 每行的打印输出中将包括数据包的数据链路层头部信息
- `-E`: 揭秘IPSEC数据
- `-L`：列出指定网络接口所支持的数据链路层的类型后退出
- `-Z`：后接用户名，在抓包时会受到权限的限制。如果以root用户启动tcpdump，tcpdump将会有超级用户权限。
- `-d`：打印出易读的包匹配码
- `-dd`：以C语言的形式Help打印出包匹配码.
- `-ddd`：以十进制数的形式打印出包匹配码

## 过滤规则

网络流量的数据包非常多，要抓到我们所需要的数据包，就需要我们定义一个精准的过滤器，把这些目标数据包，从巨大的数据包网络中抓取出来。所以学习抓包工具，其实就是学习如何定义过滤器的过程。

过滤器也可以简单地分为三类：

* **proto**：根据协议进行过滤，可识别的关键词有： tcp, udp, icmp, ip, ip6, arp, rarp,ether,wlan, fddi, tr, decnet。
* **dir**：根据数据流向进行过滤，可识别的关键字有：src, dst，同时你可以使用逻辑运算符进行组合，比如 src or dst。
* **type**：根据类型过滤，可识别的关键词有：host, net, port, portrange，这些词后边需要再接参数。默认为 host。

例如使用 `host` 就可以指定 host ip 进行过滤

```
$ tcpdump host 192.168.10.100
```

数据包的 ip 可以再细分为源ip和目标ip两种

```
# 根据源ip进行过滤
$ tcpdump -i eth2 src 192.168.10.100

# 根据目标ip进行过滤
$ tcpdump -i eth2 dst 192.168.10.200
```

可以同时使用多个过滤器，使用逻辑表达式进行拼接

* `&&` 或者 `and` ：且关系
* `||` 或者 `or`：或关系
* `!` 或者 `not` ：非关系

举个例子，我想需要抓一个来自`10.5.2.3`，发往任意主机的3389端口的包

```
$ tcpdump src 10.5.2.3 and dst port 3389
```

当你在使用多个过滤器进行组合时，有可能需要用到括号，而括号在 shell 中是特殊符号，因为你需要使用引号将其包含。例子如下：

```
$ tcpdump 'src 10.0.2.4 and (dst port 3389 or 22)'
```

而在单个过滤器里，常常会判断一条件是否成立，这时候，就要使用下面两个符号

- `=`：判断二者相等
- `==`：判断二者相等
- `!=`：判断二者不相等

当你使用这两个符号时，tcpdump 还提供了一些关键字的接口来方便我们进行判断，比如

- if：表示网卡接口名、
- proc：表示进程名
- pid：表示进程 id
- svc：表示 service class
- dir：表示方向，in 和 out
- eproc：表示 effective process name
- epid：表示 effective process ID

比如我现在要过滤来自进程名为 `nc` 发出的流经 en0 网卡的数据包，或者不流经 en0 的入方向数据包，可以这样子写

```
$ tcpdump "( if=en0 and proc=nc ) || (if!=en0 and dir=in)"
```

## 理解 tcpdump 的输出

tcpdump 的输出格式总体上为：

```
系统时间 源主机.端口 > 目标主机.端口 数据包参数
```

以 TCP 的三次握手过程为例

```
06:35:05.274759 IP 172.20.20.1.58563 > 172.18.0.2.3306: Flags [S], seq 2597376001, win 65535, options [mss 1460], length 0
06:35:05.275132 IP 172.18.0.2.3306 > 172.20.20.1.58563: Flags [S.], seq 1153192404, ack 2597376002, win 29200, options [mss 1460], length 0
06:35:05.275464 IP 172.20.20.1.58563 > 172.18.0.2.3306: Flags [.], ack 1, win 65535, length 0
```

从上面的输出来看，可以总结出：

1. 第一列：时分秒毫秒 06:35:05.274759
2. 第二列：网络协议 IP
3. 第三列：发送方的 ip 地址+端口号，其中 172.20.20.1 是 ip，而 58563 是端口号
4. 第四列：箭头 >， 表示数据流向
5. 第五列：接收方的 ip 地址+端口号，其中 172.18.0.2 是 ip，而 3306 是端口号
6. 第六列：冒号
7. 第七列：数据包内容，包括 Flags 标识符，seq 号，ack 号，win 窗口，数据长度 length 等

常见的 TCP 报文 Flags有以下几种：

- `[S]` : SYN（开始连接）
- `[P]` : PSH（推送数据）
- `[F]` : FIN （结束连接）
- `[R]` : RST（重置连接）
- `[.]` : 没有 Flag （意思是除上面四种类型外的其他情况，有可能是 ACK 也有可能是 URG）
- `[S.]` 表示 `SYN-ACK`，就是 `SYN` 报文的应答报文。

##  示例

### 抓取某主机的数据包

抓取主机 172.18.82.173 上所有收到（DST_IP）和发出（SRC_IP）的所有数据包

```shell
tcpdump host 172.18.82.173
```

抓取经过指定网口 interface ，并且 DST_IP 或 SRC_IP 是 172.18.82.173 的数据包

```shell
tcpdump -i eth0 host 172.18.82.173
```

筛选 SRC_IP，抓取经过 interface 且从 172.18.82.173 发出的包

```shell
tcpdump -i eth0 src host 172.18.82.173
```

筛选 DST_IP，抓取经过 interface 且发送到 172.18.82.173 的包

```shell
tcpdump -i eth0 dst host 172.18.82.173
```

抓取主机 200.200.200.1 和主机 200.200.200.2 或 200.200.200.3 通信的包

```shell
tcpdump host 200.200.200.1 and \(200.200.200.2 or 200.200.200.3\)
```

抓取主机 200.200.200.1 和除了主机 200.200.200.2 之外所有主机通信的包

```shell
tcpdump ip host 200.200.200.1 and ! 200.200.200.2
```

### 抓取某端口的数据包

抓取所有端口，显示 IP 地址

```shell
tcpdump -nS
```

抓取某端口上的包

```shell
Copytcpdump port 22
```

抓取经过指定 interface，并且 DST_PORT 或 SRC_PORT 是 22 的数据包

```shell
tcpdump -i eth0 port 22
```

筛选 SRC_PORT

```shell
tcpdump -i eth0 src port 22
```

筛选 DST_PORT

```shell
tcpdump -i eth0 dst port 22
```

比如希望查看发送到 host 172.18.82.173 的网口 eth0 的 22 号端口的包

```shell
[root@by ~]# tcpdump -i eth0 -nnt dst host 172.18.82.173 and port 22 -c 1 -vv
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
IP (tos 0x14, ttl 114, id 27674, offset 0, flags [DF], proto TCP (6), length 40)
    113.98.59.61.51830 > 172.18.82.173.22: Flags [.], cksum 0x7fe3 (correct), seq 19775964, ack 1564316089, win 2053, length 0
```

### 抓取某网络（网段）的数据包

抓取经过指定 interface，并且 DST_NET 或 SRC_NET 是 172.18.82 的包

```shell
tcpdump -i eth0 net 172.18.82
```

筛选 SRC_NET

```shell
tcpdump -i eth0 src net 172.18.82
```

筛选 DST_NET

```shell
tcpdump -i eth0 dst net 172.18.82
```

###  抓取某协议的数据包

```shell
tcpdump -i eth0 icmp
tcpdump -i eth0 ip
tcpdump -i eth0 tcp
tcpdump -i eth0 udp
tcpdump -i eth0 arp
```

### 复杂的逻辑表达式抓取过滤条件

> 对于复杂的过滤器表达式，为了逻辑清晰，可以使用 `()`，不过默认情况下，tcpdump 会将 `()` 当做特殊字符，所以必须使用 `''` 来消除歧义。

抓取经过 interface eth0 发送到 host 200.200.200.1 或 200.200.200.2 的 TCP 协议 22 号端口的数据包

```shell
tcpdump -i eth0 -nntvv -c 10 '((tcp) and (port 22) and ((dst host 200.200.200.1) or (dst host 200.200.200.2)))'
```

抓取经过 interface eth0， DST_MAC 或 SRC_MAC 地址是 00:16:3e:12:16:e7 的 ICMP 数据包

```shell
tcpdump -i eth0 '((icmp) and ((ether host 00:16:3e:12:16:e7)))' -nnvv
```

抓取经过 interface eth0，目标网络是 172.18 但目标主机又不是 172.18.82.173 的 TCP 且非 22 号端口号的数据包

```shell
tcpdump -i eth0 -nntvv '((dst net 172.18) and (not dst host 172.18.82.173) and (tcp) and (not port 22))'
```

抓取流入 interface eth0，host 为 172.18.82.173 且协议为 ICMP 的数据包

```shell
tcpdump -i eth0 -nntvv -P in host 172.18.82.173 and icmp
```

抓取流出 interface eth0，host 为 172.18.82.173 且协议为 ICMP 的数据包

```shell
tcpdump -i eth0 -nntvv -P out host 172.18.82.173 and icmp
```

### 结合 Wireshark 进行分析

通常 `Wireshark`（或 tshark）比 tcpdump 更容易分析应用层协议。一般的做法是在远程服务器上先使用 `tcpdump` 抓取数据并写入文件，然后再将文件拷贝到本地工作站上用 `Wireshark` 分析。

把所有 80 端口的数据导出到文件。

```
tcpdump -w capture_file.pcap port 80

```

读取文件 capture_file.pcap 里的数据报文，显示到屏幕上。

```
tcpdump -nXr capture_file.pcap host host1
```

> `.pcap` 格式的文件需要用 wireshark、Snort 等工具查看，使用 `vim` 或 `cat` 会出现乱码。

### 提取 HTTP 请求的信息

从 HTTP 请求头中提取 HTTP 的 User-Agent：

```
$ tcpdump -nn -A -s1500 -l | grep "User-Agent:"
```

通过 `egrep` 可以同时提取User-Agent 和主机名（或其他头文件）：

```
$ tcpdump -nn -A -s1500 -l | egrep -i 'User-Agent:|Host:'
```

抓取 HTTP GET 请求包：

```
$ tcpdump -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x47455420'

# or

$ tcpdump -vvAls0 | grep 'GET'
```

可以抓取 HTTP POST 请求包：

```
$ tcpdump -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504f5354'

# or

$ tcpdump -vvAls0 | grep 'POST'
```

> 该方法不能保证抓取到 HTTP POST 有效数据流量，因为一个 POST 请求会被分割为多个 TCP 数据包。

从 HTTP POST 请求中提取密码和主机名：

```
$ tcpdump -s 0 -A -n -l | egrep -i "POST /|pwd=|passwd=|password=|Host:"
```

提取 HTTP 请求的主机名和路径：

```
$ tcpdump -s 0 -v -n -l | egrep -i "POST /|GET /|Host:"
```

抓取 80 端口的 HTTP 有效数据包，排除 TCP 连接建立过程的数据包（SYN / FIN / ACK）：

```
$ tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

### 找出发包数最多的 IP

找出一段时间内发包最多的 IP，或者从一堆报文中找出发包最多的 IP，可以使用下面的命令：

```
$ tcpdump -nnn -t -c 200 | cut -f 1,2,3,4 -d '.' | sort | uniq -c | sort -nr | head -n 20
```

- **cut -f 1,2,3,4 -d '.'** : 以 `.` 为分隔符，打印出每行的前四列。即 IP 地址。
- **sort | uniq -c** : 排序并计数
- **sort -nr** : 按照数值大小逆向排序

## 注意事项

* 在使用 tcpdump 命令时，要注意你的终端的缓冲区大小，如果捕获的数据包太多，可能会导致终端无法显示完整的输出。你可以使用 `-w` 选项将数据包写入文件，然后用 `-r` 选项从文件中读取数据包，或者使用其他工具来分析数据包，如 Wireshark。
* 在使用 tcpdump 命令时，要注意你的网络接口的模式，如果你的网络接口是混杂模式，那么你可以捕获所有经过该接口的数据包，包括不是发给你的数据包。如果你的网络接口是非混杂模式，那么你只能捕获发给你的数据包。你可以使用 ifconfig 命令来查看或设置你的网络接口的模式。
* 在使用 tcpdump 命令时，要注意你的过滤表达式的逻辑，如果你的过滤表达式有多个条件，你需要使用括号来分组，否则可能会得到意想不到的结果。例如，如果你想捕获目标端口为80或443，且源端口为22的数据包，你需要使用括号来分组，否则可能会得到意想不到的结果。例如，如果你想捕获目标端口为80或443，且源端口为22的数据包，你应该使用这样的表达式：

```shell
$ tcpdump -i ens3 '(dst port 80 or dst port 443) and src port 22'
```

而不是这样的表达式：

```shell
$ tcpdump -i ens3 dst port 80 or dst port 443 and src port 22
```

因为后者的逻辑是捕获目标端口为80，或者目标端口为443且源端口为22的数据包，这可能不是你想要的结果。

* 在使用 tcpdump 命令时，要注意你的网络流量的安全性，如果你捕获的数据包包含敏感信息，如密码，信用卡号等，你应该使用加密算法和密钥来保护你的数据包，或者使用 -E 选项来解密 IPsec 流量。你也应该避免将你的数据包文件分享给不可信的人或机构，以防止数据泄露。
