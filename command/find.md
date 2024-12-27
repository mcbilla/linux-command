find
===

在指定目录下查找文件和目录

## 补充说明
find命令是Linux系统中的一个非常强大和灵活的命令，它可以根据各种条件在指定的目录下搜索文件和目录，不仅仅是按照文件名。例如，它可以搜索空文件、可执行文件、特定用户拥有的文件等。它还可以按照访问或修改时间、正则表达式等方式列出文件。它默认是递归的，也就是说它会搜索当前目录及其所有子目录。它还可以处理一些特殊的文件类型，如符号链接、命名管道、套接字等。所有这些都非常有用。find命令真的很强大。但是，我们还可以利用它的强大功能，将搜索的结果传递给其他命令进行进一步的处理。这样我们就可以对find命令发现的文件和目录做一些操作。

## 适用的Linux版本

find命令是GNU项目的一部分，因此它在大多数Linux发行版中都是可用的。如果你使用的是一个没有预装find命令的Linux系统，你可以使用以下命令来安装它：

- 对于基于Debian或Ubuntu的系统，使用apt-get命令：

```bash
$ sudo apt-get install findutils
```

- 对于基于Red Hat或CentOS的系统，使用yum或dnf命令：

```bash
$ sudo yum install findutils
```

或者

```bash
$ sudo dnf install findutils
```

- 对于基于Arch Linux的系统，使用pacman命令：

```bash
$ sudo pacman -S findutils
```

## 命令语法
```shell
find [path] [options] [expression]
```

* path 是要搜索的目录路径，可以是一个或多个目录或文件名，多个路径之间用空格分隔。如果未指定路径，则默认为当前目录（.）。
* options 是指定搜索类型的选项，比如按照文件名、文件类型、文件大小等进行过滤。这些选项有很多，我们会在后面介绍一些常用的。
* expression 是指定搜索条件的表达式，可以是一个或多个测试、操作或逻辑运算符组成的复合表达式。如果未指定表达式，则默认为-print，即打印出所有匹配的文件名。

## 选项

- -name pattern ：按照文件名进行搜索，支持使用通配符*和?。
- -iname pattern ：与-name类似，但是忽略大小写。
- -type type ：按照文件类型进行搜索，可以是以下字符之一：
  - f ：普通文件。
  - d ：目录。
  - l ：符号链接。
  - c ：字符设备。
  - b ：块设备。
  - p ：命名管道（FIFO）。
  - s ：套接字。
- -size [+-]size[cwbkMG] ：按照文件大小进行搜索，支持使用+或-表示大于或小于指定大小，单位可以是以下字符之一：
  - c ：字节（默认单位）。
  - w ：双字节（16位）。
  - b ：块（512字节）。
  - k ：千字节（1024字节）。
  - M ：兆字节（1024*1024字节）。
  - G ：吉字节（1024*1024*1024字节）。
- -mtime n ：按照修改时间进行搜索，n是一个整数表示天数。支持使用+或-表示在n天前或后修改的文件，或者使用n表示正好在n天前修改的文件。
- -atime n ：按照访问时间进行搜索，n是一个整数表示天数。支持使用+或-表示在n天前或后访问的文件，或者使用n表示正好在n天前访问的文件。
- -ctime n ：按照状态改变时间进行搜索，n是一个整数表示天数。支持使用+或-表示在n天前或后状态改变的文件，或者使用n表示正好在n天前状态改变的文件。
- -user name ：按照文件所有者进行搜索，name是一个用户名或用户ID。
- -group name ：按照文件所属组进行搜索，name是一个组名或组ID。
- -perm mode ：按照文件权限进行搜索，mode是一个八进制或符号表示的权限模式。支持使用+或-表示包含或匹配指定的权限位。

## 表达式

- -exec command {} ; ：对搜索到的每个文件执行command命令，其中{}代表文件名，;代表命令结束符。注意{}和;之间要有空格。
- -exec command {} + ：类似于-exec command {} ;，但是会将多个文件名作为command的参数一次执行，而不是每个文件执行一次command。
- -ok command {} ; ：类似于-exec command {} ;，但是会在执行command之前询问用户是否确认。
- -print ：打印出搜索到的文件名，这是默认的操作，通常可以省略。
- -print0 ：类似于-print，但是使用空字符（\0）而不是换行符（\n）分隔文件名。这样可以避免文件名中包含空格或特殊字符时造成的问题。

## 示例
### 根据文件名查找
列出当前目录及子目录下所有文件和文件夹
```shell
$ find .
```
在/home目录下查找以.txt结尾的文件名
```shell
$ find /home -name "*.txt"
```
同上，但忽略大小写
```shell
$ find /home -iname "*.txt"
```
当前目录及子目录下查找所有以.txt和.pdf结尾的文件
```shell
$ find . \( -name "*.txt" -o -name "*.pdf" \)
或
$ find . -name "*.txt" -o -name "*.pdf"
```
基于正则表达式匹配文件路径
```shell
$ find . -regex ".*\(\.txt\|\.pdf\)$"
```
同上，但忽略大小写
```shell
$ find . -iregex ".*\(\.txt\|\.pdf\)$"
```

### 根据文件类型查找

查找目前目录及所有子目录的一般文件
```shell
$ find . -type f
```
向下最大深度限制为3
```shell
$ find . -maxdepth 3 -type f
```
搜索出深度距离当前目录至少2个子目录的所有文件
```shell
$ find . -mindepth 2 -type f
```

### 根据时间戳查找
UNIX/Linux文件系统每个文件都有三种时间戳：
* 访问时间 （-atime/天，-amin/分钟）：用户最近一次访问时间。
* 修改时间 （-mtime/天，-mmin/分钟）：文件最后一次修改时间。
* 变化时间 （-ctime/天，-cmin/分钟）：文件数据元（例如权限等）最后一次修改时间。

搜索最近七天内（不包括）被访问过的所有文件
```shell
$ find . -type f -atime -7
```
搜索恰好在七天前那一天被访问过的所有文件
```shell
$ find . -type f -atime 7
```
搜索七天前（不包括）被访问过的所有文件
```shell
$ find . -type f -atime +7
```
搜索访问时间超过10分钟的所有文件
```shell
$ find . -type f -amin +10
```
找出比file.log修改时间更长的所有文件
```shell
$ find . -type f -newer file.log
```

### 根据文件大小进行匹配
搜索大于10KB的文件
```shell
$ find . -type f -size +10k
```
搜索小于10KB的文件
```shell
$ find . -type f -size -10k
```
搜索等于10KB的文件
```shell
$ find . -type f -size 10k
```

### 反向查找

找出/home下不是以.txt结尾的文件

```shell
$ find /home ! -name "*.txt"
```

### 删除匹配文件

删除当前目录下所有.txt文件
```shell
$ find . -type f -name "*.txt" -delete
```

### 根据文件权限/所有权进行查找
当前目录下搜索出权限为777的文件
```shell
$ find . -type f -perm 777
```
找出当前目录下权限不是644的php文件
```shell
$ find . -type f -name "*.php" ! -perm 644
```
找出当前目录用户tom拥有的所有文件
```shell
$ find . -type f -user tom
```
找出当前目录用户组sunk拥有的所有文件
```shell
$ find . -type f -group sunk
```

### 与其他命令结合使用
找出当前目录下所有root的文件，并把所有权更改为用户tom
```shell
$ find .-type f -user root -exec chown tom {} \;
```
找出自己家目录下所有的.txt文件并删除，-ok 和 -exec 行为一样，不过它会给出提示，是否执行相应的操作。
```shell
find $HOME/. -name "*.txt" -ok rm {} \;
```
因为单行命令中-exec参数中无法使用多个命令，以下方法可以实现在-exec之后接受多条命令
```shell
-exec ./text.sh {} \;
```

### 搜索但跳过指定目录
查找当前目录或者子目录下所有.txt文件，但是跳过子目录sk
```shell
# ./sk 不能写成 ./sk/ ，否则没有作用。
find . -path "./sk" -prune -o -name "*.txt" -print
```
忽略两个目录
```shell
# 如果写相对路径必须加上./
$ find . \( -path ./sk -o  -path ./st \) -prune -o -name "*.txt" -print
```

### 其他常用
找到七天前修改过的文件类型为jpeg或jpg的文件
```shell
$ find ~ \( -iname '*jpeg' -o -iname '*jpg' \) -type f -mtime -7
```
删除 mac 下自动生成的文件
```shell
$ find ./ -name '__MACOSX' -depth -exec rm -rf {} \;
```
统计代码行数
```shell
$ find . -name "*.java"|xargs cat|grep -v ^$|wc -l # 代码行数统计, 排除空行
```