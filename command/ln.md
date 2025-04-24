ln
===

为文件创建链接

## 补充说明

ln（link files）的功能是为文件或目录创建链接。ln 可以创建硬链接或软链接，默认创建硬链接。

硬链接和软链接的区别：

![img](https://xzchsia.github.io/img/in-post/linux-hard-soft-link/linux-soft-hard-link-diff.png)

硬链接：

- 硬链接和源文件指向相同的inode节点，但不占用实际空间，硬链接文件相当于文件的另一个入口。
- 删除硬链接文件或者删除源文件任意之一，文件实体并未被删除。只有删除了源文件和所有对应的硬链接文件，文件实体才会被删除。可以通过给文件设置硬链接文件来防止重要文件被误删。
- 不能对目录创建硬链接，不能对不同文件系统创建硬链接，不能对不存在的文件创建硬链接。

软链接：

- 软链接和源文件的inode节点号不同，进而指向的block也不同，软连接block中存放了源文件的路径名，类似于Windows操作系统中的快捷方式。
- 软链接和源文件是两个独立的文件，删除源文件，软链接依然存在，但无法访问源文件内容，软链接失效时一般是白字红底闪烁。
- 可以对目录创建软链接，可以跨文件系统创建软链接，可以对不存在的文件创建软链接。

## 适用的linux版本

ln命令是一个通用的Linux命令，它在大多数Linux发行版中都可以使用。如果某些Linux系统没有安装ln命令，可以通过以下命令来安装：

```bash
# Debian/Ubuntu
sudo apt-get install coreutils

# Red Hat/CentOS
sudo yum install coreutils
#或者
sudo dnf install coreutils

# Arch Linux
sudo pacman -S coreutils
```

##  命令语法

```shell
ln [选项] 源文件 目标文件
```

其中，源文件是要创建链接的原始文件或目录，目标文件是要创建的链接文件或目录。如果没有指定选项，ln命令默认创建硬链接。如果要创建符号链接，需要使用-s选项。

##  选项

```shell
--backup[=CONTROL]          # 为每个已存在的目标文件创建备份文件
-b                          # 类似--backup，但不接受任何参数
-d, -F, --directory         # 创建指向目录的硬链接(只适用于超级用户)
-f, --force                 # 强行删除任何已存在的目标文件
-i, --interactive           # 覆盖既有文件之前先询问用户
-L, --logical               # 取消引用作为符号链接的目标
-n, --no-dereference        # 把符号链接的目的目录视为一般文件
-P, --physical              # 直接将硬链接到符号链接
-r, --relative              # 创建相对于链接位置的符号链接
-s, --symbolic              # 对源文件建立符号链接，而非硬链接
-S, --suffix=SUFFIX         # 用"-b"参数备份目标文件后，备份文件的字尾会被加上一个备份字符串，预设的备份字符串是符号“~”，用户可通过“-S”参数来改变它
-t, --target-directory=DIRECTORY # 指定要在其中创建链接的DIRECTORY
-T, --no-target-directory   # 将“LINK_NAME”视为常规文件
-v, --verbose               # 打印每个链接文件的名称
--help                      # 显示此帮助信息并退出
--version                   # 显示版本信息并退出
```

##  示例

### 为一个文件创建硬链接

```bash
# 创建一个名为file1.txt的文件，并写入一些内容
echo "Hello, world!" > file1.txt

# 为file1.txt创建一个硬链接，名为file2.txt
ln file1.txt file2.txt

# 查看两个文件的inode号和内容，可以看到它们是相同的
ls -i file1.txt file2.txt
cat file1.txt file2.txt
```

### 为一个目录创建硬链接（失败）

```bash
# 创建一个名为dir1的目录，并在其中创建一些文件
mkdir dir1 && cd dir1 && touch a b c && cd ..

# 试图为dir1创建一个硬链接，名为dir2（失败）
ln dir1 dir2

# 查看错误信息，提示不允许为目录创建硬链接
ln: 'dir1': hard link not allowed for directory
```

### 为一个文件创建软链接

```bash
# 创建一个名为file3.txt的文件，并写入一些内容
echo "Hello, Linux!" > file3.txt

# 为file3.txt创建一个符号链接，名为file4.txt
ln -s file3.txt file4.txt

# 查看两个文件的inode号和内容，可以看到它们是不同的，但内容相同
ls -i file3.txt file4.txt
cat file3.txt file4.txt
```

### 为一个目录创建软链接

```bash
# 创建一个名为dir3的目录，并在其中创建一些文件
mkdir dir3 && cd dir3 && touch d e f && cd ..

# 为dir3创建一个符号链接，名为dir4
ln -s dir3 dir4

# 查看两个目录的inode号和内容，可以看到它们是不同的，但内容相同
ls -i dir3 dir4
ls -l dir3 dir4
```

### 覆盖已经存在的目标文件

```bash
# 创建一个名为file5.txt的文件，并写入一些内容
echo "Hello, world!" > file5.txt

# 创建一个名为file6.txt的文件，并写入一些内容
echo "Hello, Linux!" > file6.txt

# 试图为file5.txt创建一个硬链接，名为file6.txt（失败）
ln file5.txt file6.txt

# 查看错误信息，提示目标文件已经存在
ln: 'file6.txt': File exists

# 使用-f选项强制覆盖目标文件
ln -f file5.txt file6.txt

# 查看两个文件的inode号和内容，可以看到它们是相同的，且内容为file5.txt的内容
ls -i file5.txt file6.txt
cat file5.txt file6.txt
```

### 在覆盖已经存在的目标文件之前进行备份

```bash
# 创建一个名为file7.txt的文件，并写入一些内容
echo "Hello, world!" > file7.txt

# 创建一个名为file8.txt的文件，并写入一些内容
echo "Hello, Linux!" > file8.txt

# 使用-b选项在覆盖目标文件之前进行备份，备份文件的后缀为~
ln -b file7.txt file8.txt

# 查看两个文件和备份文件的inode号和内容，可以看到它们是不同的，且内容分别为file7.txt和file8.txt的内容
ls -i file7.txt file8.txt file8.txt~
cat file7.txt file8.txt file8.txt~
```

### 在覆盖已经存在的目标文件之前询问用户是否确认

```bash
# 创建一个名为file9.txt的文件，并写入一些内容
echo "Hello, world!" > file9.txt

# 创建一个名为file10.txt的文件，并写入一些内容
echo "Hello, Linux!" > file10.txt

# 使用-i选项在覆盖目标文件之前询问用户是否确认，输入y表示确认，输入n表示取消
ln -i file9.txt file10.txt

# 查看提示信息，提示是否覆盖目标文件，输入y表示确认，输入n表示取消
ln: replace 'file10.txt'? y # 输入y表示确认，输入n表示取消

# 查看两个文件的inode号和内容，可以看到它们是相同的，且内容为file9.txt的内容（如果输入n，则不会发生任何改变）
ls -i file9.txt file10.txt 
cat file9.txt file10.txt 
```

### 示每个创建的链接的详细信息

```bash
# 创建一个名为file11.txt的文件，并写入一些内容
echo "Hello, world!" > file11.txt

# 为file11.txt创建两个硬链接，分别名为file12.txt和file13.txt，并使用-v选项显示每个创建的链接的详细信息
ln -v file11.txt file12.txt
ln -v file11.txt file13.txt

# 查看输出信息，显示了每个创建的链接的源文件和目标文件
'file11.txt' -> 'file12.txt'
'file11.txt' -> 'file13.txt'
```

### 当创建符号链接时，不要跟随目标目录

```bash
# 创建一个名为dir5的目录，并在其中创建一些文件
mkdir dir5 && cd dir5 && touch g h i && cd ..

# 为dir5创建一个符号链接，名为dir6，并使用-n选项不要跟随目标目录
ln -s -n dir5 dir6

# 查看两个目录的inode号和内容，可以看到它们是不同的，但内容相同
ls -i dir5 dir6
ls -l dir5 dir6
```

### 为一个远程文件创建符号链接

```bash
# 创建一个名为file14.txt的文件，并写入一些内容
echo "Hello, world!" > file14.txt

# 将file14.txt上传到一个远程服务器，假设地址为http://example.com/file14.txt
scp file14.txt user@example.com:~/file14.txt

# 为远程文件创建一个符号链接，名为file15.txt，并使用完整的URL作为源文件
ln -s http://example.com/file14.txt file15.txt

# 查看两个文件的inode号和内容，可以看到它们是不同的，但内容相同
ls -i file14.txt file15.txt
cat file14.txt file15.txt
```

### 为一个不存在的文件创建符号链接（成功）

```bash
# 试图为一个不存在的文件，名为file16.txt，创建一个符号链接，名为file17.txt（成功）
ln -s file16.txt file17.txt

# 查看两个文件的inode号和内容，可以看到它们是不同的，且内容为空
ls -i file16.txt file17.txt
cat file16.txt file17.txt

# 查看file17.txt的属性，可以看到它是一个符号链接，但是指向了一个不存在的文件，因此显示为红色或带有删除线
ls -l file17.txt
```

### 删除一个硬链接或符号链接

```bash
# 创建一个名为file18.txt的文件，并写入一些内容
echo "Hello, world!" > file18.txt

# 为file18.txt创建一个硬链接，名为file19.txt，并查看它们的inode号和内容
ln file18.txt file19.txt && ls -i file18.txt file19.txt && cat file18.txt file19.txt 

# 删除file19.txt这个硬链接，并查看它们的inode号和内容，可以看到只有file18.txt还存在，且内容不变
rm file19.txt && ls -i file18.txt file19.txt && cat file18.txt file19.txt 

# 为file18.txt创建一个符号链接，名为file20.txt，并查看它们的inode号和内容
ln -s file18.txt file20.txt && ls -i file18.txt file20.txt && cat file18.txt file20.txt 

# 删除file20.txt这个符号链接，并查看它们的inode号和内容，可以看到只有file18.txt还存在，且内容不变
rm file20.txt && ls -i file18.txt file20.txt && cat file18.txt file20.tx 
```

### 修改源文件或目标文件的内容或名称对链接的影响

```bash
# 创建一个名为file21.txt的文件，并写入一些内容
echo "Hello, world!" > file21.txt

# 为file21.txt创建一个硬链接，名为file22.txt，并查看它们的inode号和内容
ln file21.txt file22.txt && ls -i file21.txt file22.txt && cat file21.txt file22.txt 

# 修改file21.txt的内容，并查看它们的inode号和内容，可以看到它们都发生了变化，且内容相同
echo "Hello, Linux!" > file21.txt && ls -i file21.txt file22.txt && cat file21.txt file22.txt 

# 修改file22.txt的内容，并查看它们的inode号和内容，可以看到它们都发生了变化，且内容相同
echo "Hello, ln!" > file22.txt && ls -i file21.txt file22.txt && cat file21.txt file22.txt 

# 修改file21.txt的名称，并查看它们的inode号和内容，可以看到只有file21.txt的名称发生了变化，但inode号和内容不变
mv file21.txt file23.txt && ls -i file23.txt file22.txt && cat file23.txt file22.txt 

# 修改file22.txt的名称，并查看它们的inode号和内容，可以看到只有file22.txt的名称发生了变化，但inode号和内容不变
mv file22.txt file24.txt && ls -i file23.txt file24.txt && cat file23.txt file24.txt
# 创建一个名为file25.txt的文件，并写入一些内容
echo "Hello, world!" > file25.txt

# 为file25.txt创建一个符号链接，名为file26.txt，并查看它们的inode号和内容
ln -s file25.txt file26.txt && ls -i file25.txt file26.txt && cat file25.txt file26.txt
```
