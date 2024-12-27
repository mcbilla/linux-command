cp
===

将源文件或目录复制到目标文件或目录中

## 补充说明

**cp命令**用来将一个或多个源文件或者目录复制到指定的目的文件或目录。cp命令可以将一个或多个源文件或目录复制到指定的目的文件或目录中。它可以创建源文件的精确副本，也可以根据需要修改文件的属性和权限。

## 适用的Linux版本

cp命令是一个通用的Linux命令，它适用于几乎所有的Linux发行版，包括Ubuntu, Debian, Fedora, CentOS, Red Hat, SUSE, Arch Linux等。如果你的系统中没有安装cp命令，你可以使用以下命令来安装它：

```shell
# 对于基于Debian的系统，如Ubuntu
sudo apt-get install coreutils

# 对于基于Red Hat的系统，如Fedora
sudo yum install coreutils

# 对于基于Arch的系统
sudo pacman -S coreutils
```

##  命令语法

```shell
cp [选项] 源文件 目标文件
```

其中，源文件（source）表示要复制的文件或目录的路径，目标文件（destination）表示复制后的文件或目录的路径。

* 如果源文件有多个，那么目标文件必须是一个已存在的目录。
* 如果源文件和目标文件都是文件，那么cp命令会将源文件复制到目标文件中。
* 如果目标文件不存在，那么cp命令会创建它。如果目标文件已存在，那么cp命令会覆盖它，除非使用了-n选项。

##  选项

| 选项     | 说明                                                 |
| -------- | ---------------------------------------------------- |
| -a       | 复制目录及其所有内容，并保留链接、属性和权限         |
| -b       | 在覆盖已存在的目标文件之前，创建一个备份文件         |
| -d       | 复制时保留链接，而不是复制链接指向的文件             |
| -f       | 强制复制，即使目标文件已存在也会覆盖，而且不给出提示 |
| -i       | 在覆盖已存在的目标文件之前，提示用户确认             |
| -l       | 不复制文件，只是创建链接文件                         |
| -n       | 不覆盖已存在的目标文件                               |
| -p       | 保留源文件的属性、权限和时间戳信息                   |
| -r 或 -R | 递归复制目录及其所有内容                             |
| -u       | 只复制更新时间较新的源文件                           |
| -v       | 显示详细的复制过程                                   |

## 示例

- 将当前目录下的file.txt复制到同一目录下，并命名为file_backup.txt：

```bash
cp file.txt file_backup.txt
```

- 将当前目录下的file.txt复制到/backup目录下，并保留原来的文件名：

```bash
cp file.txt /backup
```

- 将当前目录下的file.txt复制到/backup目录下，并命名为new_file.txt：

```bash
cp file.txt /backup/new_file.txt
```

- 将当前目录下的file1.txt和file2.txt复制到/backup目录下：

```bash
cp file1.txt file2.txt /backup
```

- 将当前目录下所有以.txt结尾的文件复制到/backup目录下：

```bash
cp *.txt /backup
```

- 将当前目录及其所有内容复制到/backup目录下，并创建一个新的子目录current：

```bash
cp -r . /backup/current
```

- 将当前目录及其所有内容复制到/backup/current目录下，并覆盖已存在的同名文件：

```bash
cp -rf . /backup/current
```

- 将当前目录及其所有内容复制到/backup/current目录下，并在覆盖已存在的同名文件之前，创建一个备份文件：

```bash
cp -rb . /backup/current
```

- 将当前目录及其所有内容复制到/backup/current目录下，并在覆盖已存在的同名文件之前，提示用户确认：

```bash
cp -ri . /backup/current
```

- 将当前目录及其所有内容复制到/backup/current目录下，并保留源文件的属性、权限和时间戳信息：

```bash
cp -rp . /backup/current
```

- 将当前目录及其所有内容复制到/backup/current目录下，并显示详细的复制过程：

```bash
cp -rv . /backup/current
```

- 将当前目录及其所有内容复制到/backup/current目录下，并只复制更新时间较新的源文件：

```bash
cp -ru . /backup/current
```

- 将当前目录下的file.txt复制到同一目录下，并创建一个硬链接文件link.txt：

```bash
cp -l file.txt link.txt
```

- 将当前目录下的file.txt复制到同一目录下，并创建一个符号链接文件link.txt：

```bash
cp -s file.txt link.txt
```

- 将当前目录下的file.txt复制到同一目录下，并不覆盖已存在的同名文件：

```bash
cp -n file.txt file.txt
```

## 注意事项

- 在使用cp命令时，要注意源文件和目标文件的路径是否正确，以免造成不必要的损失。
- 在使用cp命令时，要注意是否需要保留源文件的属性、权限和时间戳信息，以免造成不必要的麻烦。
- 在使用cp命令时，要注意是否需要覆盖已存在的同名文件，以免造成不必要的损失。如果需要覆盖，可以使用-f选项或-i选项来强制或提示覆盖。如果不需要覆盖，可以使用-n选项来阻止覆盖。
- 在使用cp命令时，要注意是否需要递归复制目录及其所有内容，以免造成不必要的占用空间。如果需要递归复制，可以使用-r或-R选项来实现。如果不需要递归复制，可以使用--no-preserve=mode选项来忽略子目录和子文件。
- 在使用cp命令时，要注意是否需要创建链接文件，以免造成不必要的混乱。如果需要创建硬链接文件，可以使用-l选项来实现。如果需要创建符号链接文件，可以使用-s选项来实现。



###  
