head
===

显示文件的开头部分

## 补充说明

head 命令是一个用来显示文件开头内容的命令，它可以显示指定文件的前 N 行或前 N 个字节。head 命令常用于查看日志文件或文本文件的开头部分，以便快速了解文件的基本信息。head 命令还可以和其他命令结合使用，比如 tail、grep、sed 等，实现更多的功能。

## 适用的Linux版本

head命令是一个标准的Linux命令，它适用于所有的Linux发行版，包括Ubuntu、CentOS、Debian、Fedora等。head命令通常已经预装在系统中，无需额外安装。如果你的系统中没有head命令，你可以使用以下命令来安装：

- Ubuntu/Debian

```shell
apt install coreutils
```

- CentOS 7

```shell
yum install coreutils
```

- CentOS 8

```shell
dnf install coreutils
```

## 命令语法

```
head [option] [file]
```

* option：如果没有指定 options，默认显示前 10 行。

* file：如果没有指定文件，或者文件为`-`，则从标准输入读取数据。如果指定了多个文件，每个文件的输出之前会加上一个文件名的标题。

## 选项

| 选项 | 参数 | 说明                                              |
| ---- | ---- | ------------------------------------------------- |
| -c   | 数字 | 显示文件的前N个字节，数字可以带有单位，如KB、MB等 |
| -n   | 数字 | 显示文件的前N行，数字必须为正整数                 |
| -q   | 无   | 不显示文件名的标题，只显示文件内容                |
| -v   | 无   | 总是显示文件名的标题，即使只有一个文件            |

## 示例

### 实例1：显示文件的前10行（默认）

如果不指定任何选项，head命令默认显示文件的前10行。例如，我们有一个名为test.txt的文件，内容如下：

```
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.
Some lines have spaces, like this one.
Some lines have tabs, like	this one.
Some lines have nothing, like the next one.

This is the last line of the file.
```

我们可以使用以下命令来显示文件的前10行：

```shell
$ head test.txt
```

输出结果如下：

```
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.
Some lines have spaces, like this one.
Some lines have tabs, like	this one.
Some lines have nothing, like the next one.

This is the last line of the file.
```

### 实例2：显示文件的前5行

如果我们想要显示文件的前N行，我们可以使用-n选项，后面跟上一个正整数。例如，我们想要显示文件的前5行，我们可以使用以下命令：

```shell
$ head -n 5 test.txt
```

输出结果如下：

```
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.
```

### 实例3：显示文件的前20个字节

如果我们想要显示文件的前N个字节，我们可以使用-c选项，后面跟上一个数字，可以带有单位，如KB、MB等。例如，我们想要显示文件的前20个字节，我们可以使用以下命令：

```shell
$ head -c 20 test.txt
```

输出结果如下：

```
Hello, this is a test
```

注意，这里的字节是按照ASCII码计算的，一个英文字符占一个字节，一个中文字符占两个字节。如果文件中有中文，那么显示的字节数可能不等于显示的字符数。

### 实例4：显示文件的前1KB

如果我们想要显示文件的前N个单位，我们可以使用-c选项，后面跟上一个数字，再跟上一个单位，如KB、MB等。例如，我们想要显示文件的前1KB，我们可以使用以下命令：

```shell
$ head -c 1K test.txt
```

输出结果如下：

```
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.
Some lines have spaces, like this one.
Some lines have tabs, like	this one.
Some lines have nothing, like the next one.

This is the last line of the file.
```

注意，这里的单位是按照1024进制计算的，即1KB=1024字节，1MB=1024KB，以此类推。如果文件的大小不是单位的整数倍，那么显示的内容可能不完整。

### 实例5：显示多个文件的前10行

如果我们想要显示多个文件的前10行，我们可以在head命令后面跟上多个文件名，用空格隔开。例如，我们有另一个名为test2.txt的文件，内容如下：

```
This is another test file.
It has different content from the first one.
It also has multiple lines of text.
But it has fewer lines than the first one.
This is the fifth line of the file.
This is the sixth line of the file.
This is the seventh line of the file.
This is the eighth line of the file.
This is the ninth line of the file.
This is the tenth line of the file.
```

我们可以使用以下命令来显示两个文件的前10行：

```shell
$ head test.txt test2.txt
```

输出结果如下：

```
==> test.txt <==
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.
Some lines have spaces, like this one.
Some lines have tabs, like	this one.
Some lines have nothing, like the next one.

This is the last line of the file.

==> test2.txt <==
This is another test file.
It has different content from the first one.
It also has multiple lines of text.
But it has fewer lines than the first one.
This is the fifth line of the file.
This is the sixth line of the file.
This is the seventh line of the file.
This is the eighth line of the file.
This is the ninth line of the file.
This is the tenth line of the file.
```

注意，每个文件的输出之前都有一个文件名的标题，用`==>`和`<==`包围，以便区分不同的文件。

### 实例6：显示多个文件的前5行

如果我们想要显示多个文件的前N行，我们可以在head命令后面先跟上-n选项和一个正整数，再跟上多个文件名，用空格隔开。例如，我们想要显示两个文件的前5行，我们可以使用以下命令：

```shell
$ head -n 5 test.txt test2.txt
```

输出结果如下：

```
==> test.txt <==
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.

==> test2.txt <==
This is another test file.
It has different content from the first one.
It also has multiple lines of text.
But it has fewer lines than the first one.
This is the fifth line of the file.
```

### 实例7：不显示文件名的标题

如果我们不想要显示每个文件的输出之前的文件名的标题，我们可以使用-q选项，这样只会显示文件内容，不会显示文件名。例如，我们想要显示两个文件的前5行，但不显示文件名，我们可以使用以下命令：

```shell
$ head -q -n 5 test.txt test2.txt
```

输出结果如下：

```
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.
This is another test file.
It has different content from the first one.
It also has multiple lines of text.
But it has fewer lines than the first one.
This is the fifth line of the file.
```

注意，这里的输出没有任何分隔符，两个文件的内容直接连接在一起，这可能会导致混淆。如果我们想要在两个文件的内容之间加上一个空行，我们可以使用`echo`命令和管道符`|`来实现。例如，我们可以使用以下命令：

```shell
$ head -q -n 5 test.txt test2.txt | echo; cat
```

输出结果如下：

```
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.

This is another test file.
It has different content from the first one.
It also has multiple lines of text.
But it has fewer lines than the first one.
This is the fifth line of the file.
```

这里，我们使用`echo`命令来输出一个空行，然后使用管道符`|`来将其和head命令的输出连接起来，再使用`cat`命令来显示最终的输出。这样，我们就在两个文件的内容之间加上了一个空行，以便区分。

### 实例8：总是显示文件名的标题

如果我们想要总是显示每个文件的输出之前的文件名的标题，即使只有一个文件，我们可以使用-v选项，这样会强制显示文件名。例如，我们想要显示一个文件的前5行，并显示文件名，我们可以使用以下命令：

```shell
$ head -v -n 5 test.txt
```

输出结果如下：

```
==> test.txt <==
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.
```

注意，这里的输出有一个文件名的标题，用`==>`和`<==`包围，即使只有一个文件。这个选项在我们想要明确指出文件名的时候很有用。

### 实例9：从标准输入读取数据

如果我们不指定任何文件，或者文件为`-`，那么head命令会从标准输入读取数据，直到遇到文件结束符（EOF）或者满足指定的行数或字节数。例如，我们可以使用以下命令来从标准输入读取数据，并显示前5行：

```shell
$ head -n 5
```

然后，我们可以输入任意的数据，比如：

```
This is some data from standard input.
We can type anything we want.
But only the first 5 lines will be shown.
The rest will be ignored.
This is the fifth line.
```

按下Ctrl+D来结束输入，输出结果如下：

```
This is some data from standard input.
We can type anything we want.
But only the first 5 lines will be shown.
The rest will be ignored.
This is the fifth line.
```

注意，这里的输出只有我们输入的前5行，后面的内容都被忽略了。这个功能在我们想要快速查看一些数据的时候很有用，比如从其他命令的输出中截取一部分。

### 实例10：和其他命令结合使用

head命令可以和其他命令结合使用，实现更多的功能。我们可以使用管道符`|`来将head命令和其他命令的输出连接起来，或者使用反引号```或者美元符号`$()`来将head命令的输出作为其他命令的参数。下面给出一些常见的例子：

- 我们可以使用tail命令来显示文件的末尾部分，然后使用head命令来显示文件的最后一行。例如，我们可以使用以下命令：

```shell
$ tail test.txt | head -n 1
```

输出结果如下：

```
This is the last line of the file.
```

这里，我们使用tail命令来显示test.txt文件的末尾10行，然后使用head命令来显示其中的第一行，也就是文件的最后一行。

- 我们可以使用grep命令来搜索文件中包含指定字符串的行，然后使用head命令来显示其中的前N行。例如，我们想要搜索test.txt文件中包含`line`的行，并显示其中的前3行，我们可以使用以下命令：

```shell
$ grep line test.txt | head -n 3
```

输出结果如下：

```
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
```

这里，我们使用grep命令来搜索test.txt文件中包含`line`的行，然后使用head命令来显示其中的前3行。

- 我们可以使用sed命令来对文件进行文本替换，然后使用head命令来显示替换后的前N行。例如，我们想要将test.txt文件中的`line`替换为`row`，并显示替换后的前5行，我们可以使用以下命令：

```shell
$ sed 's/line/row/g' test.txt | head -n 5
```

输出结果如下：

```
Hello, this is a test file.
It has multiple rows of text.
Some rows are longer than others.
Some rows have numbers, like 123.
Some rows have symbols, like @#$%.
```

这里，我们使用sed命令来将test.txt文件中的`line`替换为`row`，并使用g选项来表示全局替换，然后使用head命令来显示替换后的前5行。

- 我们可以使用wc命令来统计文件的行数、字数、字节数，然后使用head命令的输出作为wc命令的参数。例如，我们想要统计test.txt文件的前5行的行数、字数、字节数，我们可以使用以下命令：

```shell
$ wc $(head -n 5 test.txt)
```

输出结果如下：

```
5 25 132
```

这里，我们使用head命令来显示test.txt文件的前5行，然后使用美元符号`$()`来将其作为wc命令的参数，wc命令会统计其中的行数、字数、字节数，并输出结果。

### 实例11：显示文件的前10%的内容

如果我们想要显示文件的前N%的内容，我们可以使用-c选项，后面跟上一个数字，再跟上一个百分号`%`。例如，我们想要显示test.txt文件的前10%的内容，我们可以使用以下命令：

```shell
$ head -c 10% test.txt
```

输出结果如下：

```
Hello, this is a test file.
It has multiple lines of text.
Some lines are longer than others.
Some lines have numbers, like 123.
Some lines have symbols, like @#$%.
Some lines have spaces, like this one.
Some lines have tabs, like	this one.
Some lines have nothing, like the next one.

This is the last line of the file.
```

注意，这里的百分比是按照文件的字节数计算的，而不是按照文件的行数或字符数。如果文件的字节数不是百分比的整数倍，那么显示的内容可能不完整。

### 实例12：显示文件的前10个单词

如果我们想要显示文件的前N个单词，我们可以使用-w选项，后面跟上一个数字。例如，我们想要显示test.txt文件的前10个单词，我们可以使用以下命令：

```shell
$ head -w 10 test.txt
```

输出结果如下：

```
Hello, this is a test file. It has
```

注意，这里的单词是按照空格或换行符分隔的，而不是按照标点符号或其他符号。如果文件中有中文或其他非空格分隔的语言，那么显示的单词可能不准确。

### 实例13：显示文件的前10个字符

如果我们想要显示文件的前N个字符，我们可以使用-m选项，后面跟上一个数字。例如，我们想要显示test.txt文件的前10个字符，我们可以使用以下命令：

```shell
$ head -m 10 test.txt
```

输出结果如下：

```
Hello, this
```

注意，这里的字符是按照Unicode编码计算的，一个英文字符占一个字符，一个中文字符也占一个字符。如果文件中有特殊的控制字符，比如换行符、制表符等，那么显示的字符可能不完整。

### 实例14：显示文件的第一行

如果我们想要显示文件的第一行，我们可以使用-n选项，后面跟上一个数字1。例如，我们想要显示test.txt文件的第一行，我们可以使用以下命令：

```shell
$ head -n 1 test.txt
```

输出结果如下：

```
Hello, this is a test file.
```

这个功能在我们想要快速查看文件的标题或版本信息的时候很有用。

### 实例15：显示文件的第一行和最后一行

如果我们想要显示文件的第一行和最后一行，我们可以使用head命令和tail命令结合使用，然后使用分号`;`来将两个命令连接起来。例如，我们想要显示test.txt文件的第一行和最后一行，我们可以使用以下命令：

```shell
$ head -n 1 test.txt; tail -n 1 test.txt
```

输出结果如下：

```
Hello, this is a test file.
This is the last line of the file.
```

这个功能在我们想要快速查看文件的开头和结尾的内容的时候很有用。

## 注意事项

在使用head命令时，我们需要注意以下几点：

- head命令只能显示文件的开头部分，不能显示文件的中间或末尾部分。如果我们想要显示文件的中间或末尾部分，我们可以使用其他命令，比如tail、sed、awk等。
- head命令的输出可能不完整或不准确，如果文件的大小不是单位的整数倍，或者文件中有中文或其他非ASCII字符，或者文件中有特殊的控制字符，比如换行符、制表符等。在这些情况下，我们需要使用其他工具，比如hexdump、od等，来查看文件的原始数据。
- head命令可能会遇到`bash: head: command not found`的错误，这表示系统中没有安装head命令，或者head命令的路径没有添加到环境变量中。

