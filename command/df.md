df
===

显示磁盘的空间使用情况

## 补充说明

df命令（英文全拼：disk free）用于显示目前在Linux系统上的文件系统磁盘使用情况统计。它可以帮助我们了解文件系统的磁盘使用情况，包括已使用的磁盘空间、可用的磁盘空间等。默认显示单位为KB。

## 适用的Linux版本

df命令在大多数Linux发行版中都可以使用，包括但不限于Ubuntu, Debian, Fedora, CentOS等。如果你发现你的系统中没有df命令，你可以尝试使用你的包管理器来安装它。例如，在基于Debian的系统中，你可以使用以下命令来安装：

```shell
$ sudo apt-get -y install coreutils
```

在基于RHEL的系统中，你可以使用以下命令来安装：

```shell
$ sudo yum -y install coreutils
```

在centos8中：

```shell
$ sudo dnf -y install coreutils
```

##  命令语法

```shell
df [options] [文件/目录]
```

##  选项

```shell
-a或--all：包含全部的文件系统；
--block-size=<区块大小>：以指定的区块大小来显示区块数目；
-h或--human-readable：以可读性较高的方式来显示信息；
-H或--si：与-h参数相同，但在计算时是以1000 Bytes为换算单位而非1024 Bytes；
-i或--inodes：显示inode的信息；
-k或--kilobytes：指定区块大小为1024字节；
-l或--local：仅显示本地端的文件系统；
-m或--megabytes：指定区块大小为1048576字节；
--no-sync：在取得磁盘使用信息前，不要执行sync指令，此为预设值；
-P或--portability：使用POSIX的输出格式；
--sync：在取得磁盘使用信息前，先执行sync指令；
-t<文件系统类型>或--type=<文件系统类型>：仅显示指定文件系统类型的磁盘信息；
-T或--print-type：显示文件系统的类型；
-x<文件系统类型>或--exclude-type=<文件系统类型>：不要显示指定文件系统类型的磁盘信息；
--help：显示帮助；
--version：显示版本信息。
```

显示值以 `--block-size` 和 `DF_BLOCK_SIZE`，`BLOCK_SIZE` 和 `BLOCKSIZE` 环境变量中的第一个可用 `SIZE` 为单位。 否则，单位默认为 `1024` 个字节（如果设置 `POSIXLY_CORRECT`，则为`512`）。

SIZE是一个整数和可选单位（例如：10M是10 * 1024 * 1024）。 单位是K，M，G，T，P，E，Z，Y（1024的幂）或KB，MB，...（1000的幂）。

## 示例

### 查看系统磁盘设备（默认是KB为单位）

```shell
$ df
文件系统               1K-块        已用     可用 已用% 挂载点
/dev/sda2            146294492  28244432 110498708  21% /
/dev/sda1              1019208     62360    904240   7% /boot
tmpfs                  1032204         0   1032204   0% /dev/shm
/dev/sdb1            2884284108 218826068 2518944764   8% /data1
```

### 以人类易读的方式显示

```shell
$ df -h
文件系统              容量  已用 可用 已用% 挂载点
/dev/sda2             140G   27G  106G  21% /
/dev/sda1             996M   61M  884M   7% /boot
tmpfs                1009M     0 1009M   0% /dev/shm
/dev/sdb1             2.7T  209G  2.4T   8% /data1
```

### 查看全部文件系统

```shell
$ df -a
文件系统               1K-块        已用     可用 已用% 挂载点
/dev/sda2            146294492  28244432 110498708  21% /
proc                         0         0         0   -  /proc
sysfs                        0         0         0   -  /sys
devpts                       0         0         0   -  /dev/pts
/dev/sda1              1019208     62360    904240   7% /boot
tmpfs                  1032204         0   1032204   0% /dev/shm
/dev/sdb1            2884284108 218826068 2518944764   8% /data1
none                         0         0         0   -  /proc/sys/fs/binfmt_misc
```

### 查看指定目录所在的文件系统

```shell
$ df /usr/tmp
Filesystem     1K-blocks     Used Available Use% Mounted on
/dev/sda1       41921540 15400324  26521216  37% /
```



