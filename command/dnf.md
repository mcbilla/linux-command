dnf
===

新一代的RPM软件包管理器

## 补充说明

**DNF** 是新一代的rpm软件包管理器。他首先出现在 Fedora 18 这个发行版中。而最近，它取代了yum，正式成为 Fedora 22 的包管理器。

DNF包管理器克服了YUM包管理器的一些瓶颈，提升了包括用户体验，内存占用，依赖分析，运行速度等多方面的内容。DNF使用 RPM, libsolv 和 hawkey 库进行包管理操作。尽管它没有预装在 CentOS 和 RHEL 7 中，但你可以在使用 YUM 的同时使用 DNF 。

DNF 的最新稳定发行版版本号是 1.0，发行日期是2015年5月11日。 这一版本的 DNF 包管理器（包括在他之前的所有版本） 都大部分采用 Python 编写，发行许可为GPL v2.

##  安装 DNF 包管理器

DNF 并未默认安装在 RHEL 或 CentOS 7系统中，但是 Fedora 22 已经默认使用 DNF .

1、为了安装 DNF ，您必须先安装并启用 epel-release 依赖。

```shell
yum install -y epel-release
```

2、使用 epel-release 依赖中的 YUM 命令来安装 DNF 包。在系统中执行以下命令：

```shell
yum install -y dnf
```

然后， DNF 包管理器就被成功的安装到你的系统中了。

## 命令语法

```shell
dnf [options] COMMAND
```

## 选项

- -c [config file], --config [config file] 配置文件位置
- -q, --quiet 静默执行
- -v, --verbose 详尽执行
- --version 显示 DNF 版本并推出
- --installroot [path] 设置目标根目录
- --nodocs 不要安装文档
- --noplugins 禁用所有插件
- --enableplugin [plugin] 启用指定名称的插件
- --disableplugin [plugin] 禁用指定名称的插件
- --releasever RELEASEVER 覆盖在配置文件和仓库文件中 $releasever 的值
- --setopt SETOPTS 设置任意配置和仓库选项
- --skip-broken 通过跳过软件包来解决依赖问题
- -h, --help, --help-cmd 显示命令帮助
- --allowerasing 允许解决依赖关系时删除已安装软件包
- -b, --best 在事务中尝试最佳软件包版本。
- --nobest 不用把事务限制在最佳选择
- -C, --cacheonly 完全从系统缓存运行，不升级缓存
- -R [minutes], --randomwait [minutes] 最大命令等待时间
- -d [debug level], --debuglevel [debug level] 调试输出级别
- --debugsolver 转储详细解决结果至文件
- --showduplicates 在 list/search 命令下，显示仓库里重复的条目
- -e ERRORLEVEL, --errorlevel ERRORLEVEL 错误输出级别
- --obsoletes 对升级启用 dnf 的过期处理逻辑，或对 info、list 和 repoquery 显示软件包过期的功能
- --rpmverbosity [debug level name] rpm调试输出等级
- -y, --assumeyes 全部问题自动应答为是
- --assumeno 全部问题自动应答为否
- --enablerepo [repo] 启用其他存储库。列出选项。支持 glob，可以多次指定。
- --disablerepo [repo] 禁用存储库。列出选项。支持 glob，可以多次指定。
- --repo [repo], --repoid [repo] 启用指定 id 或 glob 的仓库，可以指定多次
- --enable 使用 config-manager 命令启用 repos (自动保存)
- --disable 使用 config-manager 命令禁用 repos (自动保存)
- -x [package], --exclude [package], --excludepkgs [package] 用全名或通配符排除软件包
- --disableexcludes [repo], --disableexcludepkgs [repo] 禁用 excludepkgs
- --repofrompath [repo,path] 要使用的附加存储库的标签和路径（与 baseurl 中相同的路径），可以多次指定。
- --noautoremove 禁用删除不再被使用的依赖软件包
- --nogpgcheck 禁用 gpg 签名检查 (如果 RPM 策略允许)
- --color COLOR 配置是否使用颜色
- --refresh 在运行命令之前将元数据标记为过期。
- -4 仅解析 IPv4 地址
- -6 仅解析 IPv6 地址
- --destdir DESTDIR, --downloaddir DESTDIR 设置软件包要复制到的目录
- --downloadonly 仅下载软件包
- --comment COMMENT 为事务添加一个注释
- --bugfix 在更新中包括与 bug 修复有关的软件包
- --enhancement 在更新中包括与功能增强有关的软件包。
- --newpackage 在更新中包括与新软件包有关的软件包
- --security 在更新中包括与安全有关的软件包
- --advisory ADVISORY, --advisories ADVISORY 在更新中包括修复指定公告所必须的软件包
- --bz BUGZILLA, --bzs BUGZILLA 在更新中包括修复给定 BZ 所必须的软件包
- --cve CVES, --cves CVES 在更新中包括修复给定 CVE 所必须的软件包
- --sec-severity {Critical,Important,Moderate,Low}, --secseverity {Critical,Important,Moderate,Low} 在更新中包括匹配给定安全等级的安全相关的软件包
- --forcearch ARCH 强制使用一个架构 

## 命令

#### 主要命令列表

- alias 列出或新建命令别名
- autoremove 删除所有原先因为依赖关系安装的不需要的软件包
- check 在包数据库中寻找问题
- check-update 检查是否有软件包升级
- clean 删除已缓存的数据
- deplist 列出软件包的依赖关系和提供这些软件包的源
- distro-sync 同步已经安装的软件包到最新可用版本
- downgrade 降级包
- group 显示或使用组信息
- help 显示一个有帮助的用法信息
- history 显示或使用事务历史
- info 显示关于软件包或软件包组的详细信息
- install 向系统中安装一个或多个软件包
- list 列出一个或一组软件包
- makecache 创建元数据缓存
- mark 在已安装的软件包中标记或者取消标记由用户安装的软件包。
- module 与模块交互。
- provides 查找提供指定内容的软件包
- reinstall 重装一个包
- remove 从系统中移除一个或多个软件包
- repolist 显示已配置的软件仓库
- repoquery 搜索匹配关键字的软件包
- repository-packages 对指定仓库中的所有软件包运行命令
- search 在软件包详细信息中搜索指定字符串
- shell 运行一个非交互式的 DNF shell
- swap 运行交互式的 DNF mod 以删除或安装 spec
- updateinfo 显示软件包的参考建议
- upgrade 升级系统中的一个或多个软件包
- upgrade-minimal 升级，但只有“最新”的软件包已修复可能影响你的系统的问题

#### 插件命令列表

- builddep Install build dependencies for package or spec file
- changelog 查看软件包的改变日志数据
- config-manager 管理 dnf 配置选项和软件仓库
- copr 与 Copr 仓库交互
- debug-dump 转储已安装的 RPM 软件包信息至文件
- debug-restore 恢复调试用转储文件中的软件包记录
- debuginfo-install 安装调试信息软件包
- download 下载软件包至当前目录
- needs-restarting 判断所升级的二进制文件是否需要重启
- playground 与 Playground 仓库交互。
- repoclosure 显示仓库中未被解决的依赖关系的列表
- repodiff 列出两组仓库中的不同
- repograph 以点线图方式输出完整的软件包依赖关系图
- repomanage 管理 RPM 软件包目录
- reposync 下载远程仓库中的全部软件包

## 示例

### 查看 DNF 包管理器版本

用处：该命令用于查看安装在您系统中的 DNF 包管理器的版本

```shell
dnf -–version
```

### 查看系统中可用的 DNF 软件库

用处：该命令用于显示系统中可用的 DNF 软件库

```shell
dnf repolist
```

### 查看系统中可用和不可用的所有的 DNF 软件库

用处：该命令用于显示系统中可用和不可用的所有的 DNF 软件库

```shell
dnf repolist all
```

### 列出所有 RPM 包

用处：该命令用于列出用户系统上的所有来自软件库的可用软件包和所有已经安装在系统上的软件包

```shell
dnf list
```

### 列出所有安装了的 RPM 包 

用处：该命令用于列出所有安装了的 RPM 包

```shell
dnf list installed
```

### 列出所有可供安装的 RPM 包

用处：该命令用于列出来自所有可用软件库的可供安装的软件包

```shell
dnf list available
```

### 搜索软件库中的 RPM 包

用处：当你不知道你想要安装的软件的准确名称时，你可以用该命令来搜索软件包。你需要在”search”参数后面键入软件的部分名称来搜索。（在本例中我们使用”nano”）

```shell
dnf search nano
```

### 查找某一文件的提供者

用处：当你想要查看是哪个软件包提供了系统中的某一文件时，你可以使用这条命令。（在本例中，我们将查找”/bin/bash”这个文件的提供者）

```shell
dnf provides /bin/bash
```

### 查看软件包详情

用处：当你想在安装某一个软件包之前查看它的详细信息时，这条命令可以帮到你。（在本例中，我们将查看”nano”这一软件包的详细信息）

```shell
dnf info nano
```

### 安装软件包

用处：使用该命令，系统将会自动安装对应的软件及其所需的所有依赖（在本例中，我们将用该命令安装nano软件）

```shell
dnf install nano
```

### 升级软件包

用处：该命令用于升级制定软件包（在本例中，我们将用命令升级”systemd”这一软件包）

```shell
dnf update systemd
```

### 检查系统软件包的更新

用处：该命令用于检查系统中所有软件包的更新

```shell
dnf check-update
```

### 升级所有系统软件包

用处：该命令用于升级系统中所有有可用升级的软件包

```shell
dnf update 或 dnf upgrade
```

### 删除软件包

用处：删除系统中指定的软件包（在本例中我们将使用命令删除”nano”这一软件包）

```shell
dnf remove nano 或 dnf erase nano
```

### 删除无用孤立的软件包

用处：当没有软件再依赖它们时，某一些用于解决特定软件依赖的软件包将会变得没有存在的意义，该命令就是用来自动移除这些没用的孤立软件包。

```shell
dnf autoremove
```

### 删除缓存的无用软件包 

用处：在使用 DNF 的过程中，会因为各种原因在系统中残留各种过时的文件和未完成的编译工程。我们可以使用该命令来删除这些没用的垃圾文件。

```shell
dnf clean all
```

### 获取有关某条命令的使用帮助 

用处：该命令用于获取有关某条命令的使用帮助（包括可用于该命令的参数和该命令的用途说明）（本例中我们将使用命令获取有关命令”clean”的使用帮助）

```shell
dnf help clean
```

### 查看所有的 DNF 命令及其用途

用处：该命令用于列出所有的 DNF 命令及其用途

```shell
dnf help
```

### 查看 DNF 命令的执行历史

用处：您可以使用该命令来查看您系统上 DNF 命令的执行历史。通过这个手段您可以知道在自您使用 DNF 开始有什么软件被安装和卸载。

```shell
dnf history
```

### 查看所有的软件包组

用处：该命令用于列出所有的软件包组

```shell
dnf grouplist
```

### 安装一个软件包组

用处：该命令用于安装一个软件包组（本例中，我们将用命令安装”Educational Software”这个软件包组）

```shell
dnf groupinstall ‘Educational Software’
```

### 升级一个软件包组中的软件包

用处：该命令用于升级一个软件包组中的软件包（本例中，我们将用命令升级”Educational Software”这个软件包组中的软件）

```shell
dnf groupupdate ‘Educational Software’
```

### 删除一个软件包组

用处：该命令用于删除一个软件包组（本例中，我们将用命令删除”Educational Software”这个软件包组）

```shell
dnf groupremove ‘Educational Software’
```

### 从特定的软件包库安装特定的软件

用处：该命令用于从特定的软件包库安装特定的软件（本例中我们将使用命令从软件包库 epel 中安装 phpmyadmin 软件包）

```shell
dnf –enablerepo=epel install phpmyadmin
```

### 更新软件包到最新的稳定发行版

用处：该命令可以通过所有可用的软件源将已经安装的所有软件包更新到最新的稳定发行版

```shell
dnf distro-sync
```

### 重新安装特定软件包

用处：该命令用于重新安装特定软件包（本例中，我们将使用命令重新安装”nano”这个软件包）

```shell
dnf reinstall nano
```

### 回滚某个特定软件的版本

用处：该命令用于降低特定软件包的版本（如果可能的话）（本例中，我们将使用命令降低”acpid”这个软件包的版本）

```shell
dnf downgrade acpid
```

样例输出：

```shell
Using metadata from Wed May 20 12:44:59 2015
No match for available package: acpid-2.0.19-5.el7.x86_64
Error: Nothing to do.
```

原作者注：在执行这条命令的时候， DNF 并没有按照我期望的那样降级指定的软件（“acpid”）。该问题已经上报。

##  总结

DNF 包管理器作为 YUM 包管理器的升级替代品，它能自动完成更多的操作。但在我看来，正因如此，所以 DNF 包管理器不会太受那些经验老道的 Linux 系统管理者的欢迎。举例如下：

1.  在 DNF 中没有 –skip-broken 命令，并且没有替代命令供选择。
2.  在 DNF 中没有判断哪个包提供了指定依赖的 resolvedep 命令。
3.  在 DNF 中没有用来列出某个软件依赖包的 deplist 命令。
4.  当你在 DNF 中排除了某个软件库，那么该操作将会影响到你之后所有的操作，不像在 YUM 下那样，你的排除操作只会咋升级和安装软件时才起作用。

### dnf和yum的区别

| 编号 | DNF（Dandified YUM）                                         | YUM（Yellowdog Updater, Modified）                |
| ---- | ------------------------------------------------------------ | ------------------------------------------------- |
| 1    | DNF 使用 libsolv 来解析依赖关系，由 SUSE 开发和维护          | YUM 使用公开的 API 来解析依赖关系                 |
| 2    | API 有完整的文档                                             | API 没有完整的文档                                |
| 3    | 由 C、C++、Python 编写的                                     | 只用 Python 编写                                  |
| 4    | DNF 目前在 Fedora、RHEL 8、CentOS 8、OEL 8 和 Mageia 6/7 中使用 | YUM 目前在 RHEL 6/7、CentOS 6/7、OEL 6/7 中使用   |
| 5    | DNF 支持各种扩展                                             | Yum 只支持基于 Python 的扩展                      |
| 6    | API 有良好的文档，因此很容易创建新的功能                     | 因为 API 没有正确的文档化，所以创建新功能非常困难 |
| 7    | DNF 在同步存储库的元数据时，使用的内存较少                   | 在同步存储库的元数据时，YUM 使用了过多的内存      |
| 8    | DNF 使用满足性算法来解决依赖关系解析（它是用字典的方法来存储和检索包和依赖信息） | 由于使用公开 API 的原因，Yum 依赖性解析变得迟钝   |
| 9    | 从内存使用量和版本库元数据的依赖性解析来看，性能都不错       | 总的来说，在很多因素的影响下，表现不佳            |
| 10   | DNF 更新：在 DNF 更新过程中，如果包中包含不相关的依赖，则不会更新 | YUM 将在没有验证的情况下更新软件包                |
| 11   | 如果启用的存储库没有响应，DNF 将跳过它，并继续使用可用的存储库处理事务 | 如果有存储库不可用，YUM 会立即停止                |
| 12   | dnf update 和 dnf upgrade 是等价的                           | 在 Yum 中则不同                                   |
| 13   | 安装包的依赖关系不更新                                       | Yum 为这种行为提供了一个选项                      |
| 14   | 清理删除的包：当删除一个包时，DNF 会自动删除任何没有被用户明确安装的依赖包 | Yum 不会这样做                                    |
| 15   | 存储库缓存更新计划：默认情况下，系统启动后 10 分钟后，DNF 每小时会对配置的存储库检查一次更新。这个动作由系统定时器单元 dnf-makecache.timer 控制 | Yum 也会这样做                                    |
| 16   | 内核包不受 DNF 保护。不像 Yum，你可以删除所有的内核包，包括运行中的内核包 | Yum 不允许你删除运行中的内核                      |
| 17   | libsolv：用于解包和读取资源库。hawkey: 为 libsolv 提供简化的 C 和 Python API 库。librepo: 提供 C 和 Python（类似 libcURL）API 的库，用于下载 Linux 存储库元数据和软件包。libcomps: 是 yum.comps 库的替代品。它是用纯 C 语言编写的库，有 Python 2 和 Python 3 的绑定。 | Yum 不使用单独的库来执行这些功能                  |
| 18   | DNF 包含 29000 行代码                                        | Yum 包含 56000 行代码                             |
| 19   | DNF 由 Ales Kozumplik 开发                                   | YUM 由 Zdenek Pavlas、Jan Silhan 和团队成员开发   |
