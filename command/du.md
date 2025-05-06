du
===

显示指定文件和目录占用的磁盘空间

## 补充说明

du （disk usage）命令用于显示指定的目录或文件所占用的磁盘空间。
du命令与df命令不同，df 命令是统计文件系统整体磁盘使用情况（已使用和未使用量），du 命令是统计指定目录或文件占用磁盘的大小。

## 适用的Linux版本

du命令是一个标准的Linux命令，它适用于大多数的Linux发行版，如Ubuntu, Debian, CentOS, Fedora, RedHat等。如果某些Linux系统没有安装du命令，可以使用以下命令进行安装：

- 对于使用apt-get作为包管理工具的系统，如Ubuntu, Debian等，可以使用以下命令安装du命令：

```bash
sudo apt-get update
sudo apt-get install coreutils
```

- 对于使用yum作为包管理工具的系统，如CentOS, Fedora, RedHat等，可以使用以下命令安装du命令：

```bash
sudo yum update
sudo yum install coreutils
```

- 对于使用dnf作为包管理工具的系统，如CentOS 8等，可以使用以下命令安装du命令：

```bash
sudo dnf update
sudo dnf install coreutils
```

## 命令语法

```shell
du [选项][文件或目录]
```

**如果没有指定文件或目录，du命令会显示当前目录下各个子目录和文件所占用的磁盘空间大小，默认显示单位KB**。

## 选项

```shell
-a, --all                              显示目录中个别文件的大小。
-B, --block-size=大小                  使用指定字节数的块
-b, --bytes                            显示目录或文件大小时，以byte为单位。
-c, --total                            除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
-D, --dereference-args                 显示指定符号链接的源文件大小。
-d, --max-depth=N                      限制文件夹深度
-H, --si                               与-h参数相同，但是K，M，G是以1000为换算单位。
-h, --human-readable                   以K，M，G为单位，提高信息的可读性。
-k, --kilobytes                        以KB(1024bytes)为单位输出。
-l, --count-links                      重复计算硬件链接的文件。
-m, --megabytes                        以MB为单位输出。
-L<符号链接>, --dereference<符号链接>  显示选项中所指定符号链接的源文件大小。
-P, --no-dereference                   不跟随任何符号链接(默认)
-0, --null                             将每个空行视作0 字节而非换行符
-S, --separate-dirs                    显示个别目录的大小时，并不含其子目录的大小。
-s, --summarize                        仅显示总计，只列出最后加总的值。
-x, --one-file-xystem                  以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
-X<文件>, --exclude-from=<文件>        在<文件>指定目录或文件。
--apparent-size                        显示表面用量，而并非是磁盘用量；虽然表面用量通常会小一些，但有时它会因为稀疏文件间的"洞"、内部碎片、非直接引用的块等原因而变大。
--files0-from=F                        计算文件F中以NUL结尾的文件名对应占用的磁盘空间如果F的值是"-"，则从标准输入读入文件名
--exclude=<目录或文件>                 略过指定的目录或文件。
--max-depth=N                          显示目录总计(与--all 一起使用计算文件)当N为指定数值时计算深度为N，等于0时等同--summarize
--si                                   类似-h，但在计算时使用1000 为基底而非1024
--time                                 显示目录或该目录子目录下所有文件的最后修改时间
--time=WORD                            显示WORD时间，而非修改时间：atime，access，use，ctime 或status
--time-style=样式                      按照指定样式显示时间(样式解释规则同"date"命令)：full-iso，long-iso，iso，+FORMAT
--help                                 显示此帮助信息并退出
--version                              显示版本信息并退出
```

## 示例

### 显示当前目录下各个子目录所占用的磁盘空间大小

```shell
$ du
8       ./dir2
12      ./dir1/dir1-dira
876     ./dir1
1092    .
```

### 显示当前目录下各个子目录+文件所占用的磁盘空间大小

```shell
$ du -a
100     ./file2
8       ./dir2
100     ./file1
4       ./dir1/dir1-dira/dir1-dira-file1
12      ./dir1/dir1-dira
792     ./dir1/dir1-file2
64      ./dir1/dir1-file1
876     ./dir1
1092    .
```

### 以易读的方式显示目录大小

```shell
$ du -h /home/test
8.0K    /home/test/dir2
12K     /home/test/dir1/dir1-dira
876K    /home/test/dir1
1.1M    /home/test
```

### 显示当前目录大小

```shell
# 只显示当前目录总大小
$ du -s .
1932    .

# 显示当前目录下目录和文件大小（不包含子目录和子文件）
$ du -sh *
4.0K    conf
234M    data
0       logs
514M    mysqlbinlog
0       mysql-files
```

### 文件从大到小排序

```shell
$ du -sh * |sort -rh
2.9M    command
1.9M    assets
148K    template
72K     package-lock.json
52K     dist
28K     build
16K     README.md
4.0K    renovate.json
4.0K    package.json
4.0K    LICENSE
```

