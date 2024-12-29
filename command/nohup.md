nohup
===

将程序以忽略挂起信号的方式运行起来

## 补充说明

**nohup命令** 可以将程序以忽略挂起信号的方式运行起来，被运行的程序的输出信息将不会显示到终端。

无论是否将 nohup 命令的输出重定向到终端，输出都将附加到当前目录的 nohup.out 文件中。如果当前目录的 nohup.out 文件不可写，输出重定向到`$HOME/nohup.out`文件中。如果没有文件能创建或打开以用于追加，那么 command 参数指定的命令不可调用。如果标准错误是一个终端，那么把指定的命令写给标准错误的所有输出作为标准输出重定向到相同的文件描述符。

### nohup和&的区别

在Linux中，信号是进程间通讯的一种方式，它采用的是异步机制。当信号发送到某个进程中时，操作系统会中断该进程的正常流程，并进入相应的信号处理函数执行操作，完成后再回到中断的地方继续执行。

涉及程序后台运行的部分信号列表如下：

| 信号    | 编号 | 解释 | 说明                           |
| ------- | ---- | ---- | ------------------------------ |
| SIGHUP  | 1    | 挂起 | 终端控制进程结束(终端连接断开) |
| SIGINT  | 2    | 中断 | 用户发送INTR字符(Ctrl+C)触发   |
| SIGQUIT | 3    | 退出 | 用户发送QUIT字符(Ctrl+/)触发   |

使用`nohup`执行程序

- 结果默认会输出到 nohup.out
- 免疫 SIGHUP 信号，关闭 shell 后仍会继续执行
- 可以使用 SIGINT 信号终止，执行 Ctrl+C 命令后停止执行

使用`&`执行程序

- 结果会输出到终端
- 对 SIGINT 信号免疫，输入 Ctrl+C 命令仍会继续执行
- 可以使用 SIGHUP 信号终止，关掉 Shell 后停止执行

要让进程真正不受 Shell 中 Ctrl+C 和 Shell 关闭的影响，用法如下，默认会输出到当前目录的 nohup.out 文件

```shell
nohup command &
```

##  命令语法

```shell
 nohup Command [ Arg … ] [　& ]
```

* **Command**：要执行的命令。

* **Arg**：一些参数，可以指定输出文件。

* **&**：让命令在后台执行，终端退出后命令仍旧执行。

##  选项

```shell
--help：在线帮助；
--version：显示版本信息。
```

##  示例

### nohup使用

使用nohup命令提交作业，如果使用nohup命令提交作业，那么在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中

```shell
$ nohup vmstat 2 10
nohup: ignoring input and appending output to ‘nohup.out’
```

上面命令会在同一个目录下生成一个名称为 `nohup.out` 的文件，其中包含了正在运行的程序的输出内容，并且不受 Shell 关闭的影响。

### nohup和&使用

```shell
$ nohup vmstat 2 10 &
[1] 19341
$ nohup: ignoring input and appending output to ‘nohup.out’
```

上面命令会在同一个目录下生成一个名称为 `nohup.out` 的文件，其中包含了正在运行的程序的输出内容，并且不受 Ctrl+C 和 Shell 关闭的影响。

### nohup和&使用，重定向文件

```shell
nohup command > myout.file 2>&1 &
```

在上面的例子中，输出被重定向到 `myout.file` 文件中。
