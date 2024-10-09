cat
===

连接多个文件并打印到标准输出。

## 补充说明

cat命令是concatenate（连接）的缩写，它的主要功能是连接文件或标准输入并打印到标准输出。它可以用来显示文件内容，创建文件，合并文件，追加文件内容等。cat命令是Linux系统中最常用的命令之一，它可以帮助我们快速查看或处理文本文件。

## 适用的Linux版本

cat命令是GNU coreutils包中的一个基本命令，它适用于几乎所有的Linux发行版，如Ubuntu, Debian, Fedora, CentOS, Red Hat等。如果你的系统没有安装GNU coreutils包，你可以使用以下命令进行安装：

- Ubuntu、Debian

```shell
apt-get install coreutils
```

- CentOS、Red Hat

```shell
yum install coreutils
```

- Fedora

```shell
dnf install coreutils
```

## 命令语法

```
cat [OPTION] [FILE]
```

其中，选项可以用来指定一些附加的功能，文件可以是一个或多个要处理的文件名，如果没有指定文件或文件为 `-`，则表示从标准输入读取数据。

## 选项

| 选项              | 说明                                 |
| ----------------- | ------------------------------------ |
| -A                | 显示所有特殊字符，相当于 `-vET`      |
| -b                | 对非空输出行编号                     |
| -e                | 在每行结束处显示 `$`，相当于 `-vE`   |
| -E 或 --show-ends | 在每行的末尾加上$符号                |
| -n                | 对所有输出行编号                     |
| -s                | 压缩连续的空行为一行                 |
| -t                | 显示制表符为 `^I`，相当于 `-vT`      |
| -T 或 --show-tabs | 将制表符显示为 `^I`                  |
| -v                | 显示非打印字符，除了换行符和制表符   |
| -u                | 禁用输出缓冲，即不缓存数据，直接输出 |

## 示例

### 显示一个文件内容

```
cat file.txt
```

将 file.txt 文件的内容打印到标准输出。

### 查看多个文件的内容

```
cat file1.txt file2.txt
```

显示 file1.txt 和 file2.txt 的内容

### 创建文件

```
cat > file.txt
```

从标准输入读取数据，并将其写入file.txt文件中，按 `Ctrl+D` 结束输入。

### 合并文件

```
cat file1.txt file2.txt > file3.txt
```

将 file1.txt 和 file2.txt 文件的内容合并，并将其写入 file3.txt 文件中。

### 追加文件内容

```
cat file1.txt >> file2.txt
```

将 file1.txt 文件的内容追加到file2.txt文件的末尾。

### 显示行号

```
cat -n file.txt
```

这会在每一行的前面显示行号。

### 给非空白行编号

```
cat -b file.txt
```

给 file.txt 的非空白行编号，空白行不编号

### 显示特殊字符

```
cat -A file.txt
```

显示文件中的所有特殊字符，如制表符、换行符、回车符等，制表符显示为 `^I`，行尾显示为 `$`。

### 压缩连续的空白行

```
cat -s file.txt
```

将 file.txt 中连续的多个空白行压缩为一个空白行

### 制作软盘镜像文件

```
cat /dev/fd0 > floppy.img
```

将软盘设备 /dev/fd0 的内容复制到 floppy.img 文件中

## Linux cat命令的注意事项

- 如果要查看大型文件或二进制文件，建议使用 less 或 hexdump 等其他工具，以避免占用过多内存或输出乱码。
- 如果要编辑文件内容，建议使用 vim 或 nano 等文本编辑器，而不是使用 cat 重定向覆盖原文件。
- 如果要复制文件内容到剪贴板，建议使用 xclip 或 xsel 等工具，而不是使用 cat 输出到终端。
- cat命令不会修改原始文件的内容，除非使用重定向符号覆盖原始文件。
- cat命令不会自动换行，除非源文件本身有换行符。
- cat命令不会检查源文件是否存在或是否可读，如果出现错误，会直接报错。
