yum
===

基于RPM的软件包管理器

## 补充说明

**yum命令** 是在Fedora、RedHat、CentOS、SUSE中基于rpm的软件包管理器，它可以使系统管理人员交互和自动化地更新与管理RPM软件包，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

yum是基于python2开发的，所以安装yum前需要先安装python2环境，且不能升级到python3。系统如果升级到python3环境可能会导致整个yum不能工作。

### RPM包是什么

RPM 是 RedHat Package Manage 的简写，是一种由红帽公司开发的软件包管理方式，在 Linux 中通过以 .rmp 为扩展名的文件对应用程序包进行管理。RPM 文件带有 .rpm 扩展名，RPM 程序包由一个存档文件组成，该文件包含特定程序包的库和依赖项。使用 RPM 我们可以方便的进行软件的安装、查询、卸载、升级、校验等工作。
RPM和其他低级包管理器（如Debian的dpkg和Slackware的pkgtools）一样，不能自动解决依赖缺失的问题，如果安装的过程中缺少依赖项，需要自己手动搜索RPM依赖包并下载。

RPM 二进制包命名的一般格式如下：

```
报名-版本号-发布次数-发行商-Linux平台-适合的硬件平台-包扩展名
```

例如，RPM 包的名称是`httpd-2.2.15-15.el6.centos.1.i686.rpm`，其中：

* httped：软件包名。这里需要注意，httped 是包名，而 httpd-2.2.15-15.el6.centos.1.i686.rpm 通常称为包全名，包名和包全名是不同的，在某些 Linux 命令中，有些命令（如包的安装和升级）使用的是包全名，而有些命令（包的查询和卸载）使用的是包名，一不小心就会弄错。
* 2.2.15：包的版本号，版本号的格式通常为主版本号.次版本号.修正号。
* 15：二进制包发布的次数，表示此 RPM 包是第几次编程生成的。
* el*：软件发行商，el6 表示此包是由 Red Hat 公司发布，适合在 RHEL 6.x (Red Hat Enterprise Unux) 和 CentOS 6.x 上使用。
* centos：表示此包适用于 CentOS 系统。
* i686：表示此包使用的硬件平台，目前的 RPM 包支持的平台如下所示：

| 平台名称 | 适用平台信息                                                 |
| -------- | ------------------------------------------------------------ |
| i386     | 386 以上的计算机都可以安装                                   |
| i586     | 686 以上的计算机都可以安装                                   |
| i686     | 奔腾 II 以上的计算机都可以安装，目前所有的 CPU 是奔腾 II 以上的，所以这个软件版本居多 |
| x86_64   | 64 位 CPU 可以安装                                           |
| noarch   | 没有硬件限制                                                 |

- rpm：RPM 包的扩展名，表明这是编译好的二进制包，可以使用 rpm 命令直接安装。此外，还有以 src.rpm 作为扩展名的 RPM 包，这表明是源代码包，需要安装生成源码，然后对其编译并生成 rpm 格式的包，最后才能使用 rpm 命令进行安装。

### RPM包默认安装路径

通常情况下，RPM 包采用系统默认的安装路径，所有安装文件会按照类别分散安装到下图所示的目录中。

| 安装路径        | 含 义                      |
| --------------- | -------------------------- |
| /etc/           | 配置文件安装目录           |
| /usr/bin/       | 可执行的命令安装目录       |
| /usr/lib/       | 程序所使用的函数库保存位置 |
| /usr/share/doc/ | 基本的软件使用手册保存位置 |
| /usr/share/man/ | 帮助文件保存位置           |

RPM 包的默认安装路径是可以通过命令查询的。

除此之外，RPM 包也支持手动指定安装路径，但此方式并不推荐。因为一旦手动指定安装路径，所有的安装文件会集中安装到指定位置，且系统中用来查询安装路径的命令也无法使用（需要进行手工配置才能被系统识别），得不偿失。

## 命令语法

```shell
yum/dnf [options] [command] [package ...]
```

## 选项

```shell
-h：显示帮助信息；
-y：对所有的提问都回答“yes”；
-c：指定配置文件；
-q：安静模式；
-v：详细模式；
-d：设置调试等级（0-10）；
-e：设置错误等级（0-10）；
-R：设置yum处理一个命令的最大等待时间；
-C：完全从缓存中运行，而不去下载或者更新任何头文件。
```

## 命令

```shell
install：安装rpm软件包；
update：更新rpm软件包；
check-update：检查是否有可用的更新rpm软件包；
remove：删除指定的rpm软件包；
list：显示软件包的信息；
search：检查软件包的信息；
info：显示指定的rpm软件包的描述信息和概要信息；
clean：清理yum过期的缓存；
shell：进入yum的shell提示符；
resolvedep：显示rpm软件包的依赖关系；
localinstall：安装本地的rpm软件包；
localupdate：显示本地rpm软件包进行更新；
deplist：显示rpm软件包的所有依赖关系；
provides：查询某个程序所在安装包。
```

## 示例

部分常用的命令包括：

*   自动搜索最快镜像插件：`yum install yum-fastestmirror`
*   安装yum图形窗口插件：`yum install yumex`
*   查看可能批量安装的列表：`yum grouplist`

### 安装

```shell
yum install              #全部安装
yum install package1     #安装指定的安装包package1
yum groupinsall group1   #安装程序组group1
```

### 更新和升级

```shell
yum update               #全部更新
yum update package1      #更新指定程序包package1
yum check-update         #检查可更新的程序
yum upgrade package1     #升级指定程序包package1
yum groupupdate group1   #升级程序组group1
```

### 查找和显示

```shell
# 检查 MySQL 是否已安装
yum list installed | grep mysql
yum list installed mysql*

yum info package1      #显示安装包信息package1
yum list               #显示所有已经安装和可以安装的程序包
yum list package1      #显示指定程序包安装情况package1
yum groupinfo group1   #显示程序组group1信息yum search string 根据关键字string查找安装包
```

### 删除程序

```shell
yum remove &#124; erase package1   #删除程序包package1
yum groupremove group1             #删除程序组group1
yum deplist package1               #查看程序package1依赖情况
```

### 清除缓存

```shell
yum clean packages       # 清除缓存目录下的软件包
yum clean headers        # 清除缓存目录下的 headers
yum clean oldheaders     # 清除缓存目录下旧的 headers
```

### 更多实例

```shell
# yum
/etc/yum.repos.d/       # yum 源配置文件
vi /etc/yum.repos.d/nginx.repo # 举个栗子: nginx yum源
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/6/$basearch/
gpgcheck=0
enabled=1

# yum mirror
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
wget https://mirror.tuna.tsinghua.edu.cn/help/centos/
yum makecache

# 添加中文语言支持
LANG=C # 原始语言
LANG=zh_CN.utf8 # 切换到中文
yum groupinstall "Chinese Support" # 添加中文语言支持
```



