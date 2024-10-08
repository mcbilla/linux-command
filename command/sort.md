sort
===

对文本文件中所有行进行排序

## 补充说明

Linux sort 命令是用于对文本文件的内容进行排序的工具。它可以按照不同的规则和选项来对文本行进行排序，从而提高可读性和便利性。sort 命令的全称是 sort lines of text files，意思是对文本文件的行进行排序。

## 适用的Linux版本

sort 命令是 GNU coreutils 包中的一个常用命令，它在大多数 Linux 发行版中都是默认安装的。如果没有安装 sort 命令，可以使用以下命令来安装：

* CentOS 7 和 CentOS 8

```shell
yum install coreutils
```

* Ubuntu 和 Debian

```shell
apt-get install coreutils
```

## 命令语法

```shell
sort [options] [file...]
```

* options是可选的参数，用于指定排序的规则和选项
* file...是可选的文件名，用于指定要排序的文件。如果没有指定文件名，则 sort 命令会从标准输入读取数据。

## 选项

排序选项：

```
-b, --ignore-leading-blanks    忽略每行前面开始出的空格字符。
-d, --dictionary-order         排序时，处理英文字母、数字及空格字符，忽略其他的字符。
-f, --ignore-case              将小写字母作为大写字母考虑。
-g, --general-numeric-sort     根据数字排序。
-i, --ignore-nonprinting       排序时，除了040至176之间的ASCII字符外，忽略其他的字符（排除不可打印字符）。
-M, --month-sort               按照非月份、一月、十二月的顺序排序。
-h, --human-numeric-sort       根据存储容量排序(注意使用大写字母，例如：2K 1G)。
-n, --numeric-sort             根据数字排序。
-R, --random-sort              随机排序，但分组相同的行。
--random-source=FILE           从FILE中获取随机长度的字节。
-r, --reverse                  将结果倒序排列。
--sort=WORD                    根据WORD排序，其中: general-numeric 等价于 -g，human-numeric 等价于 -h，month 等价于 -M，numeric 等价于 -n，random 等价于 -R，version 等价于 -V。
-V, --version-sort             文本中(版本)数字的自然排序。
```

其他选项：

```
--batch-size=NMERGE                    一次合并最多NMERGE个输入；超过部分使用临时文件。
-c, --check, --check=diagnose-first    检查输入是否已排序，该操作不会执行排序。
-C, --check=quiet, --check=silent      类似于 -c 选项，但不输出第一个未排序的行。
--compress-program=PROG                使用PROG压缩临时文件；使用PROG -d解压缩。
--debug                                注释用于排序的行，发送可疑用法的警报到stderr。
--files0-from=F                        从文件F中读取以NUL结尾的所有文件名称；如果F是 - ，那么从标准输入中读取名字。
-k, --key=KEYDEF                       通过一个key排序；KEYDEF给出位置和类型。
-m, --merge                            合并已排序文件，之后不再排序。
-o, --output=FILE                      将结果写入FILE而不是标准输出。
-s, --stable                           通过禁用最后的比较来稳定排序。
-S, --buffer-size=SIZE                 使用SIZE作为内存缓存大小。
-t, --field-separator=SEP              使用SEP作为列的分隔符。
-T, --temporary-directory=DIR          使用DIR作为临时目录，而不是 $TMPDIR 或 /tmp；多次使用该选项指定多个临时目录。
--parallel=N                           将并发运行的排序数更改为N。
-u, --unique                           同时使用-c，严格检查排序；不同时使用-c，输出排序后去重的结果。
-z, --zero-terminated                  设置行终止符为NUL（空），而不是换行符。
--help                                 显示帮助信息并退出。
--version                              显示版本信息并退出。

KEYDEF的格式为：F[.C][OPTS][,F[.C][OPTS]] ，表示开始到结束的位置。
F表示列的编号
C表示
OPTS为[bdfgiMhnRrV]中的一到多个字符，用于覆盖当前排序选项。
使用--debug选项可诊断出错误的用法。

SIZE 可以有以下的乘法后缀:
% 内存的1%；
b 1；
K 1024（默认）；
剩余的 M, G, T, P, E, Z, Y 可以类推出来。
```

### 实例1：按照字母顺序排序

假设有一个文件`names.txt`，其内容如下：

```shell
$ cat names.txt
Alice
Bob
Charlie
David
Eve
Frank
```

默认的方式将文本文件的第一列以 ASCII 码的次序排列，可以使用以下命令：

```shell
$ sort names.txt
Alice
Bob
Charlie
David
Eve
Frank
```

### 实例2：按照数字顺序排序

假设有一个文件`numbers.txt`，其内容如下：

```shell
$ cat numbers.txt
10
5
100
50
1
```

要按照数字顺序对文件进行排序，可以使用以下命令：

```shell
$ sort -n numbers.txt
1
5
10
50
100
```

注意，如果不使用`-n`选项，sort命令会按照字符串的顺序进行排序，即：

```shell
$ sort numbers.txt
1
10
100
5
50
```

### 实例3：按照逆序排序

要按照逆序对文件进行排序，可以使用`-r`选项。例如，要按照字母逆序对文件`names.txt`进行排序，可以使用以下命令：

```
$ sort -r names.txt
Frank
Eve
David
Charlie
Bob
Alice
```

要按照数字逆序对文件`numbers.txt`进行排序，可以使用以下命令：

```shell
$ sort -nr numbers.txt
100
50
10
5
1
```

### 实例4：按照字段或列排序

sort命令可以使用`-t`选项来指定字段或列的分隔符，然后使用`-k`选项来指定要排序的字段或列的范围。例如，假设有一个文件`scores.txt`，其内容如下：

```shell
$ cat scores.txt
Alice:90:80:85
Bob:95:75:80
Charlie:85:95:90
David:80:90:95
Eve:75:85:75
Frank:70:80:70
```

每一行表示一个学生的姓名和三门课程的成绩，用冒号分隔。要按照第一门课程的成绩进行排序，可以使用以下命令：

```shell
$ sort -t':' -k2 scores.txt
Frank:70:80:70
Eve:75:85:75
David:80:90:95
Charlie:85:95:90
Alice:90:80:85
Bob:95:75:80
```

如果不指定字段或列的结束位置，默认是到行尾。如果要指定字段或列的结束位置，可以在`-k`选项后加上一个逗号和结束位置。例如，要按照第二门课程的成绩进行排序，可以使用以下命令：

```
$ sort -t':' -k3,3 scores.txt
Bob:95:75:80
Alice:90:80:85
Frank:70:80:70
Eve:75:85:75
David:80:90:95
Charlie:85:95:90
```

如果要按照多个字段或列进行排序，可以在`-k`选项后加上多个范围。例如，要先按照第一门课程的成绩进行排序，再按照第二门课程的成绩进行排序，可以使用以下命令：

```shell
$ sort -t':' -k2,2 -k3,3 scores.txt
Frank:70:80:70
Eve:75:85:75
David:80:90:95
Charlie:85:95:90
Alice:90:80:85
Bob :95 :75 :80
```

### 实例5：按照月份排序

假设有一个文件`months.txt`，其内容如下：

```shell
$ cat months.txt
Jan
Feb
Mar
Apr
May
Jun
Jul
Aug
Sep
Oct
Nov
Dec
```

要按照月份的顺序对文件进行排序，可以使用`-M`选项。例如，要按照升序排序，可以使用以下命令：

```shell
$ sort -M months.txt
Jan
Feb
Mar
Apr
May
Jun
Jul
Aug
Sep
Oct
Nov
Dec
```

要按照降序排序，可以使用`-r`选项。例如，要按照降序排序，可以使用以下命令：

```shell
$ sort -Mr months.txt
Dec
Nov
Oct
Sep
Aug
Jul
Jun
May
Apr
Mar
Feb
Jan
```

### 实例5：按照月份排序

假设有一个文件`months.txt`，其内容如下：

```shell
$ cat months.txt
Jan
Feb
Mar
Apr
May
Jun
Jul
Aug
Sep
Oct
Nov
Dec
```

要按照月份的顺序对文件进行排序，可以使用`-M`选项。例如，使用以下命令：

```shell
$ sort -M months.txt
Jan
Feb
Mar
Apr
May
Jun
Jul
Aug
Sep
Oct
Nov
Dec
```

### 实例6：按照版本号排序

假设有一个文件`versions.txt`，其内容如下：

```shell
$ cat versions.txt
1.0.0
1.0.1
1.0.10
1.0.2
1.1.0
1.2.0
2.0.0
```

要按照版本号的顺序对文件进行排序，可以使用`-V`选项。例如，使用以下命令：

```
[linux@bashcommandnotfound.cn ~]$ sort -V versions.txt
1.0.0
1.0.1
1.0.2
1.0.10
1.1.0
1.2.0
2.0.0
```

### 实例7：按照人类可读的数字排序

假设有一个文件`sizes.txt`，其内容如下：

```shell
$ cat sizes.txt
10K
5M
100K
50M
1G
```

要按照人类可读的数字的顺序对文件进行排序，可以使用`-h`选项。例如，使用以下命令：

```shell
$ sort -h sizes.txt
10K
100K
5M
50M
1G
```

### 实例8：随机排序

要对文件或标准输入的内容进行随机排序，可以使用`-R`选项。例如，要对文件`names.txt`进行随机排序，可以使用以下命令：

```shell
$ sort -R names.txt
Charlie
Alice
Frank
Eve
David
Bob
```

每次执行该命令，输出的结果可能都不一样。

### 实例9：去重或合并

sort命令可以使用`-u`选项来去除重复的行。例如，假设有一个文件`colors.txt`，其内容如下：

```shell
$ cat colors.txt
red
blue
green
red
yellow
blue
```

要去除重复的颜色，可以使用以下命令：

```shell
$ sort -u colors.txt
blue
green
red
yellow
```

sort命令也可以使用`-m`选项来合并多个已经排序好的文件。例如，假设有两个文件`colors1.txt`和`colors2.txt`，其内容分别如下：

```shell
$ cat colors1.txt
blue
green
red
yellow

$ cat colors2.txt
black
cyan
magenta
white
```

要合并这两个文件，可以使用以下命令：

```shell
$ sort -m colors1.txt colors2.txt
black
blue
cyan
green
magenta
red
white
yellow
```

### 实例10：检查是否已经排序

sort命令可以使用`-c`选项来检查文件或标准输入的内容是否已经按照顺序排序。如果已经排序，sort命令不会输出任何内容；如果没有排序，sort命令会输出第一个乱序的行。例如，要检查文件`names.txt`是否已经按照字母顺序排序，可以使用以下命令：

```shell
$ sort -c names.txt
sort: names.txt:3: disorder: Charlie
```

这表示第三行的`Charlie`没有按照字母顺序排序。

要检查文件`numbers.txt`是否已经按照数字顺序排序，可以使用以下命令：

```shell
$ sort -nc numbers.txt
sort: numbers.txt:4: disorder: 5
```

这表示第四行的`5`没有按照数字顺序排序。

## Linux sort命令的注意事项

以下是一些使用sort命令时需要注意的事项：

- sort命令默认是按照当前的区域设置（locale）来进行排序的，这可能会影响到不同语言或字符集的排序结果。如果要使用标准的ASCII字符集来进行排序，可以在执行sort命令之前设置环境变量`LC_ALL=C`。
- sort命令默认是将标准错误输出（stderr）重定向到标准输出（stdout）的，这可能会导致一些错误信息被混入到排序结果中。如果要避免这种情况，可以在执行sort命令时加上`2>/dev/null`来忽略错误信息。
- sort命令默认是将排序结果输出到标准输出（stdout）的，如果要将排序结果保存到文件中，可以使用`o`选项或者重定向符号`>`。例如，要将文件`names.txt`按照字母顺序排序后保存到文件`sorted_names.txt`中，可以使用以下两种方法之一：

```
$ sort names.txt -o sorted_names.txt
$ sort names.txt > sorted_names.txt
```
