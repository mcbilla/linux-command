<p align="center">
  <a href="https://linux.mcbilla.com/">
    <img src="./template/img/banner.svg?sanitize=true">
  </a>
  <h1>Linux Command</h1>
</p>

## Linux命令分类

**这里存放Linux 命令大全并不全，你可以通过[linux-command](https://linux.mcbilla.com/)来搜索，它是把 [command](./assets/command) 目录里面搜集的命令，生成了静态HTML并提供预览以及索引搜索。**

## 常用命令

### 网络

- [curl](https://linux.mcbilla.com/c/curl.html)：利用URL规则在命令行下工作的文件传输工具
- [netstat](https://linux.mcbilla.com/c/netstat.html)：查看网络系统状态信息
- [ss](https://linux.mcbilla.com/c/ss.html)：比 netstat 更高效好用的 socket 统计信息工具
- [nc](https://linux.mcbilla.com/c/nc.html)：网络界的瑞士军刀，号称最强大的网络工具
- [ifconfig](https://linux.mcbilla.com/c/ifconfig.html)：配置和显示网络接口
- [ip](https://linux.mcbilla.com/c/ip.html)：管理网络接口和路由，用于替代ifconfig、route等命令
- [tcpdump](https://linux.mcbilla.com/c/tcpdump.html)：抓包工具，用于捕获和分析网络流量
- [iptables](https://linux.mcbilla.com/c/iptables.html)：Linux上常用的防火墙软件，监控和过滤从服务器进出的网络数据包

### 磁盘和内存

- [vmstat](https://linux.mcbilla.com/c/vmstat.html)：监测系统的虚拟内存、进程、CPU活动、块设备I/O等信息
- [free](https://linux.mcbilla.com/c/free.html)：显示物理内存和交换空间的使用情况
- [df](https://linux.mcbilla.com/c/df.html)：显示磁盘的空间使用情况
- [du](https://linux.mcbilla.com/c/du.html)：显示每个文件和目录的磁盘使用空间
- [tree](https://linux.mcbilla.com/c/tree.html)：树状图列出目录的内容
- [mount](https://linux.mcbilla.com/c/mount.html)：用于挂载Linux系统外的文件
- [fsck](https://linux.mcbilla.com/c/fsck.html)：检查并且试图修复文件系统中的错误

### 文本

grep 、sed、awk 被称为 Linux 中的文本三剑客。

- [grep](https://linux.mcbilla.com/c/grep.html)：适合单纯的查找或匹配文本
- [sed](https://linux.mcbilla.com/c/sed.html)：适合编辑匹配到的文本
- [awk](https://linux.mcbilla.com/c/awk.html)：适合格式化文本，对文本进行较复杂格式处理

其他常用命令：

- [more](https://linux.mcbilla.com/c/more.html)：显示文件内容，每次显示一屏
- [sort](https://linux.mcbilla.com/c/sort.html)：对文本文件中所有行进行排序
- [uniq](https://linux.mcbilla.com/c/uniq.html)：检查和删除文本文件中的重复行
- [wc](https://linux.mcbilla.com/c/wc.html)：统计文件的字节数、字数、行数
- [head](https://linux.mcbilla.com/c/head.html)：显示文件的开头部分
- [tail](https://linux.mcbilla.com/c/tail.html)：在屏幕上显示指定文件的末尾若干行
- [cat](https://linux.mcbilla.com/c/cat.html)：连接多个文件并打印到标准输出
- [more](https://linux.mcbilla.com/c/more.html)：显示文件内容，每次显示一屏
- [less](https://linux.mcbilla.com/c/less.html)：分屏上下翻页浏览文件内容

### 系统

- [top](https://linux.mcbilla.com/c/top.html)：显示系统的运行状态和进程信息
- [sar](https://linux.mcbilla.com/c/sar.html)：系统运行状态统计工具
- [pstack](https://linux.mcbilla.com/c/pstack.html)：分析进程调用栈
- [htop](https://linux.mcbilla.com/c/htop.html)：一个交互式的进程查看器，可以动态观察系统进程状况
- [ps](https://linux.mcbilla.com/c/ps.html)：报告当前系统的进程状态
- [alias](https://linux.mcbilla.com/c/alias.html)：定义或显示别名

### 用户

- [chmod](https://linux.mcbilla.com/c/chmod.html)：用来变更文件或目录的权限
- [sudo](https://linux.mcbilla.com/c/sudo.html)：以其他身份来执行命令

### 文件

- [mv](https://linux.mcbilla.com/c/mv.html)：对文件或目录重命名或者移动
- [cp](https://linux.mcbilla.com/c/cp.html)：将源文件或目录复制到目标文件或目录中
- [rm](https://linux.mcbilla.com/c/rm.html)：删除指定的文件和目录
- [find](https://linux.mcbilla.com/c/find.html)：在指定目录下查找文件和目录
- [touch](https://linux.mcbilla.com/c/touch.html)：创建新的空文件和更新文件时间戳
- [ls](https://linux.mcbilla.com/c/ls.html)：显示目录内容列表
- [lsof](https://linux.mcbilla.com/c/lsof.html)：显示Linux中的文件打开情况
- [ln](https://linux.mcbilla.com/c/ln.html)：为文件创建链接
- [tar](https://linux.mcbilla.com/c/tar.html)：打包、压缩和解压文件
- [zip](https://linux.mcbilla.com/c/zip.html)：压缩文件并生成zip文件
- [unzip](https://linux.mcbilla.com/c/unzip.html)：解压缩zip文件
- [rar](https://linux.mcbilla.com/c/rar.html)：压缩文件并生成rar文件
- [unrar](https://linux.mcbilla.com/c/unrar.html)：解压缩rar文件

### 包管理

- [pip](https://linux.mcbilla.com/c/pip.html)：Python 编程语言中的包管理器，用于安装和管理第三方 Python 模块
- [apt](https://linux.mcbilla.com/c/apt.html)：Debian Linux发行版中的APT软件包管理工具
- [yum](https://linux.mcbilla.com/c/yum.html)：基于RPM的软件包管理器
- [dnf](https://linux.mcbilla.com/c/dnf.html)：新一代的RPM软件包管理器

### 其他

- [xargs](https://linux.mcbilla.com/c/xargs.html)：给其他命令传递参数的一个过滤器
- [nohup](https://linux.mcbilla.com/c/nohup.html)：将程序以忽略挂起信号的方式运行起来
- [管道符 `|`](https://linux.mcbilla.com/c/管道符.html) ：把前一个命令原本要输出到屏幕的信息当做后一个命令的标准输入。
- [重定向 `>`](https://linux.mcbilla.com/c/重定向.html) ：输入/输出重定向
- [crontab](https://linux.mcbilla.com/c/crontab.html)：提交和管理用户的需要周期性执行的任务

## Linux学习资源整理


### 社区网站

- [Linux中国](https://linux.cn/) - 各种资讯、文章、技术
- [实验楼](https://www.shiyanlou.com/) - 免费提供了Linux在线环境，不用在自己机子上装系统也可以学习Linux，超方便实用。
- [鸟哥的linux私房菜](http://linux.vbird.org/) - 非常适合Linux入门初学者看的教程。
- [Linux公社](http://www.linuxidc.com/) - Linux相关的新闻、教程、主题、壁纸都有。
- [Linux Today](http://www.linuxde.net) - Linux新闻资讯发布，Linux职业技术学习！。
- [X-CMD](https://www.x-cmd.com/) - Shell + AWK 为核心增强原生命令输出以及交互体验，各种命令以及现代化软件包的介绍和使用教程，每日科技新闻资讯，欢迎浏览关注！

### 知识相关

- [Linux思维导图整理](http://www.jianshu.com/p/59f759207862)
- [Linux初学者进阶学习资源整理](http://www.jianshu.com/p/fe2a790b41eb)
- [Linux 基础入门（新版）](https://www.shiyanlou.com/courses/1)
- [【译】Linux概念架构的理解](http://www.jianshu.com/p/c5ae8f061cfe) [En](http://oss.org.cn/ossdocs/linux/kernel/a1/index.html)
- [Linux 守护进程的启动方法](http://www.ruanyifeng.com/blog/2016/02/linux-daemon.html)
- [Linux编程之内存映射](https://www.shiyanlou.com/questions/2992)
- [Linux知识点小结](https://blog.huachao.me/2016/1/Linux%E7%9F%A5%E8%AF%86%E7%82%B9%E5%B0%8F%E7%BB%93/)
- [10大白帽黑客专用的 Linux 操作系统](https://linux.cn/article-6971-1.html)

### 软件工具

- [超赞的Linux软件](https://www.gitbook.com/book/alim0x/awesome-linux-software-zh_cn/details) Github仓库[Zh](https://github.com/alim0x/Awesome-Linux-Software-zh_CN) [En](https://github.com/VoLuong/Awesome-Linux-Software)

Adobe软件的最佳替代品 [原文在这里](https://linux.cn/article-8928-1.html)

- [Evince (Adobe Acrobat Reader)](https://wiki.gnome.org/Apps/Evince) 一个“支持多种文档格式的文档查看器”，可以查看PDF，还支持各种漫画书格式
- [Pixlr (Adobe Photoshop)](https://pixlr.com/) 一个强大的图像编辑工具
- [Inkscape (Adobe Illustrator)](https://inkscape.org/zh/) 一个专业的矢量图形编辑器
- [Pinegrow Web Editor (Adobe Dreamweaver)](https://pinegrow.com/) 一个可视化编辑制作 HTML 网站
- [Scribus (Adobe InDesign)](https://www.scribus.net/) 一个开源电子杂志制作软件
- [Webflow (Adobe Muse)](https://webflow.com/) 一款可以帮助用户不用编码就可以快速创建网站的谷歌浏览器插件。
- [Tupi (Adobe Animate)](http://www.maefloresta.com/portal/) 一款可以创建HTML5动画的工具。
- [Black Magic Fusion (Adobe After Effects)](https://www.blackmagicdesign.com) 一款先进的合成软件，广泛应用于视觉特效、广电影视设计以及3D动画设计等领域。

