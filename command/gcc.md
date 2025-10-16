gcc
===

基于C/C++的编译器

## 补充说明

**gcc命令** 使用GNU推出的基于 `C/C++` 的编译器，是开放源代码领域应用最广泛的编译器，具有功能强大，编译代码支持性能优化等特点。现在很多程序员都应用 `GCC`，怎样才能更好的应用 `GCC`。目前，`GCC` 可以用来编译 `C/C++`、`FORTRAN`、`JAVA`、`OBJC`、`ADA`等语言的程序，可根据需要选择安装支持的语言。

## 适用的Linux版本

- **Ubuntu/Debian:**

  ```
  sudo apt update
  sudo apt install gcc g++   # gcc 用于 C， g++ 用于 C++
  ```

- **CentOS/RHEL/Fedora:**

  ```
  sudo yum install gcc gcc-c++   # 对于较老的 CentOS/RHEL
  # 或者
  sudo dnf install gcc gcc-c++   # 对于 Fedora 和较新的 RHEL/CentOS
  ```

- **macOS:**
  安装 Xcode Command Line Tools：

  ```
  xcode-select --install
  ```

安装完成后，可以通过以下命令验证版本：

```
gcc --version
g++ --version
```

##  语法

```shell
gcc (选项) (C源文件)
```

##  选项

| 选项              | 说明                                                         |
| :---------------- | :----------------------------------------------------------- |
| `-o <file>`       | 指定输出文件名。                                             |
| `-c`              | 只编译和汇编，生成目标文件，不链接。                         |
| `-E`              | 只进行预处理。                                               |
| `-S`              | 只进行预处理和编译，生成汇编代码。                           |
| `-g`              | **在可执行文件中包含调试信息**，便于使用 GDB 进行调试。**开发时强烈建议加上。** |
| `-O<level>`       | **优化级别**。`-O0`（不优化），`-O1`，`-O2`（常用，较好的优化），`-O3`（激进优化），`-Os`（优化代码大小）。 |
| `-Wall`           | **开启大部分常用的警告**。强烈建议始终使用，可以帮助你发现代码中的潜在问题。 |
| `-Wextra`         | 开启额外的警告。                                             |
| `-Werror`         | 将所有警告视为错误。                                         |
| `-std=<standard>` | **指定语言标准**。如 `-std=c99`, `-std=c11`, `-std=c++11`, `-std=c++14`, `-std=c++17` 等。 |
| `-I<dir>`         | **指定头文件的搜索目录**。                                   |
| `-L<dir>`         | **指定库文件的搜索目录**。                                   |
| `-l<library>`     | **链接时指定库**。例如 `-lm` 链接数学库。                    |
| `-D<macro>`       | **在命令行定义宏**。例如 `-DDEBUG` 相当于在代码中写 `#define DEBUG`。 |

##  实例

#### 最简单的编译

假设我们有一个 `hello.c` 文件：

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

要编译它，只需执行：

```
gcc hello.c
```

GCC 会执行上述四个步骤，并默认生成一个名为 `a.out` 的可执行文件。运行它：

```
./a.out
# 输出：Hello, World!
```

#### 指定输出文件名：`-o` 选项

使用 `-o` 选项可以指定生成的可执行文件的名称。

```
gcc hello.c -o hello
```

这将生成一个名为 `hello` 的可执行文件。运行它：

```
./hello
```

对于 C++ 程序（如 `hello.cpp`），使用 `g++` 命令，用法完全相同：

```
g++ hello.cpp -o hello_cpp
```

#### 预处理：`-E`

只进行预处理，将结果输出到标准输出。通常重定向到 `.i` 文件。

```
gcc -E hello.c -o hello.i
# 查看 hello.i 文件，你会看到所有头文件都被展开，宏也被替换。
```

#### 编译为汇编代码：`-S`

进行预处理和编译，生成汇编代码文件。

```
gcc -S hello.c
# 或者 gcc -S hello.c -o hello.s
```

这会生成一个 `hello.s` 文件。

#### 编译为目标文件：`-c`

进行预处理、编译和汇编，但不链接。生成 `.o` 目标文件。

```
gcc -c hello.c
# 或者 gcc -c hello.c -o hello.o
```

这在项目开发中非常常用，可以分别编译每个源文件，最后再统一链接。

#### 手动链接目标文件

如果你有多个 `.o` 文件，可以将它们链接在一起：

```
gcc file1.o file2.o file3.o -o myprogram
```

## 高级应用

#### 示例 1：编译多个源文件

假设你有 `main.c`, `utils.c`, `helper.c`。

**方法一：一次性编译**

```
gcc -g -Wall -I./include main.c utils.c helper.c -o myapp
```

**方法二：分步编译（推荐用于大型项目）**
这种方法只重新编译修改过的文件，效率更高。

```
# 1. 分别编译每个源文件为目标文件
gcc -g -Wall -I./include -c main.c -o main.o
gcc -g -Wall -I./include -c utils.c -o utils.o
gcc -g -Wall -I./include -c helper.c -o helper.o

# 2. 链接所有目标文件生成可执行文件
gcc main.o utils.o helper.o -o myapp
```

#### 示例 2：链接外部库

假设你的程序使用了数学库 `libm`。

数学库的函数声明在 `math.h` 中，但其实现是在一个名为 `libm.so`（动态库）或 `libm.a`（静态库）的文件中。

编译命令需要加上 `-lm`：

```
gcc -g -Wall my_math_program.c -o my_math_program -lm
```

**注意：** `-l` 选项后跟库名，但需要去掉前缀 `lib` 和后缀 `.so/.a`。所以 `libm.so` 就是 `-lm`。

如果库不在标准路径下，还需要用 `-L` 指定库的路径：

```
gcc -g -Wall -I/path/to/headers my_program.c -o my_program -L/path/to/libs -lmycustomlib
```

## 静态库与动态库

GCC 可以创建和使用这两种库。

#### 创建静态库 (`.a`)

静态库在**链接时**被完整地复制到可执行文件中。

1. **将源文件编译成目标文件：**

   ```
   gcc -c helper1.c -o helper1.o
   gcc -c helper2.c -o helper2.o
   ```

2. **使用 `ar` 命令打包目标文件创建静态库：**

   ```
   ar rcs libmylib.a helper1.o helper2.o
   ```

3. **使用静态库：**

   ```
   gcc main.c -L. -lmylib -o myapp_static
   ```

#### 创建动态库 (`.so`, Shared Object)

动态库在**程序运行时**才被加载。

1. **编译源文件，生成位置无关代码 (`-fPIC`)：**

   ```
   gcc -c -fPIC helper1.c -o helper1.o
   gcc -c -fPIC helper2.c -o helper2.o
   ```

2. **使用 `-shared` 选项创建动态库：**

   ```
   gcc -shared -o libmylib.so helper1.o helper2.o
   ```

3. **使用动态库：**

   ```
   gcc main.c -L. -lmylib -o myapp_shared
   ```

   运行 `myapp_shared` 前，需要确保系统能找到 `libmylib.so`，可以通过设置 `LD_LIBRARY_PATH` 环境变量实现：

   ```
   export LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH
   ./myapp_shared
   ```
