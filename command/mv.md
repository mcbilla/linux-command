mv
===

对文件或目录重命名或者移动

## 补充说明

**mv命令** 用来对文件或目录重新命名，或者将将源文件移至目标文件或者目标目录中。注意：mv 与 cp 的结果不同，mv 好像文件“搬家”，文件个数并未增加。而 cp 对文件进行复制，文件个数增加了。

## 适用的Linux版本

mv命令是一个通用的Linux命令，它适用于几乎所有的Linux发行版，如Ubuntu, Debian, CentOS, Fedora, Red Hat等。 如果你的Linux系统中没有安装mv命令，你可以使用以下命令来安装它：

* 基于Debian的系统，如Ubuntu

```shell
sudo apt install coreutils
```

* 基于Red Hat的系统，如CentOS

```shell
sudo yum install coreutils
```

##  命令语法

```shell
mv [选项] 源文件或目录 目标文件或目录
```

其中，源文件或目录是要移动或重命名的文件或目录，目标文件或目录是移动后或重命名后的文件或目录。如果源文件或目录有多个，那么目标文件或目录必须是一个已存在的目录。如果源文件或目录只有一个，那么目标文件或目录可以是一个不存在的文件或目录，此时相当于重命名操作。

##  选项

```shell
-f 或 --force：强制覆盖已存在的目标文件或目录，不提示用户确认。
-i 或 --interactive：在覆盖已存在的目标文件或目录之前，提示用户确认。
-n 或 --no-clobber：不覆盖已存在的目标文件或目录，不提示用户确认。
-u 或 --update：只在源文件或目录比目标文件或目录新时才进行移动操作。
-v 或 --verbose：显示移动操作的详细信息。
-b 或 --backup：在覆盖已存在的目标文件或目录之前，对其进行备份。
-t 或 --target-directory：指定一个已存在的目标目录，将所有源文件或目录移动到该目录中。
-T 或 --no-target-directory：将源文件或目录视为单个实体，不管它是否是一个已存在的目录。
```

## 示例

### 将文件file1.txt重命名为file2.txt

```bash
$ mv file1.txt file2.txt
```

### 将当前目录下所有文件移动到test目录中

```shell
$ mv ./* test
```

### 将当前目录下所有以.txt结尾的文件移动到test目录中

```bash
# 使用通配符*匹配所有以.txt结尾的文件
# 使用-t选项指定test为目标目录
$ mv -t test *.txt
```

### 将test目录重命名为test2

```bash
$ mv test test2
```

### 将test目录移动到/home/user目录下，并保持原来的名称

```bash
$ mv -t /home/user test
```

### 将test目录移动到/home/user目录下，并重命名为test2

```bash
# 直接指定目标文件或目录为/home/user/test2
$ mv test /home/user/test2
```

### 在移动文件或目录之前，提示用户确认

```bash
# 使用-i选项开启交互模式
$ mv -i file1.txt file2.txt
# 如果file2.txt已存在，会显示如下信息，并等待用户输入y或n
mv: overwrite 'file2.txt'? 
```

### 在移动文件或目录之前，对其进行备份

```bash
# 使用-b选项开启备份模式
$ mv -b file1.txt file2.txt
# 如果file2.txt已存在，会将其备份为file2.txt~
```

### 显示移动操作的详细信息

```bash
# 使用-v选项开启详细模式
$ mv -v file1.txt file2.txt
# 会显示如下信息，表示file1.txt已被重命名为file2.txt
renamed 'file1.txt' -> 'file2.txt'
```

### 不覆盖已存在的目标文件或目录

```bash
# 使用-n选项开启不覆盖模式
$ mv -n file1.txt file2.txt
# 如果file2.txt已存在，不会进行任何操作，也不会提示用户确认
```

### 只在源文件或目录比目标文件或目录新时才进行移动操作

```bash
# 使用-u选项开启更新模式
$ mv -u file1.txt file2.txt
# 如果file1.txt的修改时间比file2.txt的修改时间晚，会进行移动操作，否则不会进行任何操作，也不会提示用户确认
```

### 将多个源文件或目录移动到一个已存在的目标目录中

```bash
# 直接指定多个源文件或目录和一个已存在的目标目录作为参数，以空格分隔
$ mv file1.txt file2.txt test3 test4 /home/user/test5 
# 会将file1.txt, file2.txt, test3, test4都移动到/home/user/test5这个已存在的目录中，并保持原来的名称，如果有同名的文件或目录，会覆盖它们，除非使用其他选项来改变这一行为。
```

### 将多个源文件或目录移动到一个不存在的文件或目录中

```bash
# 直接指定多个源文件或目录和一个不存在的文件或目录作为参数，以空格分隔
$ mv file1.txt file2.txt test3 test4 /home/user/test5 
# 会报错，提示/home/user/test5不是一个已存在的目录
mv: target '/home/user/test5' is not a directory
```

### 将一个源文件或目录视为单个实体，不管它是否是一个已存在的目录

```bash
# 使用-T选项开启不作为目标目录模式
$ mv -T file1.txt /home/user/test5 
# 会将file1.txt重命名为/home/user/test5，不管/home/user/test5是否是一个已存在的目录，如果是，会覆盖它，除非使用其他选项来改变这一行为。
```
