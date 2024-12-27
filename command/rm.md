rm
===

删除指定的文件和目录

## 补充说明

**rm**  **命令** 可以删除一个目录中的一个或多个文件或目录，也可以将某个目录及其下属的所有文件及其子目录均删除掉。对于链接文件，只是删除整个链接文件，而原有文件保持不变。

注意：使用rm命令要格外小心。rm命令删除的文件或目录默认不会放入回收站，而是直接从磁盘上删除，所以一旦删除了一个文件，就无法再恢复它。在删除文件之前，最好再看一下文件的内容，确定是否真要删除。rm命令可以用-i选项，这个选项在使用文件扩展名字符删除多个文件时特别有用。使用这个选项，系统会要求你逐一确定是否要删除。这时，必须输入y并按Enter键，才能删除文件。如果仅按Enter键或其他字符，文件不会被删除。

## 适用的Linux版本

rm命令是Linux系统中的基本命令，几乎所有的Linux发行版都支持rm命令。如果某些Linux版本没有安装rm命令，可以使用以下命令来安装：

```shell
# Debian/Ubuntu
sudo apt install coreutils

# CentOS/RedHat
sudo yum install coreutils

# Fedora
sudo dnf install coreutils
```

## 命令语法

```shell
rm [选项] 文件或目录...
```

文件：指定被删除的文件列表，如果参数中含有目录，则必须加上`-r`或者`-R`选项。

## 选项

```shell
-f, --force：强制删除，忽略不存在的文件或目录，不提示确认。
-i, --interactive：交互式删除，在每次删除前提示确认。
-r, -R, --recursive：递归删除，删除指定的目录及其所有子目录和文件。
-v, --verbose：显示详细的删除信息。
–help：显示帮助信息。
–version：显示版本信息。
```

## 示例

- 删除一个文件：

```bash
# 删除文件file.txt
$ rm file.txt
rm: 是否删除 一般文件 "file.txt"? y
```

- 删除多个文件：

```bash
# 删除文件file1.txt和file2.txt
$ rm file1.txt file2.txt
rm: 是否删除 一般文件 "file1.txt"? y
rm: 是否删除 一般文件 "file2.txt"? y

# 删除以.txt为后缀的所有文件
$ rm *.txt
rm: 是否删除 一般文件 "file1.txt"? y
rm: 是否删除 一般文件 "file2.txt"? y
...
```

- 删除一个目录：

```bash
# 删除目录dir1
$ rm dir1
rm: 无法删除目录"dir1": 是一个目录

# 删除目录则必须配合选项"-r"
$ rm -r dir1
rm: 是否删除 目录 "dir1"? y

# 使用-rf选项强制删除非空目录dir2及其所有内容，不提示确认
$ rm -rf dir2
```

- 删除一个非空目录及其所有内容：

```bash
# 使用-r选项删除非空目录dir2及其所有内容
$ rm -r dir2
```

- 删除dir3目录下的所有文件及目录（带*号的要慎用）

```bash
$ rm -r dir3/*
```

- 显示详细的删除信息：

```bash
# 使用-v选项显示详细的删除信息
$ rm -iv file5.txt dir4

# 输出如下：
removed 'file5.txt'
removed 'dir4/file6.txt'
removed directory 'dir4'
```

## 注意事项

- rm命令是一个危险的命令，一旦执行就无法恢复，所以在使用rm命令时要非常小心，尽量避免使用-f选项，以免误删重要的文件或目录。
- rm命令只能删除普通文件或目录，不能删除设备文件、符号链接、管道等特殊文件，如果要删除这些文件，可以使用unlink命令。
- rm命令不能删除正在使用的文件或目录，如果要删除这些文件或目录，可以先关闭相关的进程或服务，然后再执行rm命令。

## 高级技巧

- 如果要删除一个文件或目录，但是不想输入完整的路径或名称，可以使用Tab键进行自动补全，例如：

```bash
# 输入rm /t后按Tab键，会自动补全为rm /tmp/
$ rm /t<Tab>
rm /tmp/
```

- 如果要删除一个文件或目录，但是不确定它是否存在，可以使用&&运算符来判断，例如：

```bash
# 如果文件file7.txt存在，则删除它，否则不执行任何操作
$ [ -f file7.txt ] && rm file7.txt
```

- 如果要删除一个文件或目录，但是想保留一个备份，可以使用cp命令先复制一份到另一个位置，然后再执行rm命令，例如：

```bash
# 先将文件file8.txt复制到/tmp目录下，然后再删除它
$ cp file8.txt /tmp && rm file8.txt
```
