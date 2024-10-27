iptables
===

Linux上常用的防火墙软件

## 补充说明

**iptables命令** 是Linux上常用的防火墙软件，是netfilter项目的一部分。可以直接配置，也可以通过许多前端和图形界面配置。

Iptabels是与Linux内核集成的包过滤防火墙系统，几乎所有的linux发行版本都会包含Iptables的功能。如果 Linux 系统连接到因特网或 LAN、服务器或连接 LAN 和因特网的代理服务器， 则Iptables有利于在 Linux 系统上更好地控制 IP 信息包过滤和防火墙配置。

netfilter/iptables过滤防火墙系统是一种功能强大的工具，可用于添加、编辑和除去规则，这些规则是在做信息包过滤决定时，防火墙所遵循和组成的规则。这些规则存储在专用的信 息包过滤表中，而这些表集成在 Linux 内核中。在信息包过滤表中，规则被分组放在我们所谓的链（chain）中。

虽然netfilter/iptables包过滤系统被称为单个实体，但它实际上由两个组件netfilter 和 iptables 组成。

netfilter 组件也称为内核空间（kernelspace），是内核的一部分，由一些信息包过滤表组成，这些表包含内核用来控制信息包过滤处理的规则集。

iptables 组件是一种工具，也称为用户空间（userspace），它使插入、修改和除去信息包过滤表中的规则变得容易。

## 基本原理

```shell
                             ┏╍╍╍╍╍╍╍╍╍╍╍╍╍╍╍┓
 ┌───────────────┐           ┃    Network    ┃
 │ table: filter │           ┗━━━━━━━┳━━━━━━━┛
 │ chain: INPUT  │◀────┐             │
 └───────┬───────┘     │             ▼
         │             │   ┌───────────────────┐
  ┌      ▼      ┐      │   │ table: nat        │
  │local process│      │   │ chain: PREROUTING │
  └             ┘      │   └─────────┬─────────┘
         │             │             │
         ▼             │             ▼              ┌─────────────────┐
┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅    │     ┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅      │table: nat       │
 Routing decision      └───── outing decision ─────▶│chain: PREROUTING│
┅┅┅┅┅┅┅┅┅┳┅┅┅┅┅┅┅┅┅          ┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅      └────────┬────────┘
         │                                                   │
         ▼                                                   │
 ┌───────────────┐                                           │
 │ table: nat    │           ┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅               │
 │ chain: OUTPUT │    ┌─────▶ outing decision ◀──────────────┘
 └───────┬───────┘    │      ┅┅┅┅┅┅┅┅┳┅┅┅┅┅┅┅┅
         │            │              │
         ▼            │              ▼
 ┌───────────────┐    │   ┌────────────────────┐
 │ table: filter │    │   │ chain: POSTROUTING │
 │ chain: OUTPUT ├────┘   └──────────┬─────────┘
 └───────────────┘                   │
                                     ▼
                             ┏╍╍╍╍╍╍╍╍╍╍╍╍╍╍╍┓
                             ┃    Network    ┃
                             ┗━━━━━━━━━━━━━━━┛
```

1. 一个数据包进入网卡时，它首先进入PREROUTING链，内核根据数据包目的IP判断是否需要转发出去。
2. 如果数据包就是进入本机的，它就会沿着图向下移动，到达INPUT链。数据包到了INPUT链后，任何进程都会收到它。本机上运行的程序可以发送数据包，这些数据包会经 过OUTPUT链，然后到达POSTROUTING链输出。
3. 如果数据包是要转发出去的，且内核允许转发，数据包就会如图所示向右移动，经过 FORWARD链，然后到达POSTROUTING链输出。

### **规则（rules）**

规则（rules）其实就是网络管理员预定义的条件，规则一般的定义为“如果数据包头符合这样的条件，就这样处理这个数据包”。规则存储在内核空间的信息包过滤表中，这些规则分别指定了源地址、目的地址、传输协议（如TCP、UDP、ICMP）和服务类型（如HTTP、FTP和SMTP）等。当数据包与规则匹配时，iptables就根据规则所定义的方法来处理这些数据包，如放行（accept）、拒绝（reject）和丢弃（drop）等。配置防火墙的主要工作就是添加、修改和删除这些规则。

### **链（chains）**

链（chains）是数据包传播的路径，每一条链其实就是众多规则中的一个检查清单，每一条链中可以有一条或数条规则。当一个数据包到达一个链时，iptables就会从链中第一条规则开始检查，看该数据包是否满足规则所定义的条件。如果满足，系统就会根据该条规则所定义的方法处理该数据包；否则iptables将继续检查下一条规则，如果该数据包不符合链中任一条规则，iptables就会根据该链预先定义的默认策略来处理数据包。

### **表（tables）**

表（tables）提供特定的功能，iptables内置了4个表，即raw表、filter表、nat表和mangle表，分别用于实现包过滤，网络地址转换和包重构的功能。

#### raw表

只使用在PREROUTING链和OUTPUT链上,因为优先级最高，从而可以对收到的数据包在连接跟踪前进行处理。一但用户使用了RAW表,在 某个链上,RAW表处理完后,将跳过NAT表和 ip_conntrack处理,即不再做地址转换和数据包的链接跟踪处理了.

#### filter表

主要用于过滤数据包，该表根据系统管理员预定义的一组规则过滤符合条件的数据包。对于防火墙而言，主要利用在filter表中指定的规则来实现对数据包的过滤。Filter表是默认的表，如果没有指定哪个表，iptables 就默认使用filter表来执行所有命令，filter表包含了INPUT链（处理进入的数据包），RORWARD链（处理转发的数据包），OUTPUT链（处理本地生成的数据包）在filter表中只能允许对数据包进行接受，丢弃的操作，而无法对数据包进行更改

#### nat表

主要用于网络地址转换NAT，该表可以实现一对一，一对多，多对多等NAT 工作，iptables就是使用该表实现共享上网的，NAT表包含了PREROUTING链（修改即将到来的数据包），POSTROUTING链（修改即将出去的数据包），OUTPUT链（修改路由之前本地生成的数据包）

#### mangle表

主要用于对指定数据包进行更改，在内核版本2.4.18 后的linux版本中该表包含的链为：INPUT链（处理进入的数据包），RORWARD链（处理转发的数据包），OUTPUT链（处理本地生成的数据包）POSTROUTING链（修改即将出去的数据包），PREROUTING链（修改即将到来的数据包）

## 命令格式

iptables的命令格式较为复杂，一般的格式如下：

```bash
iptables [-t 表] 命令 匹配 操作
```

### 表

-t 表选项用于指定命令应用于哪个iptables内置表。

内建的规则表有四个，分别是

- raw
- filter
- nat
- mangle

当未指定规则表时，则一律视为是 filter。

### 命令

命令选项用于指定iptables的执行方式，包括插入规则，删除规则和添加规则，常用命令说明

```
-P  --policy        <链名>  定义默认策略
-L  --list          <链名>  查看iptables规则列表
-A  --append        <链名>  在规则列表的最后增加1条规则
-I  --insert        <链名>  在指定的位置插入1条规则
-D  --delete        <链名>  从规则列表中删除1条规则
-R  --replace       <链名>  替换规则列表中的某条规则
-F  --flush         <链名>  删除表中所有规则
-Z  --zero          <链名>  将表中数据包计数器和流量计数器归零
-X  --delete-chain  <链名>  删除自定义链
-v  --verbose       <链名>  与-L他命令一起使用显示更多更详细的信息
```

### 匹配规则

匹配选项指定数据包与规则匹配所具有的特征，包括源地址，目的地址，传输协议和端口号。常用匹配规则如下

```
-i --in-interface    网络接口名>     指定数据包从哪个网络接口进入，
-o --out-interface   网络接口名>     指定数据包从哪个网络接口输出
-p ---proto          协议类型        指定数据包匹配的协议，如TCP、UDP和ICMP等
-s --source          源地址或子网>   指定数据包匹配的源地址
   --sport           源端口号>       指定数据包匹配的源端口号
   --dport           目的端口号>     指定数据包匹配的目的端口号
-m --match           匹配的模块      指定数据包规则所使用的过滤模块
```

iptables执行规则时，是从规则表中从上至下顺序执行的，如果没遇到匹配的规则，就一条一条往下执行，如果遇到匹配的规则后，那么就执行本规则，执行后根据本规则的动作(accept，reject，log，drop等)，决定下一步执行的情况，后续执行一般有三种情况。

- 一种是继续执行当前规则队列内的下一条规则。比如执行过Filter队列内的LOG后，还会执行Filter队列内的下一条规则。
- 一种是中止当前规则队列的执行，转到下一条规则队列。比如从执行过accept后就中断Filter队列内其它规则，跳到nat队列规则去执行
- 一种是中止所有规则队列的执行。

### 动作

前面我们说过iptables处理动作除了 ACCEPT、REJECT、DROP、REDIRECT 、MASQUERADE 以外，还多出 LOG、ULOG、DNAT、RETURN、TOS、SNAT、MIRROR、QUEUE、TTL、MARK等。我们只说明其中最常用的动作：

**REJECT** 拦阻该数据包，并返回数据包通知对方，可以返回的数据包有几个选择：ICMP port-unreachable、ICMP echo-reply 或是tcp-reset（这个数据包包会要求对方关闭联机），进行完此处理动作后，将不再比对其它规则，直接中断过滤程序。 范例如下：

```
iptables -A  INPUT -p TCP --dport 22 -j REJECT --reject-with ICMP echo-reply
```

**DROP** 丢弃数据包不予处理，进行完此处理动作后，将不再比对其它规则，直接中断过滤程序。

**REDIRECT** 将封包重新导向到另一个端口（PNAT），进行完此处理动作后，将会继续比对其它规则。这个功能可以用来实作透明代理 或用来保护web 服务器。例如：

```
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT--to-ports 8081
```

**MASQUERADE** 改写封包来源IP为防火墙的IP，可以指定port 对应的范围，进行完此处理动作后，直接跳往下一个规则链（mangle:postrouting）。这个功能与 SNAT 略有不同，当进行IP 伪装时，不需指定要伪装成哪个 IP，IP 会从网卡直接读取，当使用拨接连线时，IP 通常是由 ISP 公司的 DHCP服务器指派的，这个时候 MASQUERADE 特别有用。范例如下：

```
iptables -t nat -A POSTROUTING -p TCP -j MASQUERADE --to-ports 21000-31000
```

**LOG** 将数据包相关信息纪录在 /var/log 中，详细位置请查阅 /etc/syslog.conf 配置文件，进行完此处理动作后，将会继续比对其它规则。例如：

```
iptables -A INPUT -p tcp -j LOG --log-prefix "input packet"
```

**SNAT** 改写封包来源 IP 为某特定 IP 或 IP 范围，可以指定 port 对应的范围，进行完此处理动作后，将直接跳往下一个规则炼（mangle:postrouting）。范例如下：

```
iptables -t nat -A POSTROUTING -p tcp-o eth0 -j SNAT --to-source 192.168.10.15-192.168.10.160:2100-3200
```

**DNAT** 改写数据包包目的地 IP 为某特定 IP 或 IP 范围，可以指定 port 对应的范围，进行完此处理动作后，将会直接跳往下一个规则链（filter:input 或 filter:forward）。范例如下：

```
iptables -t nat -A PREROUTING -p tcp -d 15.45.23.67 --dport 80 -j DNAT --to-destination 192.168.10.1-192.168.10.10:80-100
```

**MIRROR** 镜像数据包，也就是将来源 IP与目的地IP对调后，将数据包返回，进行完此处理动作后，将会中断过滤程序。

**QUEUE** 中断过滤程序，将封包放入队列，交给其它程序处理。透过自行开发的处理程序，可以进行其它应用，例如：计算联机费用.......等。

**RETURN** 结束在目前规则链中的过滤程序，返回主规则链继续过滤，如果把自订规则炼看成是一个子程序，那么这个动作，就相当于提早结束子程序并返回到主程序中。

**MARK** 将封包标上某个代号，以便提供作为后续过滤的条件判断依据，进行完此处理动作后，将会继续比对其它规则。范例如下：

```
iptables -t mangle -A PREROUTING -p tcp --dport 22 -j MARK --set-mark 22
```

看了本文是不是对iptables参数有所了解了，下文我会使用实例来更详细的说明iptables的参数的用法。

## 示例

### 常用命令示例

1、命令 -A, --append

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

说明 ：新增规则到INPUT规则链中，规则时接到所有目的端口为80的数据包的流入连接，该规则将会成为规则链中的最后一条规则。

2、命令 -D, --delete

```bash
iptables -D INPUT -p tcp --dport 80 -j ACCEPT

或

iptables -D INPUT 1
```

说明： 从INPUT规则链中删除上面建立的规则，可输入完整规则，或直接指定规则编号加以删除。

3、命令 -R, --replace

```bash
iptables -R INPUT 1 -s 192.168.0.1 -j DROP
```

说明 取代现行第一条规则，规则被取代后并不会改变顺序。

4、命令 -I, --insert

```bash
iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT
```

说明： 在第一条规则前插入一条规则，原本该位置上的规则将会往后移动一个顺位。

5、命令 -L, --list

```bash
iptables -L INPUT
```

说明：列出INPUT规则链中的所有规则。

6、命令 -F, --flush

```bash
iptables -F INPUT
```

说明： 删除INPUT规则链中的所有规则。

7、命令 -Z, --zeroLINUX教程 centos教程

```bash
iptables -Z INPUT
```

说明 将INPUT链中的数据包计数器归零。它是计算同一数据包出现次数，过滤阻断式攻击不可少的工具。

8、命令 -N, --new-chain

```bash
iptables -N denied
```

说明： 定义新的规则链。

9、命令 -X, --delete-chain

```bash
iptables -X denied
```

说明： 删除某个规则链。

10、命令 -P, --policy

```bash
iptables -P INPUT DROP
```

说明 ：定义默认的过滤策略。 数据包没有找到符合的策略，则根据此预设方式处理。

11、命令 -E, --rename-chain

```bash
iptables -E denied disallowed
```

说明： 修改某自订规则链的名称。

### 常用封包比对参数

1、参数 -p, --protocol

```bash
iptables -A INPUT -p tcpLINUX教程 centos教程
```

说明：比对通讯协议类型是否相符，可以使用 ! 运算子进行反向比对，例如：-p ! tcp ，意思是指除 tcp 以外的其它类型，包含udp、icmp ...等。如果要比对所有类型，则可以使用 all 关键词，例如：-p all。

2、参数 -s, --src, --source

```bash
iptables -A INPUT -s 192.168.1.100
```

说明：用来比对数据包的来源IP，可以比对单机或网络，比对网络时请用数字来表示屏蔽，例如：-s 192.168.0.0/24，比对 IP 时可以使用!运算子进行反向比对，例如：-s ! 192.168.0.0/24。

3、参数 -d, --dst, --destination

```bash
iptables -A INPUT -d 192.168.1.100
```

说明：用来比对封包的目的地 IP，设定方式同上。

4、参数 -i, --in-interface

```bash
iptables -A INPUT -i lo
```

说明:用来比对数据包是从哪个网卡进入，可以使用通配字符 + 来做大范围比对，如：-i eth+ 表示所有的 ethernet 网卡，也可以使用 ! 运算子进行反向比对，如：-i ! eth0。这里lo指本地换回接口。

5、参数 -o, --out-interface

```bash
iptables -A FORWARD -o eth0
```

说明：用来比对数据包要从哪个网卡流出，设定方式同上。

6、参数 --sport, --source-port

```bash
iptables -A INPUT -p tcp --sport 22
```

说明：用来比对数据的包的来源端口号，可以比对单一端口，或是一个范围，例如：--sport 22:80，表示从 22 到 80 端口之间都算是符合件，如果要比对不连续的多个端口，则必须使用 --multiport 参数，详见后文。比对端口号时，可以使用 ! 运算子进行反向比对。

7、参数 --dport, --destination-port

```bash
iptables -A INPUT -p tcp --dport 22
```

说明 用来比对封包的目的地端口号，设定方式同上。

8、参数 --tcp-flags

```bash
iptables -p tcp --tcp-flags SYN,FIN,ACK SYN
```

说明：比对 TCP 封包的状态标志号，参数分为两个部分，第一个部分列举出想比对的标志号，第二部分则列举前述标志号中哪些有被设，未被列举的标志号必须是空的。TCP 状态标志号包括：SYN（同步）、ACK（应答）、FIN（结束）、RST（重设）、URG（紧急）PSH（强迫推送） 等均可使用于参数中，除此之外还可以使用关键词 ALL 和 NONE 进行比对。比对标志号时，可以使用 ! 运算子行反向比对。

9、参数 --syn

```bash
iptables -p tcp --syn
```

说明：用来比对是否为要求联机之TCP 封包，与 iptables -p tcp --tcp-flags SYN,FIN,ACK SYN 的作用完全相同，如果使用 !运算子，可用来比对非要求联机封包。

10、参数 -m multiport --source-port

```bash
iptables -A INPUT -p tcp -m multiport --source-port 22,53,80,110 -j ACCEPT
```

说明 用来比对不连续的多个来源端口号，一次最多可以比对 15 个端口，可以使用 ! 运算子进行反向比对。

11、参数 -m multiport --destination-port

```bash
iptables -A INPUT -p tcp -m multiport --destination-port 22,53,80,110 -j ACCEPT
```

说明：用来比对不连续的多个目的地端口号，设定方式同上。

12、参数 -m multiport --port

```bash
iptables -A INPUT -p tcp -m multiport --port 22,53,80,110 -j ACCEPT
```

说明：这个参数比较特殊，用来比对来源端口号和目的端口号相同的数据包，设定方式同上。注意：在本范例中，如果来源端口号为 80，目的地端口号为 110，这种数据包并不算符合条件。

13、参数 --icmp-type

```bash
iptables -A INPUT -p icmp --icmp-type 8 -j DROP
```

说明：用来比对 ICMP 的类型编号，可以使用代码或数字编号来进行比对。请打 iptables -p icmp --help 来查看有哪些代码可用。这里是指禁止ping如，但是可以从该主机ping出。

14、参数 -m limit --limit

```bash
iptables -A INPUT -m limit --limit 3/hour
```

说明：用来比对某段时间内数据包的平均流量，上面的例子是用来比对：每小时平均流量是否超过一次3个数据包。 除了每小时平均次外，也可以每秒钟、每分钟或每天平均一次，默认值为每小时平均一次，参数如后： /second、 /minute、/day。 除了进行数据包数量的比对外，设定这个参数也会在条件达成时，暂停数据包的比对动作，以避免因洪水攻击法，导致服务被阻断。

15、参数 --limit-burst

```bash
iptables -A INPUT -m limit --limit-burst 5
```

说明：用来比对瞬间大量封包的数量，上面的例子是用来比对一次同时涌入的封包是否超过 5 个（这是默认值），超过此上限的封将被直接丢弃。使用效果同上。

16、参数 -m mac --mac-source

```bash
iptables -A INPUT -m mac --mac-source 00:00:00:00:00:01 -j ACCEPT
```

说明：用来比对数据包来源网络接口的硬件地址，这个参数不能用在 OUTPUT 和 Postrouting 规则链上，这是因为封包要送出到网后，才能由网卡驱动程序透过 ARP 通讯协议查出目的地的 MAC 地址，所以 iptables 在进行封包比对时，并不知道封包会送到个网络接口去。linux基础

17、参数 --mark

```bash
iptables -t mangle -A INPUT -m mark --mark 1
```

说明：用来比对封包是否被表示某个号码，当封包被比对成功时，我们可以透过 MARK 处理动作，将该封包标示一个号码，号码最不可以超过 4294967296。linux基础

18、参数 -m owner --uid-owner

```bash
iptables -A OUTPUT -m owner --uid-owner 500
```

说明：用来比对来自本机的封包，是否为某特定使用者所产生的，这样可以避免服务器使用 root 或其它身分将敏感数据传送出，可以降低系统被骇的损失。可惜这个功能无法比对出来自其它主机的封包。

19、参数 -m owner --gid-owner

```bash
iptables -A OUTPUT -m owner --gid-owner 0
```

说明：用来比对来自本机的数据包，是否为某特定使用者群组所产生的，使用时机同上。

20、参数 -m owner --pid-owner

```bash
iptables -A OUTPUT -m owner --pid-owner 78
```

说明：用来比对来自本机的数据包，是否为某特定行程所产生的，使用时机同上。

21、参数 -m owner --sid-owner

```bash
iptables -A OUTPUT -m owner --sid-owner 100
```

说明： 用来比对来自本机的数据包，是否为某特定联机（Session ID）的响应封包，使用时机同上。

22、参数 -m state --state

```bash
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
```

说明 用来比对联机状态，联机状态共有四种：INVALID、ESTABLISHED、NEW 和 RELATED。

23、iptables -L -n -v 可以查看计数器

INVALID 表示该数据包的联机编号（Session ID）无法辨识或编号不正确。ESTABLISHED 表示该数据包属于某个已经建立的联机。NEW 表示该数据包想要起始一个联机（重设联机或将联机重导向）。RELATED 表示该数据包是属于某个已经建立的联机，所建立的新联机。例如：FTP-DATA 联机必定是源自某个 FTP 联机。
