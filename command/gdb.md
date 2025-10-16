gdb
===

功能强大的程序调试器

## 补充说明

**gdb命令**全称是 **GNU Debugger**，它是 GNU 开源系统下，也是 Linux/Unix 系统中最常用、最强大的程序调试工具。简单来说，**GDB 是一个用来帮助你寻找程序中的 Bug（错误）的命令行工具。**

使用 GDB，你可以做以下事情：

1. **启动程序**：可以指定参数来运行你的程序。
2. **设置断点**：让程序在指定的位置（如某一行代码、某个函数）暂停执行。
3. **单步执行**：让程序一行一行地执行代码，便于观察执行流程。
4. **查看变量**：在程序暂停时，查看变量的值，判断其是否符合预期。
5. **查看内存**：检查特定内存地址的内容。
6. **分析核心转储**：当程序崩溃时，可以通过分析产生的核心转储文件来定位问题。
7. **修改变量**：在调试过程中动态修改变量的值，用于测试不同场景。

## 适用的Linux版本

```
#Debian
apt-get install gdb
 
#Ubuntu
apt-get install gdb
 
#Alpine
apk add gdb
 
#Arch Linux
pacman -S gdb
 
#Kali Linux
apt-get install gdb
 
#CentOS
yum install gdb
 
#Fedora
dnf install gdb
 
#OS X
brew install gdb
 
#Raspbian
apt-get install gdb
 
#Docker
docker run cmd.cat/gdb gdb
```

## 语法

```shell
gdb (选项) (二进制可执行文件)
```

## 选项

```shell
-cd：设置工作目录；
-q：安静模式，不打印介绍信息和版本信息；
-d：添加文件查找路径；
-x：从指定文件中执行GDB指令；
-s：设置读取的符号表文件。
```

进入 gdb 调试界面后，可以使用以下命令

#### 1. 启动与退出

| 命令                         | 全称/说明                   | 示例                              |
| :--------------------------- | :-------------------------- | :-------------------------------- |
| `gdb <程序名>`               | 启动 GDB 并加载可执行文件   | `gdb my_program`                  |
| `gdb --args <程序名> <参数>` | 启动 GDB 并加载带参数的程序 | `gdb --args my_program arg1 arg2` |
| `file <程序名>`              | 在 GDB 内部加载可执行文件   | `(gdb) file my_program`           |
| `run` 或 `r`                 | 运行程序                    | `(gdb) run`                       |
| `quit` 或 `q`                | 退出 GDB                    | `(gdb) quit`                      |

#### 2. 断点管理

| 命令                           | 全称/说明                | 示例                                                         |
| :----------------------------- | :----------------------- | :----------------------------------------------------------- |
| `break <位置>` 或 `b <位置>`   | 在指定位置设置断点       | `(gdb) b main` (在 main 函数开始处断点) <br />`(gdb) b 10` (在第 10 行断点) <br />`(gdb) b my_function` (在 my_function 函数处断点) |
| `info breakpoints` 或 `info b` | 显示所有已设置的断点信息 | `(gdb) info b`                                               |
| `delete <断点编号>`            | 删除指定编号的断点       | `(gdb) delete 1` (删除 1 号断点)                             |
| `delete`                       | 删除所有断点             | `(gdb) delete`                                               |

#### 3. 执行控制

| 命令              | 全称/说明                                    | 示例             |
| :---------------- | :------------------------------------------- | :--------------- |
| `continue` 或 `c` | 继续运行程序，直到下一个断点或程序结束       | `(gdb) continue` |
| `next` 或 `n`     | **单步执行**，**不进入**函数内部（跳过函数） | `(gdb) next`     |
| `step` 或 `s`     | **单步执行**，**进入**函数内部               | `(gdb) step`     |
| `finish`          | 执行完当前函数，并返回到调用它的地方         | `(gdb) finish`   |

#### 4. 查看代码与数据

| 命令                             | 全称/说明                                                    | 示例                                                         |
| :------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `list` 或 `l`                    | 显示当前位置周围的源代码                                     | `(gdb) list`                                                 |
| `print <表达式>` 或 `p <表达式>` | 打印变量或表达式的值                                         | `(gdb) p variable` <br />`(gdb) p array[i]`<br /> `(gdb) p x + y` |
| `display <表达式>`               | 每次程序暂停时，自动显示指定表达式的值                       | `(gdb) display counter`                                      |
| `info locals`                    | 显示当前函数的所有局部变量                                   | `(gdb) info locals`                                          |
| `backtrace` 或 `bt`              | 显示函数调用栈（非常有用，可以知道程序是如何执行到当前位置的） | `(gdb) bt`                                                   |
| `frame <帧编号>`                 | 切换到调用栈的指定帧                                         | `(gdb) frame 1`                                              |

## 实例

### 编译程序

为了能有效地使用 GDB，你需要在**编译程序时加上 `-g` 选项**。这个选项会在可执行文件中嵌入调试信息（如变量名、函数名、行号等），这样 GDB 才能将机器指令与你写的源代码对应起来。

**示例：**

```
gcc -g -o my_program my_program.c
```

这样会生成一个带有调试信息的 `my_program` 可执行文件。

### 一个完整的简单调试示例

假设我们有一个有问题的 C 程序 `test.c`：

```c
#include <stdio.h>

int main() {
    int i;
    int sum = 0;
    for (i = 1; i <= 10; i++) {
        sum += i;
    }
    printf("Sum from 1 to 10 is: %d\n", sum);
    return 0;
}
```

（虽然这个程序本身没问题，但我们可以演示调试过程）

1. **编译并启动 GDB**

   ```c
   gcc -g -o test test.c
   gdb ./test
   ```

2. **在 main 函数设置断点并运行**

   ```c
   (gdb) b main
   Breakpoint 1 at 0x114d: file test.c, line 4.
   (gdb) run
   Starting program: /path/to/test
   
   Breakpoint 1, main () at test.c:4
   4       int sum = 0;
   ```

3. **单步执行并查看变量**

   ```c
   (gdb) n  # 执行 int sum = 0;
   5       for (i = 1; i <= 10; i++) {
   (gdb) n  # 进入 for 循环
   6           sum += i;
   (gdb) p i  # 查看 i 的值
   $1 = 1
   (gdb) p sum # 查看 sum 的值
   $2 = 0
   ```

4. **继续执行几次循环**

   ```c
   (gdb) c
   Continuing.
   # 程序会运行，直到下一个断点。但我们没有其他断点，所以会直接跑完。
   # 如果我们在循环里设置了断点，它会每次循环都停一下。
   ```

5. **退出 GDB**

   ```
   (gdb) quit
   ```
