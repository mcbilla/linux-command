tail
===

在屏幕上显示指定文件的末尾若干行

## 补充说明

tail 是一个在终端中使用的命令，它主要用于查看文件的内容，特别是文件的末尾部分。通常，tail 命令被用于实时监控日志文件的更新，这在系统管理和故障排查中非常有用。

## 适用的Linux版本

`tail`命令几乎在所有的Linux版本中都是可用的，因为它是GNU coreutils软件包的一部分，该软件包在大多数Linux发行版中都是预安装的。如果出现`bash: tail: command not found`错误，您可能需要安装coreutils包。但这种情况非常罕见，以下是一些通用的安装命令：

```
# 对于大多数Linux发行版，coreutils应该已经预安装了。如果没有，可以尝试以下命令：
# 基于apt的发行版（如Debian、Ubuntu等）
sudo apt-get update && sudo apt-get install coreutils

# 基于yum的发行版（如RedHat、CentOS 7等）
sudo yum update && sudo yum install coreutils

# 基于dnf的发行版（如Fedora、CentOS 8等）
sudo dnf update && sudo dnf install coreutils

# 如果你使用的是macOS：
brew install coreutils
```

## 命令语法

```
tail [OPTION]... [FILE]...
```

## 选项

| 选项        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| `-c`        | 输出文件末尾的指定字节数                                     |
| `-f`        | 跟踪显示文件新追加的内容                                     |
| `-n`        | 输出文件末尾的指定行数                                       |
| `--pid=PID` | 与 `-f` 一起使用，当进程 PID 终止时，tail 会停止跟踪文件     |
| `-q`        | 不输出文件头（文件名）                                       |
| `-s`        | 与 `-f` 一起使用，指定在检查文件是否已被追加新内容时的睡眠间隔 |
| `-v`        | 总是输出文件头（文件名）                                     |

## 示例

### 实例1：查看文件末尾内容

查看文件`/var/log/syslog`的最后10行内容。

```
$ tail /var/log/syslog
```

### 实例2：查看文件末尾指定行数

查看文件`/var/log/syslog`的最后20行内容。

```
$ tail -n 20 /var/log/syslog
```

### 实例3：实时跟踪文件更新

实时监控`/var/log/syslog`文件的新内容。

```
$ tail -f /var/log/syslog
```

### 实例4：查看多个文件的末尾内容

同时查看多个文件的末尾内容。

```
$ tail -n 5 /var/log/syslog /var/log/auth.log
```

### 实例5：查看文件末尾指定字节数内容

查看文件`/var/log/syslog`的最后50个字节内容。

```
$ tail -c 50 /var/log/syslog
```

### 实例6：监视文件直到特定进程结束

监视文件，直到指定的进程结束。

```
$ tail --pid=1234 -f /var/log/syslog
```

### 实例7：在跟踪模式下，显示所有标题信息

在实时跟踪模式下，显示所有文件的标题信息。

```
$ tail -f -v /var/log/syslog
```

### 实例8：结合`grep`命令实时监控特定内容

实时监控`/var/log/syslog`文件，但仅显示包含关键字"error"的行。

```
$ tail -f /var/log/syslog | grep 'error'
```

### 实例9：在跟踪模式下显示除最后N行之外的内容

跳过最后10行并持续监控文件的更新。

```
$ tail -n +10 -f /var/log/syslog
```

### 实例10：监控多个文件并用`awk`添加自定义格式

同时监控多个日志文件，并使用`awk`为输出添加时间戳。

```
$ tail -f /var/log/syslog /var/log/auth.log | awk '{print strftime("[%Y-%m-%d %H:%M:%S]"), $0}'
```

### 实例11：使用`-retry`选项在文件不可用时继续尝试

当文件暂时不可访问时，尝试继续读取。

```
$ tail --retry -f /var/log/syslog
```

### 实例12：结合`head`命令查看文件的特定区间行

显示文件的第100行到第110行。

```
$ tail -n +100 /var/log/syslog | head -n 11
```

### 实例13：使用`F`选项代替`f`以处理日志轮转

- `F`选项类似于`f`，但当文件被移动或删除后，它会尝试重新打开该文件，这对于处理日志文件轮转非常有用。

```
$ tail -F /var/log/syslog
```

### 实例14：结合`sed`命令实时处理日志内容

实时监控日志文件的同时，使用`sed`命令删除每行中的某个特定单词。

```
$ tail -f /var/log/syslog | sed 's/error//g'
```

### 实例15：使用`-follow=name`选项并指定尝试次数

使用`--follow`选项明确指定跟踪方式，并使用`--max-unchanged-stats`指定在认为文件没有改变之前的尝试读取次数。

```
$ tail --follow=name --retry --max-unchanged-stats=10 /var/log/syslog
```

### 实例16：监控文件直到其他命令完成

使用`tail`监控日志文件，直到另一个命令（如`service nginx restart`）完成后停止。

```
$ service nginx restart & tail -f /var/log/nginx/error.log
```

## 注意事项

1. **权限**：您可能需要适当的权限来查看某些日志文件，例如使用`sudo`。
2. **文件更新**：使用`f`选项时，如果文件被删除，`tail`可能会停止输出新内容，直到该文件被重新创建。
3. **性能**：在监控大型日志文件或多个文件时，请注意系统性能，因为这可能会消耗大量的I/O资源。
4. **日志轮转**：有时日志文件会轮转，例如，`syslog`可能变成`syslog.1`。这时，您可能需要更新您监控的文件名，或者使用通配符和工具来处理日志轮转。
5. **符号链接**：如果您正在跟踪的文件是一个符号链接，当链接指向的目标文件改变时，`tail -f`将不会跟踪新的文件内容。

