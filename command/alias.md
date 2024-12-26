alias
===

定义或显示别名。

## 补充说明

alias命令的全称是alias definition，它的作用是为一个或多个命令定义一个新的名称，也就是别名。使用别名可以让你用一个简单的单词来代替一个复杂的命令，或者为一个命令添加一些默认的选项，从而简化你的输入和操作。例如，你可以用ll来代替ls -l，或者用rm来代替rm -i，这样就可以节省你的时间和精力。

## 适用的Linux版本

alias命令是一个内置的shell命令，它可以在多数Linux发行版（如Debian、Ubuntu、Alpine、Arch Linux、Kali Linux、RedHat/CentOS、Fedora、Raspbian）的主要终端命令解释器（包括bash、zsh、csh、ksh、fish、tcsh）中使用。如果你不确定你的系统是否支持alias命令，你可以用type命令来检查一下。例如，如果你的shell是bash，你可以输入：

```shell
$ type alias
alias is a shell builtin
```

这说明alias命令是一个shell内置的命令，你可以直接使用它。如果你的shell不支持alias命令，你可能会看到类似这样的输出：

```shell
$ type alias
bash: type: alias: not found
```

这说明alias命令在你的shell中不存在，你可能需要换一个shell或者安装一个支持alias命令的shell。例如，如果你想安装bash，你可以根据你的Linux发行版使用不同的包管理器来安装它。如果你的系统是基于Debian的，你可以用apt命令来安装：

```shell
$ sudo apt install bash
```

如果你的系统是基于Red Hat的，你可以用yum或者dnf命令来安装：

```shell
$ sudo yum install bash
```

或者

```shell
$ sudo dnf install bash
```

安装完成后，你可以用chsh命令来切换你的默认shell为bash：

```shell
$ chsh -s /bin/bash
```

然后重新登录你的系统，你就可以使用alias命令了。

```shell
$ alias [-p] [name[=value] ...]
```

## 命令语法

```shell
alias [选项] [别名]='[命令]'
```

- 别名：是一个由用户自定义的字符串，用来代替一个或多个命令。别名不能包含特殊字符，也不能使用alias和unalias作为别名，因为它们是保留的关键字。
- 命令：是一个有效的Linux命令，可以包含选项、参数和变量。命令必须用单引号或者双引号括起来，以防止被shell解释。如果用单引号，那么命令中的变量不会被展开，如果用双引号，那么命令中的变量会被展开。

## 选项

| 选项 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| -p   | 打印当前已经定义的所有别名，每个别名占一行，格式为alias name='value' |
| -a   | 打印当前已经定义的所有别名，每个别名占一行，格式为name='value'，不带alias前缀 |
| name | 取消指定的别名，可以同时取消多个别名，用空格分隔。如果没有指定别名，相当于-p选项 |

## 示例

### 创建一个别名

如果你想为一个命令创建一个别名，你可以直接使用alias命令，然后指定别名和命令。例如，如果你想用la来代替ls -a，你可以输入：

```shell
$ alias la='ls -a'
```

这样，当你输入la时，就相当于输入ls -a，可以显示当前目录下的所有文件，包括隐藏文件。

### 查看一个别名

如果你想查看一个别名的定义，你可以用type命令，然后指定别名。例如，如果你想查看la的定义，你可以输入：

```shell
$ type la
la is aliased to `ls -a'
```

这样，你就可以看到la是一个别名，它的值是ls -a。

### 查看所有别名

如果你想查看当前已经定义的所有别名，你可以用alias命令，不带任何参数。例如，你可以输入：

```shell
$ alias
alias la='ls -a'
alias ll='ls -l'
alias rm='rm -i'
```

这样，你就可以看到当前已经定义的三个别名，分别是la、ll和rm，以及它们的值。

### 删除一个别名

如果你想删除一个别名，你可以用unalias命令，然后指定别名。例如，如果你想删除la这个别名，你可以输入：

```shell
$ unalias la
```

这样，la这个别名就被取消了，当你输入la时，就会提示命令不存在。

### 删除所有别名

如果你想删除所有的别名，你可以用unalias命令，然后加上-a选项。例如，你可以输入：

```shell
$ unalias -a
```

这样，所有的别名都被取消了，当你输入任何别名时，都会提示命令不存在。

### 创建一个包含多个命令的别名

如果你想为一系列的命令创建一个别名，你可以用分号（;）或者管道符（|）来分隔不同的命令，然后用引号括起来。例如，如果你想用backup来代替以下的命令：

```shell
$ cd /home/linux
$ tar -czvf backup.tar.gz *
$ mv backup.tar.gz /mnt/backup
```

你可以输入：

```shell
$ alias backup='cd /home/linux; tar -czvf backup.tar.gz *; mv backup.tar.gz /mnt/backup'
```

这样，当你输入backup时，就相当于执行了上面的三个命令，可以将你的主目录下的所有文件打包并移动到备份目录。

### 创建一个带有选项的别名

如果你想在创建别名时引用一些额外的选项，你可以将它们作为值的一部分。例如，添加move作为mv命令的别名，带有-i选项，表示在覆盖文件时询问确认：

```shell
$ alias move='mv -i'
```

这样，当你使用move命令移动或重命名文件时，如果目标文件已经存在，系统会提示你是否要覆盖它。

### 创建一个运行脚本的别名

另一种使用别名的方法是创建一个快捷方式来运行一些脚本。要做到这一点，你需要提供脚本的绝对路径作为别名的值。例如，创建一个别名来运行一个名为file_rename.sh的bash脚本，它位于Example/Test目录下：

```shell
$ alias frename='Example/Test/file_rename.sh'
```

这样，当你输入frename并按回车时，就相当于运行了file_rename.sh脚本。

### 创建一个永久的别名

如果你想让一个别名永久有效，你需要将它添加到你的shell配置文件中。不同的shell可能有不同的配置文件，例如bash的配置文件是.bashrc，zsh的配置文件是.zshrc，它们都位于你的/home/<用户名>/目录下。你可以使用任何文本编辑器来编辑这些文件，例如vim或nano。例如，使用vim编辑.bashrc文件：

```shell
$ vim ~/.bashrc
```

这将打开你的.bashrc文件，你需要在文件的末尾添加你想要的别名，就像在终端中一样。你可以使用注释来说明每个别名的作用，或者将它们分成不同的块，以便于维护。例如：

```shell
# Alias for common commands
alias c='clear'
alias la='ls -la'
alias move='mv -i'

# Alias for running scripts
alias frename='Example/Test/file_rename.sh'
```

在保存并退出文件后，你需要使用source命令或者点（.）命令来重新加载.bashrc文件，使得新的别名生效。例如：

```shell
$ source ~/.bashrc
```

或者

```shell
$ . ~/.bashrc
```

这样，你就可以在任何新的终端会话中使用你定义的别名了。

### 创建一个全局的别名

如果你想让一个别名对所有用户都有效，你需要将它添加到全局的shell配置文件中。这个文件通常位于/etc目录下，它的名称取决于你使用的shell，例如bash的全局配置文件是/etc/bashrc，zsh的全局配置文件是/etc/zshrc。你需要有root权限才能编辑这些文件，你可以使用sudo命令来提升权限。例如，使用sudo和vim编辑/etc/bashrc文件：

```shell
$ sudo vim /etc/bashrc
```

这将打开/etc/bashrc文件，你需要在文件的末尾添加你想要的别名，就像在终端。

## 注意事项

在使用alias命令时，你需要注意以下几点：

- alias命令只能为一个命令创建一个别名，如果你想为一个命令创建多个别名，你需要使用不同的名称。例如，你不能使用alias ls='ls -l'和alias ls='ls -a'来为ls命令创建两个别名，因为后一个会覆盖前一个。
- alias命令不能为一个别名创建一个别名，你需要使用原始的命令作为值。例如，你不能使用alias ll='la'来为la命令创建一个别名，因为la本身就是一个别名，你需要使用alias ll='ls -la'来为ls -la命令创建一个别名。
- alias命令不能为一个函数或一个变量创建一个别名，你需要使用原始的函数或变量作为值。例如，你不能使用alias f='foo'来为foo函数创建一个别名，因为foo是一个函数，你需要使用alias f='foo()'来为foo函数创建一个别名。
- alias命令不能为一个关键字或一个保留字创建一个别名，你需要使用一个不同的名称。例如，你不能使用alias if='echo "if statement"'来为if关键字创建一个别名，因为if是一个保留的关键字，你需要使用一个不同的名称，如alias iff='echo "if statement"'。
- alias命令不能为一个已经存在的命令创建一个别名，除非你想覆盖它的原始功能。例如，你不能使用alias ls='echo "Hello World"'来为ls命令创建一个别名，除非你想让ls命令失去它的原始功能，而是输出Hello World。
- 如果你在创建别名时没有使用单引号（'）或者双引号（"）来包裹值，那么值中的空格会被当作分隔符，而不是命令的一部分。例如，如果你使用alias la=ls -la来为ls -la命令创建一个别名，那么实际上你创建了两个别名，一个是la，它的值是ls，另一个是-la，它的值是空。这样，当你输入la并按回车时，就相当于输入了ls命令，而不是ls -la命令。
- 如果你在创建别名时没有使用单引号（'）或者双引号（"）来包裹值，那么值中的变量会被立即展开，而不是在执行命令时展开。例如，如果你使用alias dt=date +"%Y-%m-%d %H:%M:%S"来为date +"%Y-%m-%d %H:%M:%S"命令创建一个别名，那么实际上你创建了一个别名，它的值是date +当前的日期和时间。这样，当你输入dt并按回车时，就相当于输入了date +当前的日期和时间命令，而不是date +"%Y-%m-%d %H:%M:%S"命令，这意味着你每次看到的都是你创建别名时的日期和时间，而不是当前的日期和时间。
- 如果你想在创建别名时使用变量，你需要使用双引号（"）来包裹值，这样变量会在执行命令时展开，而不是在创建别名时展开。例如，如果你使用alias dt="date +"%Y-%m-%d %H:%M:%S""来为date +"%Y-%m-%d %H:%M:%S"命令创建一个别名，那么你创建了一个别名，它的值是date +"%Y-%m-%d %H:%M:%S"。这样，当你输入dt并按回车时，就相当于输入了date +"%Y-%m-%d %H:%M:%S"命令，这意味着你每次看到的都是当前的日期和时间，而不是你创建别名时的日期和时间。
- 如果你在创建别名时使用了单引号（'）来包裹值，那么值中的任何字符都会被当作字面值，而不会被解释或展开。例如，如果你使用alias dt='date +"%Y-%m-%d %H:%M:%S"'来为date +"%Y-%m-%d %H:%M:%S"命令创建一个别名，那么你创建了一个别名，它的值是date +"%Y-%m-%d %H:%M:%S"。这样，当你输入dt并按回车时，就相当于输入了date +"%Y-%m-%d %H:%M:%S"命令，这意味着你每次看到的都是当前的日期和时间，而不是你创建别名时的日期和时间。
- 如果你在创建别名时使用了反斜杠（\）来转义某些字符，那么这些字符会被当作字面值，而不会被解释或展开。例如，如果你使用alias dt=date +%Y-%m-%d %H:%M:%S来为date +"%Y-%m-%d %H:%M:%S"命令创建一个别名，那么你创建了一个别名，它的值是date +%Y-%m-%d %H:%M:%S。这样，当你输入dt并按回车时，就相当于输入了date +%Y-%m-%d %H:%M:%S命令，这意味着你每次看到的都是当前的日期和时间，而不是你创建的别名。
