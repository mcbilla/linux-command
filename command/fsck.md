fsck
===

检查并且试图修复文件系统中的错误

## 补充说明

`fsck`，全称File System Consistency Check，主要用于检查和修复Linux文件系统的不一致和错误。该工具用于解决潜在的文件系统问题。`fsck`可以为你提供检查和修复一切文件系统中的问题的功能，包括一些潜在的磁盘错误等。

## Linux fsck命令适用的Linux版本

`fsck`命令在所有主流的Linux发行版，如Debian、Ubuntu、Alpine、Arch Linux、Kali Linux、RedHat/CentOS、Fedora、Raspbian等上都是默认安装的，属于Linux系统中的基础命令，无需另行安装。

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -V
```

不过对于一些基于轻量级Linux的系统，比如一些Docker容器，你可能需要使用包管理器进行安装，具体命令如下：

```bash
# For Debian/Ubuntu
[linux@bashcommandnotfound.cn ~]$ apt-get update
[linux@bashcommandnotfound.cn ~]$ apt-get install e2fsprogs

# For CentOS7/RHEL7
[linux@bashcommandnotfound.cn ~]$ yum update
[linux@bashcommandnotfound.cn ~]$ yum install e2fsprogs

# For CentOS8/RHEL8
[linux@bashcommandnotfound.cn ~]$ dnf update
[linux@bashcommandnotfound.cn ~]$ dnf install e2fsprogs
```

## 基本语法

```bash
fsck [options] [filesystem ...]
```

`options` 是fsck命令的选项，`filesystem` 是需要检查和修复的文件系统挂载点或设备名。

## Linux fsck命令的常用选项或参数说明

下表列出了`fsck`命令的常用选项：

| **选项** | **描述**                                                     |
| -------- | ------------------------------------------------------------ |
| -a       | 尝试自动修复文件系统错误。不会出现提示，因此请谨慎使用。     |
| -A       | 检查/etc/fstab中列出的所有文件系统。                         |
| -C       | 显示检查ext2和ext3文件系统的进度。                           |
| -F       | 强制fsck检查文件系统。该工具甚至在文件系统看起来正常时也进行检查。 |
| -l       | 锁定设备，以防止其他程序在扫描和修复期间使用该分区。         |
| -M       | 不要检查已挂载的文件系统。挂载文件系统时，该工具返回退出代码0。 |
| -N       | 做空试。输出显示fsck在不执行任何操作的情况下将执行的操作。警告或错误消息也将被打印。 |
| -P       | 用于在多个文件系统上并行运行扫描。请谨慎使用。               |
| -R       | 使用-A选项时，告诉fsck工具不要检查根文件系统。               |
| -r       | 打印设备统计信息。                                           |
| -t       | 指定要使用fsck检查的文件系统类型。请查阅手册页以获取详细信息。 |
| -T       | 工具启动时隐藏标题。                                         |
| -y       | 尝试在检查期间自动修复文件系统错误。                         |
| -V       | 详细输出。                                                   |

## Linux fsck命令实例详解

示例中将演示`fsck`命令如何在实际中被使用。

### 实例1：在开启系统时自动运行fsck检查

在Linux系统中，`fsck`工具在系统启动时会自动检查文件系统。不过，我们可以手动触发这种行为，以在下一次系统启动时运行fsck进行检查。可以通过创建一个叫做`forcefsck`的空文件来触发这样的检查：

```bash
[linux@bashcommandnotfound.cn ~]$ touch /forcefsck
```

以上命令在 `/` 目录下创建了一个名为 `forcefsck` 的空文件。在系统重新启动时，`fsck` 将会看到这个文件并进行文件系统的检查。

### 实例2：检查所有的文件系统

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -A
```

- `A`选项让fsck检查/etc/fstab定义的所有文件系统。

### 实例3：检查指定的文件系统

如果你只想检查一个特定的文件系统，可以在`fsck`命令后面加上设备名，如下所示：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck /dev/sda1
```

在上述例子中，`fsck`将检查 `/dev/sda1` 设备对应的文件系统。

### 实例4：在检查过程中显示详细信息

如果你想在文件系统检查过程中查看更详细的信息，可以使用`-V`选项，如下：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -V /dev/sda1
```

这将会在检查 `/dev/sda1` 设备时显示详细的信息。

### 实例5：进行交互式修复

默认情况下，`fsck`会在重大问题发生时提供交互式提示。如果你希望对所有发现的问题进行交互式修复，不仅仅是重大问题，可以使用`-r`选项，如下：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -r /dev/sda1
```

这将使`fsck`在对`/dev/sda1`进行检查时，对所有发现的问题提供交互式提示，让用户决定是否修复。

### 实例6：自动修复发现的问题

如果你不想进行任何交互，而期望`fsck`自动修复所有发现的问题，可以使用`-y`选项，如下：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -y /dev/sda1
```

这将使`fsck`在检查`/dev/sda1`设备时，自动修复所有发现的问题。

### 实例7：检查多个文件系统

如果你希望一次检查多个文件系统，可以在`fsck`命令后面列出所有的设备名，如下：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck /dev/sda1 /dev/sdb2
```

在上述例子中，`fsck`将依次检查`/dev/sda1`和`/dev/sdb2`设备对应的文件系统。对于所有设备，`fsck`将采取与单个设备时相同的检查策略。

### 实例8：只显示不做任何操作

如果你想看到`fsck`检查文件系统时的输出，但不希望进行任何实际的检查，可以使用`-N`选项：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -N /dev/sda1
```

在此例子中，`fsck`会显示出检查 `/dev/sda1` 文件系统时一般会做什么，但不会进行实际的检查。

### 实例9：检查多个文件系统并自动修复误差

当你需要检查多个文件系统并希望建立自动修复任何发现的问题，你可以这样做：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -y /dev/sda1 /dev/sdc2 /dev/sdd3
```

在这个例子中，`fsck`会依次检查`/dev/sda1`，`/dev/sdc2`和`/dev/sdd3`这三个文件系统，并自动修复所有发现的问题。

### 实例10：使用`C`选项进行可视化检查

- `C`选项将导致`fsck`为检查过程产生一个进度条。这个选项对于检查大型文件系统非常有用，因为你可以用它来跟踪`fsck`的进度。

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -C /dev/sda1
```

在上述例子中，在检查 `/dev/sda1` 设备时，`fsck` 会显示一个进度条来表示检查的进度。

### 实例11: 强制检查文件系统

在某些情况下，你可能想要强制对文件系统进行检查，即使它看起来是干净的。这可以通过`-f`选项实现：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -f /dev/sda1
```

在这个例子中，`fsck`将强制检查 `/dev/sda1`，即使它看起来是干净的。

### 实例12: 检查并试图修复有坏块的文件系统

如果你的文件系统出现坏块，你可以使用`-c`选项来检查并试图修复它：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -c /dev/sda1
```

在上述情况下，`fsck`将检查 `/dev/sda1`上的坏块，并尝试进行修复。

### 实例13: 预防性读取设备上的每一块以确保它们可读

你可以使用`-t`选项进行预防性读取，尝试读取所有块以检查它们是否可读：

```bash
[linux@bashcommandnotfound.cn ~]$ fsck -t /dev/sda1
```

在这个例子中，`fsck`将预防性地读取 `/dev/sda1` 的所有块，以确认它们是否可以正常读取。

## Linux fsck命令的注意事项

- 为了避免在fsck修复过程中由于错误选择而导致的数据损坏，你可以在第一次运行fsck时加上`n`或`N`选项。这样fsck将以只读模式运行，检查文件系统但不进行任何修复操作。
- `fsck`在修复文件系统错误时可能会造成数据丢失，因此请确保对所有的重要数据做好备份。
- `fsck`应该只在无法挂载文件系统，或者在系统开启时提示你运行`fsck`时才运行。否则，运行`fsck`可能会破坏文件系统并导致数据丢失。
- 如果在使用`fsck`时碰到 `bash: fsck: command not found` 错误，说明你的系统上可能没有安装 `fsck`，可以根据上面给出的命令进行安装。
