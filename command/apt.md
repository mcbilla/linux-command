apt
===

Debian Linux发行版中的APT软件包管理工具

## 补充说明

apt是Debian及衍生系统Ubuntu下的包管理器，主要用于自动从互联网的软件仓库中搜索、安装、升级、卸载软件或操作系统，解决软件依赖关系的能力，通常使用`.deb`安装文件。

#### apt和apt-get的区别

apt-get、apt-cache 和 apt-config都是linux上常用的包管理命令。

- apt-get：用于执行和软件包安装有关的所有操作。
- apt-cache：用于查找软件包的相关信息。
- apt-rpm：处理红帽的`.rpm`文件。

可以通过设置参数进行各种精细化命令操作，但是有很多比较冷门的精细命令普通用户日常用不到。换种说法来说，就是用户最常用的 Linux 包管理命令都被分散在了 apt-get、apt-cache 和 apt-config 这三条命令当中。

apt 命令的引入就是为了解决命令过于分散的问题，它包括了 apt-get 命令出现以来使用最广泛的功能选项，以及 apt-cache 和 apt-config 命令中很少用到的功能。

在使用 apt 命令时，用户不必再由 apt-get 转到 apt-cache 或 apt-config，而且 apt 更加结构化，并为用户提供了管理软件包所需的必要选项。

简单来说就是：**apt = apt-get、apt-cache 和 apt-config 中最常用命令选项的集合**。

虽然 apt 与 apt-get 有一些类似的命令选项，但它并不能完全向下兼容 apt-get 命令。也就是说，可以用 apt 替换部分 apt-get 系列命令，但不是全部，对于低级操作，仍然需要 apt-get。命令对比：

| apt-get              | apt              | 功能                             |
| -------------------- | ---------------- | -------------------------------- |
| apt-get              | apt              | 安装软件包                       |
| apt-get remove       | apt remove       | 删除软件包                       |
| apt-get purge        | apt purge        | 移除软件包及配置文件             |
| apt-get update       | apt update       | 刷新存储库索引                   |
| apt-get upgrade      | apt upgrade      | 更新所有软件包（自动处理依赖项） |
| apt-get autoremove   | apt autoremove   | 自动删除不需要的包               |
| apt-get dist-upgrade | apt full-upgrade | 在升级软件包时自动处理依赖关系   |
| apt-cache search     | apt search       | 搜索应用程序                     |
| apt-cache show       | apt show         | 显示包细节                       |

apt 还有一些自己的命令：

| 新的apt命令      | 命令的功能                                       |
| ---------------- | ------------------------------------------------ |
| apt list         | 列出包含条件的包（已安装，可升级等）类似 dpkg -l |
| apt edit-sources | 编辑源列表                                       |

apt-get 虽然没被弃用，但作为普通用户，还是应该首先使用 apt。

##  命令语法

```shell
apt [options] [command] [pkg ...]
```

- **options：**可选，选项包括 -h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
- **command：**要进行的操作。
- **package**：安装的包名。

##  选项

- -v, --version Display information about what version of apt you are using.
- -h, --help Display a brief listing of available commands and options.
- -y Assume the answer "yes" to any prompts, proceeding with all operations if they are possible.
- --assume-no Assume the answer "no" to all prompts.
- -d, --download-only For any operation that would download packages, download them, but do nothing else.
- -f, --fix-broken When used with install or remove, this option attempts to fix any broken dependencies.
- --no-download Do not download any packages. This forces apt to use only packages it has already downloaded.
- -s, --simulate Simulate operations, reporting what they would do, but make no changes to the system.

##  命令

- update：取回可以更新的软件包列表信息，只检查不更新。update在upgrade和 full-upgrade前被执行。
- upgrade <pkg>：升级软件包。如果没有指定包名就升级所有包。
- full-upgrade：升级整个系统，必要时可以移除旧软件包。
- install <pkg>：安装软件包。pkg可以使用正则表达式/完整包名/部分包名。
- remove <pkg>：卸载指定软件包，但是会保留包的配置文件。
- purge <pkg>：在删除包的同时删除其配置文件。
- autoremove：用于删除自动安装的包，这些包是为了满足其他包的依赖关系而自动安装的，随着依赖关系的更改或需要它们的包已被删除，这些包现在不再需要了。
- search <test>：搜索软件包。
- show pkg：显示包信息，包括它的依赖关系、安装和下载大小、包的来源、包内容的描述等等。比如，在删除一个包或搜索要安装的新包之前查看这些信息是很有帮助的。
- list：显示包列表。可以通过 --installed 选项列出已安装的包，--upgrade 选项列出可以升级的包。
- edit-sources：用来编辑 /etc/apt/source.list 文件。

## 示例

### 安装包软件

```shell
# 安装软件
$ sudo apt install <package_name>

# 安装指定版本的软件
$ sudo apt install <package_name>=<version>

# 安装多个软件
$ sudo apt install <package_1> <package_2> <package_3>

# 修复依赖关系
$ sudo apt install -f <package_name>

# 安装自动跳过提示框
$ sudo apt install -y <package_name>

# 如果包存在则不要升级
sudo apt install <package_name> --no-upgrade
```

### 升级包软件

```shell
# 检查不更新
$ sudo apt update <package_name>

# 检查并更新
$ sudo apt update && sudo apt -y upgrade <package_name>
```

### 查看包软件信息

```shell
# 查看指定软件包
$ sudo apt show <package_name>

# 查看软件包列表
$ sudo apt list

# 查看已安装软件包
$ sudo apt list --installed

# 查看可升级软件包
$ sudo apt list --upgradable
```

### 搜索包软件

```shell
$ sudo apt search <package_name>
```

### 卸载包软件

```shell
# 卸载软件，保留配置文件
$ sudo apt remove <package_name>

# 同时卸载软件和配置文件
$ sudo apt purge <package_name>

# 自动清理不再使用的依赖和库文件
$ sudo apt autoremove
```
