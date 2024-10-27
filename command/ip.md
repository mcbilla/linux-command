ip
===

管理网络接口和路由

## 补充说明

ip命令是Internet Protocol Command的缩写，它是一个用于管理网络接口和路由的命令，可以替代传统的ifconfig、route等命令。ip命令可以显示、添加、删除、修改网络接口的属性，以及显示、添加、删除、修改路由表、ARP表、邻居表等。ip命令的功能非常强大，可以用来配置复杂的网络环境，如VLAN、隧道、策略路由等。

## 适用的Linux版本

ip命令是Linux内核的一部分，因此适用于所有的Linux发行版，包括CentOS、Ubuntu、Debian、Fedora等。如果系统中没有ip命令，可以根据不同的Linux发行版使用相应的包管理器来安装。

* CentOS 7/8

```shell
sudo yum install iproute
```

* Debian/Ubuntu/Deepin/Kali Linux/Raspbian

```shell
sudo apt-get install iproute2
```

* Fedora

```shell
sudo dnf install iproute
```

* Arch Linux

```shell
sudo pacman -S iproute2
```

* Alpine Linux

```shell
sudo apk add iproute2
```

##  命令语法

```shell
ip [OPTIONS] OBJECT { COMMAND | help }
```

##  对象

OBJECT 为常用对象，值可以是以下几种：

```shell
OBJECT={ link | addr | addrlabel | route | rule | neigh | ntable | tunnel | maddr | mroute | mrule | monitor | xfrm | token }
```

常用对象的取值含义如下：

* link：网络设备
* address：设备上的协议（IP或IPv6）地址
* addrlabel：协议地址选择的标签配置
* route：路由表条目
* rule：路由策略数据库中的规则

##  选项

OPTIONS 为常用选项，值可以是以下几种：

```
OPTIONS={ -V[ersion] | -s[tatistics] | -d[etails] | -r[esolve] | -h[uman-readable] | -iec | -f[amily] { inet | inet6 | ipx | dnet | link } | -o[neline] | -t[imestamp] | -b[atch] [filename] | -rc[vbuf] [size] }
```

常用选项的取值含义如下：

| 选项或参数 | 说明                              |
| ---------- | --------------------------------- |
| -4         | 使用IPv4协议                      |
| -6         | 使用IPv6协议                      |
| -s         | 显示更多的统计信息                |
| -c         | 使用彩色输出                      |
| -f         | 指定地址族，如inet、inet6、link等 |
| -o         | 以单行的形式输出信息              |
| -d         | 显示详细的信息                    |
| -V         | 显示ip命令的版本信息              |
| -h         | 显示ip命令的帮助信息              |
| link       | 操作网络接口                      |
| addr       | 操作网络接口的地址                |
| route      | 操作路由表                        |
| neigh      | 操作邻居表                        |
| show       | 显示对象的信息                    |
| add        | 添加对象                          |
| del        | 删除对象                          |
| change     | 修改对象                          |
| replace    | 替换对象                          |
| name       | 指定网络接口的名称                |
| address    | 指定网络接口的地址                |
| dev        | 指定网络接口的设备                |
| via        | 指定路由的下一跳地址              |
| table      | 指定路由表的编号或名称            |

## 示例

### 实例1：显示所有网络接口的信息

要显示所有网络接口的信息，可以使用以下命令：

```shell
$ ip link show
```

输出结果如下：

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 00:11:22:33:44:66 brd ff:ff:ff:ff:ff:ff
```

输出结果中，每一行表示一个网络接口的信息，包括接口的编号、名称、状态、类型、MAC地址等。

### 实例2：显示指定网络接口的信息

要显示指定网络接口的信息，可以使用以下命令：

```shell
$ ip link show dev eth0
```

输出结果如下：

```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff
```

输出结果中，只显示了eth0接口的信息。

### 实例3：显示所有网络接口的地址

要显示所有网络接口的地址，可以使用以下命令：

```shell
$ ip addr show
```

输出结果如下：

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
       valid_lft 86399sec preferred_lft 86399sec
    inet6 fe80::211:22ff:fe33:4455/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 00:11:22:33:44:66 brd ff:ff:ff:ff:ff:ff
```

输出结果中，每一行表示一个网络接口的地址信息，包括接口的类型、地址、掩码、广播地址、作用域等。

### 实例4：显示指定网络接口的地址

要显示指定网络接口的地址，可以使用以下命令：

```shell
$ ip addr show dev eth0
```

输出结果如下：

```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff
...
```

### 实例5：为网络接口添加IPv4地址

要为网络接口添加IPv4地址，可以使用以下命令：

```shell
$ ip addr add 192.168.1.100/24 dev eth0
```

这个命令会为eth0接口添加一个192.168.1.100/24的IPv4地址，其中/24表示子网掩码为255.255.255.0。

### 实例6：为网络接口添加IPv6地址

要为网络接口添加IPv6地址，可以使用以下命令：

```shell
$ ip addr add 2001:db8::1/64 dev eth0
```

这个命令会为eth0接口添加一个2001:db8::1/64的IPv6地址，其中/64表示子网掩码为ffff:ffff:ffff:ffff::。

### 实例7：删除网络接口的地址

要删除网络接口的地址，可以使用以下命令：

```shell
$ ip addr del 192.168.1.100/24 dev eth0
```

这个命令会删除eth0接口的192.168.1.100/24的IPv4地址。

### 实例8：显示路由表的信息

要显示路由表的信息，可以使用以下命令：

```shell
$ ip route show
```

输出结果如下：

```
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100
```

输出结果中，每一行表示一条路由规则，包括目的地址、下一跳地址、出口接口、协议、作用域、源地址等。

### 实例9：显示指定目的地址的路由信息

要显示指定目的地址的路由信息，可以使用以下命令：

```shell
$ ip route get 8.8.8.8
```

输出结果如下：

```
8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.100
    cache
```

输出结果中，显示了到达8.8.8.8的最佳路由，包括下一跳地址、出口接口、源地址等。

### 实例10：添加一条静态路由

要添加一条静态路由，可以使用以下命令：

```shell
$ ip route add 10.0.0.0/8 via 192.168.1.2 dev eth0
```

这个命令会添加一条静态路由，表示到达10.0.0.0/8网段的数据包，需要通过192.168.1.2的网关，从eth0接口发送。

### 实例11：删除一条静态路由

要删除一条静态路由，可以使用以下命令：

```shell
$ ip route del 10.0.0.0/8 via 192.168.1.2 dev eth0
```

这个命令会删除一条静态路由，表示不再使用192.168.1.2的网关，从eth0接口发送到达10.0.0.0/8网段的数据包。

### 实例12：显示ARP表的信息

要显示ARP表的信息，可以使用以下命令：

```shell
$ ip neigh show
```

输出结果如下：

```
192.168.1.1 dev eth0 lladdr 00:aa:bb:cc:dd:ee REACHABLE
192.168.1.2 dev eth0 lladdr 00:aa:bb:cc:dd:ff STALE
```

输出结果中，每一行表示一条ARP记录，包括目的地址、出口接口、MAC地址、状态等。

### 实例13：添加一条ARP记录

要添加一条ARP记录，可以使用以下命令：

```shell
$ ip neigh add 192.168.1.3 lladdr 00:aa:bb:cc:dd:11 dev eth0
```

这个命令会添加一条ARP记录，表示192.168.1.3的MAC地址为00:aa:bb:cc:dd:11，从eth0接口发送数据包。

### 实例14：删除一条ARP记录

要删除一条ARP记录，可以使用以下命令：

```shell
$ ip neigh del 192.168.1.3 lladdr 00:aa:bb:cc:dd:11 dev eth0
```

这个命令会删除一条ARP记录，表示不再使用00:aa:bb:cc:dd:11的MAC地址，从eth0接口发送数据包到192.168.1.3。

### 实例15：刷新ARP表

要刷新ARP表，可以使用以下命令：

```shell
$ ip neigh flush dev eth0
```

这个命令会刷新eth0接口的所有ARP记录，表示需要重新解析目的地址的MAC地址。

## 注意事项

在使用ip命令时，有以下几点需要注意：

- ip命令需要root权限或sudo权限才能执行，否则会提示Permission denied。
- ip命令的操作和参数可能会因为Linux内核的版本和发行版的不同而有所差异，因此在使用ip命令时，最好先查看ip命令的帮助信息或手册页，以了解具体的用法和参数。
- ip命令的操作和参数可能会影响网络的正常通信，因此在使用ip命令时，最好先备份原有的网络配置，以便在出现问题时恢复。
- 如果在执行ip命令时，提示bash: ip: command not found，表示系统没有安装ip命令，需要先安装iproute2软件包。

