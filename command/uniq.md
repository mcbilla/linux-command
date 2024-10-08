uniq
===

检查和删除文本文件中的重复行

## 适用的Linux版本

uniq 命令是 GNU coreutils 软件包的一部分，它在大多数 Linux 发行版中都是默认安装的。如果没有安装，可以执行下面命令来安装：

* CentOS 7

```shell
$ sudo yum install coreutils
```

* CentOS 8

```shell
$ sudo dnf install coreutils
```

* Ubuntu

```shell
$ sudo apt install coreutils
```

## 命令语法

```shell
uniq [options] [input_file] [output_file]
```

* options 是可选的参数，用来指定一些选项。

* input_file 是要处理的已排序的文本文件，如果没有指定，则从标准输入读取数据。

* output_file 是要输出的文本文件，如果没有指定，则输出到标准输出设备（显示终端）。

## 选项

```shell
-c 或 --count	在每行旁边显示该行在文件中出现的次数
-d 或 --repeated	只显示重复出现的行
-u 或 --unique	只显示只出现一次的行
-i 或 --ignore-case	忽略大小写差异
-f num 或 --skip-fields=num	忽略比较前num个字段
-s num 或 --skip-chars=num	忽略比较前num个字符
-w num 或 --check-chars=num	只比较每行前num个字符
```

## 示例

### 文本去重

```shell
$ cat testfile
test 30  
test 30  
test 30  
Hello 95  
Hello 95  
Hello 95  
Hello 95  
Linux 85  
Linux 85 
```

使用 uniq 命令删除重复的行后，有如下输出结果：

```shell
$ uniq testfile
test 30  
Hello 95  
Linux 85 
```

当重复的行并不相邻时，uniq 命令是不起作用的。即若文件内容为以下时，uniq 命令不起作用：

```shell
$ cat testfile1
test 30  
Hello 95  
Linux 85 
test 30  
Hello 95  
Linux 85 
test 30  
Hello 95  
Linux 85 
```

这时我们就可以使用 sort：

```shell
$ sort  testfile1 | uniq
Hello 95  
Linux 85 
test 30
```

### 统计各行在文件中出现的次数

我们有一个文本文件 test.txt，它包含以下内容：

```shell
$ cat test.txt
apple
banana
orange
apple
pear
banana
apple
orange

```

统计各行在文件中出现的次数，并使用 `-c` 选项来显示计数：

```shell
$ sort test.txt | uniq -c
      3 apple
      2 banana
      2 orange
      1 pear
```

### 只显示重复出现的行

我们可以使用 `-d` 选项来只显示重复出现的行，不显示只出现一次的行：

```shell
$ sort test.txt | uniq -d
apple
banana
orange
```

### 只显示只出现一次的行

我们可以使用 `-u` 选项来只显示只出现一次的行，不显示重复出现的行：

```shell
$ sort test.txt | uniq -u
pear
```

### 忽略大小写差异

我们有一个文本文件test2.txt，它包含以下内容：

```shell
$ cat test2.txt
Apple
apple
Banana
banana
Orange
orange
Pear
pear
```

我们可以使用 `-i` 选项来忽略大小写差异，将大写和小写视为相同：

```shell
$ sort test2.txt | uniq -i -c
      2 Apple
      2 Banana
      2 Orange
      2 Pear
```

### 忽略比较前num个字段或字符

我们有一个文本文件test3.txt，它包含以下内容：

```shell
$ cat test3.txt
1 apple red
2 banana yellow
3 orange orange
4 apple green
5 pear yellow
6 banana green
7 apple yellow
8 orange green
```

我们可以使用 `-f num`选项来忽略比较前 num 个字段，以空格为字段分隔符。例如，如果我们想忽略第一个字段，只比较水果的名称，我们可以使用 -f 1 选项：

```shell
$ sort test3.txt | uniq -f 1 -c 
      3 1 apple red 
      2 2 banana yellow 
      2 3 orange orange 
      1 5 pear yellow 
```

我们也可以使用 `-s num` 选项来忽略比较前num个字符。例如，如果我们想忽略前两个字符，只比较水果的颜色，我们可以使用 -s 2 选项：

```shell
$ sort test3.txt | uniq -s 2 -c 
      1 1 apple red 
      3 2 banana yellow 
      1 3 orange orange 
      1 4 apple green 
      1 6 banana green 
      1 8 orange green 
```

### 只比较每行前num个字符

我们有一个文本文件test4.txt，它包含以下内容：

```shell
$ cat test4.txt 
apple pie 
apple juice 
banana bread 
banana split 
orange juice 
orange peel 
pear jam 
pear cake 
```

我们可以使用 `-w num` 选项来只比较每行前num个字符。例如，如果我们想只比较水果的名称，不比较后面的食物，我们可以使用 -w 5 选项：

```shell
$ sort test4.txt | uniq -w 5 -c 
      2 apple pie 
      2 banana bread 
      2 orange juice 
      2 pear jam  
```

## Linux uniq命令的注意事项

* uniq 命令要求输入文件是已排序的，否则它不能正确地检测重复的行。因此，通常需要先使用 sort 命令对文件进行排序，然后再使用 uniq 命令进行处理。
* uniq命令只能检测相邻的重复行，如果重复行不相邻，它会被视为唯一的行。因此，在使用uniq命令之前，最好先对文件进行排序，以便将重复行放在一起。
* uniq命令默认区分大小写，如果想忽略大小写差异，需要使用 `-i` 选项。

