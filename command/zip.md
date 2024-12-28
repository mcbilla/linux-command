zip
===

压缩文件生成zip文件

## 补充说明

zip命令在Linux中使用，用于压缩和打包文件。此命令可提取ZIP存档中的文件，并更新或删除ZIP 存档中的文件。它支持不同的压缩方法、级别和加密选项。您还可以创建分割的Zip文件和密码保护的ZIP文件。

## 适用的Linux版本

Zip命令适用于大多数Linux发行版本，包括但不限于Ubuntu、Debian、Fedora、CentOS等。在一些Linux发行版本中，可能需要手动安装zip和unzip命令。在CentOS中，可以使用以下命令进行安装：

```shell
$ sudo yum install zip unzip     #For CentOS 7
$ sudo dnf install zip unzip     #For CentOS 8
$ sudo apt-get install zip unzip #Ubuntu、Debian
```

## 命令语法

```shell
zip [options] archive_name file_name
```

其中，options 是命令行选项（如 -r 用于递归压缩），archive_name 是要创建的存档文件的名称，file_name 是要添加到存档中的文件名。

## 选项

| 选择 | 描述                                   |
| ---- | -------------------------------------- |
| -r   | 递归压缩，在压缩目录时需要使用         |
| -m   | 压缩后移除原文件。                     |
| -x   | 排除指定文件。                         |
| -e   | 创建密码保护的压缩包。                 |
| -q   | 安静模式，压缩过程中不在屏幕显示消息。 |

## 示例

### 简单的压缩文件

创建一个名为archive.zip的压缩文件，其中包含当前目录中的所有文件：

```shell
$ zip archive *
```

### 使用-r选项压缩目录

使用 -r 选项可以递归地压缩目录：

```shell
$ zip -r archive dir/
```

注：在此命令中，archive 是新建存档的名称，dir/ 是要压缩的目录。

### 创建密码保护的ZIP文件

我们可以使用 -e 选项让zip命令提示您输入密码。以下命令会创建一个受密码保护的zip文件：

```shell
$ zip -e archive.zip file1 file2
```

在此命令中, file1 和 file2 是要添加到存档中的文件，zip命令会提示您两次输入密码。

### 排除特定文件

想压缩一个目录，但希望排除某些文件或目录，请使用-x选项。比如，以下命令将压缩 dir 目录下的所有文件和目录，但将排除所有 .txt 和 .doc 文件：

```shell
$ zip -r archive.zip dir/ -x *.txt *.doc
```

此处的 -x 选项后面跟随的是一个模式列表，用于指定要排除的文件。

### 压缩多个文件和目录

如果你要在一个命令中压缩多个文件和目录，可以将它们全部列在命令后面，如下所示：

```shell
$ zip archive.zip file1 dir1 file2 dir2
```

在这个命令中， file1, dir1, file2, 和 dir2 是要添加到 zip 存档中的文件和目录。

### 使用-q选项

当你急于获得操作结果，在压缩过程中不希望屏幕显示任何消息，可以使用-q选项。例如：

```shell
$ zip -q archive.zip file1 file2
```

在上面的命令中，zip命令将不会输出任何消息，只有在发生错误时，例如存档已存在或找不到文件，它将显示错误消息。

### 使用-m选项

想要在压缩文件后移除原文件，可以使用 -m 选项。比如：

```shell
$ zip -m archive.zip file1 file2
```

此命令将创建一个名为 archive.zip 的新的zip文件，并从文件系统中删除 file1 和 file2。

### 使用 -u 选项更新现有的ZIP文件

如果已有一个zip文件，你想添加新的文件或更新已存在的文件，可以使用-u选项。例如：

```shell
$ zip -u archive.zip file1
```

在这个命令中，file1将被添加到archive.zip中，如果archive.zip中已有一个叫做file1的文件，它将被新的file1替代。

### 从ZIP文件中删除文件

要从zip文件中删除特定的文件，可以使用-d选项。例如：

```shell
$ zip -d archive.zip file1
```

在这个命令中，file1将从archive.zip中被删除。

### 创建分割的ZIP文件

如果你要创建一个较大的Zip文件，可以把它分割成多个较小的部分。你可以使用-s选项。比如要创建一个最大部分为5MB的zip文件：

```shell
$ zip -s 5m -r split.zip dir1/
```

在这个命令中，dir1/是你想要压缩的目录，split.zip 将被分割成多个5MB的部分。

### 查看ZIP文件的信息

有时,我们可能需要检查ZIP文件中包含的文件和目录。可以使用-l选项查看archive.zip中的文件列表：

```shell
$ zip -sf archive.zip
```

### 压缩文件并附加注释

zip命令允许在压缩文件时添加注释。仅需使用-z选项，如下所示：

```shell
$ zip -z archive.zip file1 file2
```

运行命令后，命令行将提示您输入注释。输入注释后，按CTRL + D保存并退出。

