ifconfig
===

配置和显示网络接口

## 补充说明

ifconfig是interface configuration的缩写，它是一个用来配置和显示Linux内核中网络接口的网络工具。ifconfig可以用来设置网络接口的状态，如启动或停止，分配IP地址，设置子网掩码，修改MAC地址等。ifconfig也可以用来显示网络接口的信息，如IP地址，MAC地址，传输速率，数据包统计等。

## 适用的Linux版本

ifconfig是一个传统的Linux命令，它在大多数Linux发行版中都可以使用，但是一些较新的发行版，如Ubuntu 18.04，CentOS 8，Fedora 28等，已经不再默认安装ifconfig，而是使用ip命令来代替。ip命令是一个更强大和灵活的网络工具，它支持IPv6，可以管理路由，隧道，流量控制等功能。如果你想在这些发行版中使用ifconfig，你需要手动安装net-tools包，这是一个包含了ifconfig和其他一些传统网络工具的软件包。安装net-tools的命令如下：

* Ubuntu/Debian

```shell
$ sudo apt install net-tools
```

* CentOS 8/Fedora 28

```shell
$ sudo dnf install net-tools
```

##  命令语法

```shell
ifconfig [interface] [option]
```

* interface：指定要配置或显示的网络接口的名称，如eth0，lo等。如果不指定网络接口，ifconfig会显示所有活动的网络接口的信息。

* option：用来设置或修改网络接口的选项，如IP地址，子网掩码，广播地址等。如果不指定参数，ifconfig只会显示网络接口的信息，不会进行任何修改。

##  选项

```shell
-a	显示所有网络接口的信息，包括未激活的
-s	显示简化的网络接口信息
up	激活指定的网络接口
down	停止指定的网络接口
IP地址	设置指定网络接口的IP地址
netmask 地址	设置指定网络接口的子网掩码
broadcast 地址	设置指定网络接口的广播地址
hw 类型 地址	设置指定网络接口的硬件类型和地址，如hw ether 00:11:22:33:44:55
mtu 数字	设置指定网络接口的最大传输单元（MTU）
promisc	开启指定网络接口的混杂模式，可以接收所有经过的数据包
-promisc	关闭指定网络接口的混杂模式
arp	开启指定网络接口的地址解析协议（ARP）
-arp	关闭指定网络接口的地址解析协议（ARP）
allmulti	开启指定网络接口的所有多播模式，可以接收所有多播数据包
-allmulti	关闭指定网络接口的所有多播模式
```

##  示例

### 显示所有激活状态的网络接口信息

```shell
$ ifconfig
eth0      Link encap:Ethernet  HWaddr 00:16:3E:00:1E:51  
          inet addr:10.160.7.81  Bcast:10.160.15.255  Mask:255.255.240.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:61430830 errors:0 dropped:0 overruns:0 frame:0
          TX packets:88534 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:3607197869 (3.3 GiB)  TX bytes:6115042 (5.8 MiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:56103 errors:0 dropped:0 overruns:0 frame:0
          TX packets:56103 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:5079451 (4.8 MiB)  TX bytes:5079451 (4.8 MiB)
```

* **eth0** 和 **lo** 表示两块不同的网卡，其中 **lo** 是主机的回坏地址
  * Link encap：网卡接口类型，Ethernet（以太网）、Local Loopback （本地环回）
  * HWaddr：网卡的硬件地址，即MAC地址
  * inet addr：IPv4地址，也即是我们常说的网卡的IP地址
  * Bcast：广播地址
  * Mask：子网掩码地址
  * inet6 addr：IPv6地址
  * Scope：是用来限制 IPv6 组播的作用范围，Host（主机本地范围）、Link（链路本地范围）、Site（站点本地范围，已废弃）、Global（全局范围）
  * UP：表示网卡开启状态，如果网卡关闭时则不显示。
  * BROADCAST：表示网卡支持广播
  * RUNNING：表示网卡正在运行
  * MULTICAST ：表示网卡支持组播，如果网卡不支持则不显示
  * MTU：最大传输单元，详细的参考：https://developer.aliyun.com/article/222535
  * Metric：跃点数，通常是指到达目的地址所需的跃点数量，一个跃点代表一个路由器。另外，跃点值越大表示优先级越大
  * RX一行：分别表示网卡从启动到现在所接收的 总包数、错误数、丢弃数、过载数、帧数
  * TX一行：分别表示网卡从启动到现在所发送的 总包数、错误数、丢弃数、过载数、帧数
  * collisions：数据包发生冲突、碰撞的次数，次数多了说明网络不太好。
  * txqueuelen：发送队列长度
  * RX bytes：接收到的字节数
  * TX bytes：发送出的字节数
  * Interrupt：IRQ中断地址
  * Base address：基本地址

### 显示指定网络接口的信息

```shell
# 显示指定网络接口的信息，如eth0
$ ifconfig eth0

# 显示指定网络接口的IP地址
$ ifconfig eth0 | grep 'inet ' | awk '{print $2}'

# 显示指定网络接口的MAC地址
$ ifconfig eth0 | grep 'ether ' | awk '{print $2}'

# 显示指定网络接口的数据包统计
$ ifconfig eth0 | grep 'RX packets\|TX packets'
```

### 启动关闭指定网卡

```shell
ifconfig eth0 up
ifconfig eth0 down
```

`ifconfig eth0 up`为启动网卡eth0，`ifconfig eth0 down`为关闭网卡eth0。ssh登陆linux服务器操作要小心，关闭了就不能开启了，除非你有多网卡。

### 设置IPv4地址

```shell
# 给eth0网卡配置IP地址
$ ifconfig eth0 192.168.120.56

# 给eth0网卡配置IP地址，并加上子掩码
$ ifconfig eth0 192.168.120.56 netmask 255.255.255.0

# 给eth0网卡配置IP地址，并加上子掩码和广播地址
$ ifconfig eth0 192.168.120.56 netmask 255.255.255.0 broadcast 192.168.120.255
```

### 设置IPv6地址 

```shell
# 为网卡eth0配置IPv6地址
ifconfig eth0 add 33ffe:3240:800:1005::2/64

# 为网卡eth0删除IPv6地址
ifconfig eth0 del 33ffe:3240:800:1005::2/64
```

### 设置MAC地址

```shell
ifconfig eth0 hw ether 00:AA:BB:CC:dd:EE
```

### 启用和关闭arp协议

```shell
# 开启网卡eth0 的arp协议
ifconfig eth0 arp

# 关闭网卡eth0 的arp协议
ifconfig eth0 -arp
```

### 设置最大传输单元

```shell
# 设置能通过的最大数据包大小为 1500 bytes
ifconfig eth0 mtu 1500
```

