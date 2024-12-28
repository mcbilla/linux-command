unrar
===

解压缩rar文件

## 补充说明

`unrar`是用来解压缩RAR压缩文件的工具。它支持多种模式，包括测试压缩文件的完整性、查看压缩文件内容和解压单个文件或整个压缩包。

## 适用的Linux版本

`unrar`命令并非所有Linux发行版都预安装。如果在尝试执行时遇到`bash: unrar: command not found`错误，需要手动安装。

安装命令如下：

```bash
# 基于apt的发行版（如Debian、Ubuntu、Raspbian、Kali Linux等）
sudo apt-get update && sudo apt-get install unrar

# 基于yum的发行版（如RedHat，CentOS 7等）
sudo yum update && sudo yum install unrar

# 基于dnf的发行版（如Fedora，CentOS 8等）
sudo dnf update && sudo dnf install unrar

# 基于apk的发行版（如Alpine Linux）
sudo apk add --update unrar

# 基于pacman的发行版（如Arch Linux）
sudo pacman -Syu && sudo pacman -S unrar

# 基于zypper的发行版（如openSUSE）
sudo zypper ref && sudo zypper in unrar

# 基于pkg的FreeBSD发行版
sudo pkg update && sudo pkg install unrar

# 基于brew的OS X/macOS发行版
brew update && brew install unrar
```

##  命令语法

```
unrar <command> [option] <rar_file> [destination]
```

##  选项

| 选项 | 描述                                               |
| ---- | -------------------------------------------------- |
| e    | 将文件从压缩包中提取到当前目录下                   |
| l    | 列出压缩包内的文件                                 |
| p    | 将文件的内容输出到标准输出                         |
| t    | 测试压缩文件的完整性                               |
| x    | 将文件从压缩包中提取到指定目录下，保持原有目录结构 |

## 示例

### 查看RAR文件中的内容

查看压缩包内的文件列表，不进行解压。

```bash
$ unrar l example.rar
```

### 测试RAR文件的完整性

检查压缩包是否完整，不提取文件。

```bash
$ unrar t example.rar
```

### 解压RAR文件到当前目录

将RAR压缩文件中的所有内容解压到当前目录。

```bash
$ unrar e example.rar
```

### 解压RAR文件到指定目录

解压RAR文件到指定目录，同时保持文件的原有目录结构。

```bash
$ unrar x example.rar /path/to/destination/
```

### 提取有密码的RAR文件

如果RAR文件被密码保护，你可以使用 `-p` 选项 followed by the password to extract its contents:

```bash
$ unrar x -pYOUR_PASSWORD example.rar
```

请将 `YOUR_PASSWORD` 替换为实际的密码，注意在 `-p` 选项和密码之间没有空格。

### 仅提取RAR文件中的特定文件

如果你只想提取RAR压缩包中的特定文件，可以指定文件名：

```bash
$ unrar e example.rar filename.txt
```

将 `filename.txt` 替换为你想提取的文件名。

### 提取RAR文件并排除特定文件

如果你想提取RAR压缩包的所有文件，但排除一些特定的文件，可以使用排除模式：

```bash
$ unrar x example.rar /path/to/destination/ -x*filename_to_exclude*
```

将 `filename_to_exclude` 替换为你想排除的文件名或模式。`-x` 选项可以多次使用来排除多个文件或模式。

### 提取RAR文件并仅包含特定文件

相反，如果你只想提取匹配特定模式的文件，可以使用包含模式：

```bash
$ unrar x example.rar /path/to/destination/ -i*pattern*
```

替换 `pattern` 为你想提取的文件的模式。这里的 `pattern` 可以是部分文件名、通配符或者正则表达式。

### 设置解压缩过程的优先级

如果你需要在解压RAR文件时设置进程的优先级，可以使用 `nice` 命令：

```bash
$ nice -n 10 unrar x example.rar /path/to/destination/
```

这里 `-n 10` 设置了进程的优先级（范围从-20（最高优先级）到19（最低优先级））。调整这个值可以在系统负载较重时减少 `unrar` 命令对资源的占用。

### 批量解压多个RAR文件

如果你想要解压同一目录下的多个RAR文件，可以使用 `for` 循环：

```bash
$ for file in *.rar; do unrar x "$file" /path/to/destination/; done
```

这个命令会逐个找到当前目录下的所有 `.rar` 文件，并将它们解压到指定的目录。
