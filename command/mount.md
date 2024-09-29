mount
===

用于挂载Linux系统外的文件

## 补充说明

mount 命令的全称是mount a filesystem，它的功能是将一个文件系统或设备连接到一个已存在的目录上，这个目录称为挂载点（mount point）。

> **挂载指的是将设备文件中的顶级目录连接到 Linux 根目录下的某一目录（最好是空目录），访问此目录就等同于访问设备文件**。如果不挂载，通过 Linux 系统中的图形界面系统可以查看找到硬件设备，但命令行方式无法找到。
>
> 例如当我们把 U 盘插入 Linux 后，系统也确实会给 U 盘分配一个目录文件（比如 `sdb1`），就位于 `/dev/` 目录下（`/dev/sdb1`），因为根目录下的 `/dev/` 目录文件负责所有的硬件设备文件。但此时无法通过 `/dev/sdb1/` 直接访问 U 盘数据，访问此目录只会提供给你此设备的一些基本信息（比如容量）。我们要访问 U 盘内部目录，必须将 U 盘件与已有目录文件进行挂载。

挂载后，原来挂载点的内容将被隐藏，而文件系统或设备的内容将出现在挂载点上。挂载的文件系统或设备可以是本地的，也可以是远程的，例如NFS（网络文件系统）或SMB（服务器消息块）。

并不是根目录下任何一个目录都可以作为挂载点，由于挂载操作会使得原有目录中文件被隐藏，因此根目录以及系统原有目录都不要作为挂载点，会造成系统异常甚至崩溃，挂载点最好是新建的空目录。

mount 命令可以手动执行，也可以通过配置 `/etc/fstab`文件来自动执行。 `/etc/fstab` 文件是一个文本文件，它记录了系统中的所有可挂载的文件系统或设备，以及它们的挂载点和挂载选项。mount 命令可以根据这个文件来挂载所有或部分的文件系统或设备。

mount 命令还可以用来查询当前已挂载的文件系统或设备的信息，例如类型、选项、大小等。这些信息可以帮助你了解系统的磁盘使用情况和性能。

## Linux mount命令适用的Linux版本

mount 命令是一个通用的 Linux 命令，它可以在多数 Linux 发行版（如Debian、Ubuntu、Alpine、Arch Linux、Kali Linux、RedHat/CentOS、Fedora、Raspbian）的主要终端命令解释器（包括bash、zsh、csh、ksh、fish、tcsh）中使用。不同的Linux发行版可能支持不同的文件系统类型，例如ext4、xfs、btrfs等。你可以使用-t选项来指定要挂载的文件系统类型，也可以省略这个选项，让 mount 命令自动检测文件系统类型。

如果你的 Linux 系统没有安装 mount 命令，你可以使用以下命令来安装它：

- Ubuntu或Debian系统：

```bash
[linux@bashcommandnotfound.cn ~]$ sudo apt update
[linux@bashcommandnotfound.cn ~]$ sudo apt install mount
```

- Fedora或Red Hat系统：

```bash
[linux@bashcommandnotfound.cn ~]$ sudo dnf update
[linux@bashcommandnotfound.cn ~]$ sudo dnf install mount
```

- CentOS 7系统：

```bash
[linux@bashcommandnotfound.cn ~]$ sudo yum update
[linux@bashcommandnotfound.cn ~]$ sudo yum install mount
```

- CentOS 8系统：

```bash
[linux@bashcommandnotfound.cn ~]$ sudo dnf update
[linux@bashcommandnotfound.cn ~]$ sudo dnf install mount
```

## 基本语法

mount命令的基本语法格式如下：

```bash
mount [选项] ... 设备 | 目录
mount [选项] ... -t 类型 [-o 选项] 设备 目录
```

- 第一种格式是用来查询已挂载的文件系统或设备的信息，如果不指定任何选项或参数，就会显示所有已挂载的文件系统或设备。如果指定了一个设备或目录，就会显示与之相关的挂载信息。
- 第二种格式是用来挂载一个文件系统或设备到一个目录上，必须指定一个设备和一个目录，可以使用-t选项来指定文件系统类型，也可以使用-o选项来指定挂载选项，例如只读、同步等。

## 选项

mount 命令有很多选项，可以用来控制挂载的行为和效果。以下是一些常用的选项：

| 选项            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| -a              | 挂载 /etc/fstab 文件中列出的所有文件系统或设备               |
| -f              | 模拟挂载操作，不实际执行，用于测试和调试，通常会和 -v 一起使用。 |
| -h              | 显示帮助信息                                                 |
| -l              | 显示已挂载的文件系统或设备的标签                             |
| -L 标签         | 挂载指定标签的文件系统或设备                                 |
| -n              | 不更新 /etc/mtab 文件，用于只读文件系统或设备。在默认情况下，系统会将实际挂载的情况实时写入 /etc/mtab 文件中，但在某些场景下（例如单人维护模式），为了避免出现问题，会刻意不写入，此时就需要使用这个选项； |
| -r              | 以只读模式挂载文件系统或设备                                 |
| -t 文件系统类型 | 挂载指定类型的文件系统或设备。Linux 常见的支持类型有 EXT2、EXT3、EXT4、iso9660（光盘格式）、vfat、reiserfs 等。如果不指定具体类型，挂载时 Linux 会自动检测。 |
| -u              | 卸载指定的文件系统或设备                                     |
| -v              | 显示详细的挂载信息，通常和 -f 用来除错。                     |
| -V              | 显示版本信息                                                 |
| -F              | 这个命令通常和 -a 一起使用，它会为每一个 mount 的动作产生一个行程负责执行。在系统需要挂上大量 NFS 档案系统时可以加快挂上的动作。 |
| -o 选项         | 指定挂载选项，可以是一个或多个，用逗号分隔，例如ro,sync      |

-o 支持的特殊选项如下所示：

| 选项        | 功能                                                         |
| ----------- | ------------------------------------------------------------ |
| rw/ro       | 是否对挂载的文件系统拥有读写权限，rw 为默认值，表示拥有读写权限；ro 表示只读权限。 |
| async/sync  | 此文件系统是否使用同步写入（sync）或异步（async）的内存机制，默认为异步 async。 |
| dev/nodev   | 是否允许从该文件系统的 block 文件中提取数据，为了保证数据安装，默认是 nodev。 |
| auto/noauto | 是否允许此文件系统被以 mount -a 的方式进行自动挂载，默认是 auto。 |
| suid/nosuid | 设定文件系统是否拥有 SetUID 和 SetGID 权限，默认是拥有。     |
| exec/noexec | 设定在文件系统中是否允许执行可执行文件，默认是允许。         |
| user/nouser | 设定此文件系统是否允许让普通用户使用 mount 执行实现挂载，默认是不允许（nouser），仅有 root 可以。 |
| defaults    | 定义默认值，相当于 rw、suid、dev、exec、auto、nouser、async 这 7 个选项。 |
| remount     | 重新挂载已挂载的文件系统，一般用于指定修改特殊权限。         |

## 示例

以下是一些使用mount命令的实例，你可以根据自己的需求和环境来修改和尝试。

### 显示所有已挂载的文件系统或设备的信息

```bash
$ mount
```

### 显示指定目录的挂载信息

```bash
$ mount /home
```

### 显示指定类型的文件系统或设备的信息

```bash
$ mount -t ext4
```

### 挂载一个本地分区到一个目录上

```bash
# 我们可以使用 mkfs 命令把磁盘分区格式化为指定的文件系统，比如 ext4
$ mkfs -t ext4 /dev/sda1

然后把该分区挂载到 /mnt 目录
$ sudo mount /dev/sda1 /mnt
```

### 挂载一个远程NFS服务器上的目录到一个本地目录上

```bash
$ sudo mount -t nfs 192.168.1.100:/data /mnt
```

### 挂载一个USB闪存盘到一个目录上

```bash
$ sudo mount /dev/sdb1 /media/usb
```

### 挂载一个CD-ROM到一个目录上

```bash
$ sudo mount /dev/cdrom /media/cdrom
```

### 挂载一个ISO文件到一个目录上

```bash
$ sudo mount -o loop image.iso /media/iso
```

### 以只读模式挂载一个文件系统或设备

```bash
$ sudo mount -r /dev/sda1 /mnt
```

### 使用指定的挂载选项挂载一个文件系统或设备

```bash
$ sudo mount -o ro,sync /dev/sda1 /mnt
```

### 挂载 /etc/fstab 文件中列出的所有文件系统或设备

```bash
$ sudo mount -a
```

### 卸载一个已挂载的文件系统或设备

```bash
$ sudo umount /mnt
```

### 卸载一个已挂载的文件系统或设备，不管它是否正在使用

```bash
$ sudo umount -f /mnt
```

### 卸载一个已挂载的文件系统或设备，如果它正在使用，就延迟卸载，直到它不再使用

```bash
$ sudo umount -l /mnt
```

## Linux mount命令的注意事项

- mount 命令通常需要 root 权限或 sudo 权限才能执行，除非你在 `/etc/fstab` 文件中指定了用户可以挂载的文件系统或设备。
- mount 命令会更新 /etc/mtab 文件，这个文件记录了当前已挂载的文件系统或设备的信息。如果你的系统是只读的，或者你不想更新这个文件，你可以使用-n选项来禁止更新。
- mount 命令会检查文件系统或设备的完整性，如果发现有错误，它会尝试修复或忽略。如果你不想让mount命令做这些检查，你可以使用 -o norecovery 选项来禁止。
- 如果你尝试挂载一个不存在或不可用的文件系统或设备，你可能会遇到`bash: mount: command not found`的错误。这时，你需要检查你的文件系统或设备是否正确连接和识别，或者你是否安装了mount命令。你可以使用`lsblk`命令来查看你的文件系统或设备的信息，或者使用`which mount`命令来查看你是否有mount命令。
- 如果你尝试挂载一个已经被挂载的文件系统或设备，你可能会遇到`mount: /mnt: /dev/sda1 already mounted on /mnt.`的错误。这时，你需要先卸载这个文件系统或设备，或者选择一个不同的挂载点。
- 如果你尝试卸载一个正在使用的文件系统或设备，你可能会遇到`umount: /mnt: target is busy.`的错误。这时，你需要先关闭所有使用这个文件系统或设备的进程，或者使用-f或-l选项来强制或延迟卸载。

