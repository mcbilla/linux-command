rar
===

压缩文件并生成rar文件

## 补充说明

压缩文件或目录为rar包。

## 适用的Linux版本

`rar`命令并非所有Linux发行版都预安装。如果在尝试执行时遇到`bash: unrar: command not found`错误，需要手动安装。

安装命令如下：

```bash
# 基于apt的发行版（如Debian、Ubuntu、Raspbian、Kali Linux等）
sudo apt-get update && sudo apt-get install rar

# 基于yum的发行版（如RedHat，CentOS 7等）
sudo yum update && sudo yum install rar

# 基于dnf的发行版（如Fedora，CentOS 8等）
sudo dnf update && sudo dnf install rar

# 基于apk的发行版（如Alpine Linux）
sudo apk add --update rar

# 基于pacman的发行版（如Arch Linux）
sudo pacman -Syu && sudo pacman -S rar

# 基于zypper的发行版（如openSUSE）
sudo zypper ref && sudo zypper in rar

# 基于pkg的FreeBSD发行版
sudo pkg update && sudo pkg install rar

# 基于brew的OS X/macOS发行版
brew update && brew install rar
```

##  命令语法

```
rar [options] targetName [sourfiles...]
```

##  选项

- a：添加到压缩文件
- c：添加压缩文件注释
- l[t,b]：列出压缩文件[技术信息,简洁]
- v[t,b]：详细列出压缩文件[技术信息,简洁]
- x：用绝对路径来解压压缩文件，意思就是按照压缩文件里的文件路径来解压文件。
- d：从压缩文档中删除文件
- e：将文件解压到当前目录
- -r：递归压缩，用于目录
- t：测试压缩文件

## 示例

### 压缩

```shell
# 压缩多个文件，压缩包名为rar1.rar
$ rar a rar1 rar.txt readme.txt

# 压缩目录
$ rar a -r a.rar a/
```

### 添加注释

```shell
$ rar c rar1.rar
```

### 显示压缩文件的详细信息

```shell
$ rar v rar1.rar
```

### 解压文件

```shell
# 从压缩文件解压 *.ttf 字体文件到当前文件夹
$ rar x Fonts *.ttf

# 从压缩文件解压 *.ttf 字体文件到文件夹 NewFont
$ rar x Fonts *.ttf NewFonts\
```

### 从压缩文档中删除文件

```shell
$ rar d test.rar file1.txt
```

### 将文件解压到当前目录

```shell
$ rar e test.rar
```
