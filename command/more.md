more
===

显示文件内容，每次显示一屏

## 补充说明

more命令是一个用于在终端中分页显示文本文件的过滤器。它是UNIX系统中最古老的终端分页器之一。最初，more命令只能向下滚动，但现在我们可以使用它向上滚动一屏或向下滚动一行或一屏。more命令在读取文件的百分比时，在其状态栏上显示。当它到达文件的末尾时，它会自动关闭，而不需要按任何按钮。

## 适用的Linux版本

more命令在大多数Linux和类Unix操作系统上都可用。如果某些Linux版本不支持more命令，我们可以尝试使用less或most命令，它们提供了更多的功能和增强。我们可以使用包管理器来安装less或most命令。例如，在Ubuntu上，我们可以使用以下命令来安装它们：

```bash
$ sudo apt install less
$ sudo apt install most
```

## 基本语法

```bash
more [options] file...
```

其中，options是可选的参数，用于改变文件显示的方式；file是一个或多个要显示的文件名。

## 常用选项

more命令有许多可用的选项，我们可以使用h或?来查看它们的帮助信息。以下是一些常用的选项：

| 选项      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| -d        | 在屏幕下方提示用户“[Press space to continue, 'q' to quit.]”，如果用户按错键，则显示“[Press 'h' for instructions.]”而不是响铃 |
| -l        | 不要在遇到包含^L（换页符）的任何行时暂停                     |
| -f        | 计算行数时，以实际上的行数，而非自动换行后的行数（有些单行字数太长的会被扩展为两行或两行以上） |
| -p        | 不以卷动的方式显示每一页，而是先清除屏幕后再显示内容         |
| -c        | 跟-p类似，不同的是先显示内容再清除其他旧数据                 |
| -s        | 当遇到有连续两行以上的空白行，就代换为一行的空白行           |
| -u        | 不显示下划线                                                 |
| -n number | 指定每屏显示的行数                                           |
| +number   | 从第number行开始显示文件                                     |
| +/pattern | 在每个文件显示前搜索该模式（正则表达式），然后从该模式之后开始显示 |

常用操作命令：

- Enter：向下n行，需要定义。默认为1行
- Ctrl+F：向下滚动一屏
- 空格键：向下滚动一屏
- Ctrl+B：返回上一屏
- =：输出当前行的行号
- :f：输出文件名和当前行的行号
- V：调用vi编辑器
- !：调用Shell，并执行命令
- q：退出more

## 示例

- 显示test.txt文件的内容，并在屏幕下方提示用户操作：

```bash
$ more -d test.txt
```

- 显示test.txt文件的内容，并忽略换页符：

```bash
$ more -l test.txt
```

- 显示test.txt文件的内容，并不折行长行：

```bash
$ more -f test.txt
```

- 显示test.txt文件的内容，并每屏显示20行：

```bash
$ more -n 20 test.txt
```

- 显示test.txt文件的内容，并从第10行开始：

```bash
$ more +10 test.txt
```

- 显示test.txt文件的内容，并从第一个匹配hello的地方开始：

```bash
$ more +/hello test.txt
```

- 显示test1.txt和test2.txt两个文件的内容，并在它们之间用 `--More--(Next file: test2.txt)` 分隔：

```bash
$ more test1.txt test2.txt
```

- 显示ls命令的输出，并分页显示：

```bash
$ ls | more
```

## Linux more命令的注意事项

- more命令只能向上滚动一屏，不能向上滚动一行，如果需要更灵活的滚动，可以使用less或most命令。
- more命令不支持水平滚动，如果需要查看长行的内容，可以使用-f选项或使用less或most命令。
- more命令不支持实时监视文件的变化，如果需要这个功能，可以使用less或most命令。

