sudo
===

以其他身份来执行命令

## 补充说明

**sudo命令** 用来以其他身份来执行命令，预设的身份为 root。我们知道，使用 su 命令可以让普通用户切换到 root 身份去执行某些特权命令，但存在一些安全性的问题：仅仅为了一个特权操作就直接赋予普通用户控制系统的完整权限。

考虑到使用 su 命令可能对系统安装造成的隐患，最常见的解决方法是使用 sudo 命令。sudo 命令可以让用户不用切换到 root 身份就可以执行 root 身份下才能执行的命令，也就是说，经由 sudo 所执行的指令就好像是 root 亲自执行。另外，sudo 也可以实现切换至其他用户的身份（包括 root）的功能。

能否使用 sudo 命令，取决于对 /etc/sudoers 文件的配置（默认情况下，此文件中只配置有 root 用户）。

## 适用的Linux版本

sudo命令适用于大多数现代的Linux发行版，比如RedHat, CentOS, Debian, Ubuntu等。但是，不同的发行版可能有不同的方式来管理sudo用户。一般来说，一个用户要想使用sudo命令，就必须属于sudo, sudoers或者wheel这三个组中的一个。默认情况下，单用户系统会给它的用户赋予sudo权限。多用户系统或者服务器可能会排除一些用户的sudo权限。我们建议只给那些必须要执行sudo命令的用户赋予相应的权限。以下是如何给一个用户添加到sudoers组的方法：

- RedHat和CentOS：在这两个发行版中，wheel组控制着sudo用户。要给一个用户添加到wheel组，可以使用以下命令：

```shell
$ sudo usermod -aG wheel [username]
```

其中[username]是要添加的用户名。你可能需要以管理员身份登录或者使用su命令。

* Debian和Ubuntu：在这两个发行版中，sudo组控制着sudo用户。要给一个用户添加到sudo组，可以使用以下命令：

```shell
$ sudo usermod -aG sudo [username]
```

* 使用visudo命令编辑sudoers组：在一些现代的Linux版本中，用户可以通过编辑 /etc/sudoers 文件来赋予sudo权限。修改 /etc/sudoers，不建议直接使用 vim，而是使用 visudo 命令。因为修改 /etc/sudoers 文件需遵循一定的语法规则，使用 visudo 的好处就在于，当修改完毕 /etc/sudoers 文件，离开修改页面时，系统会自行检验 /etc/sudoers 文件的语法。/etc/sudoers 给我们提供了 2 个模板，分别用于添加用户和群组：

```shell
root ALL=(ALL) ALL
#用户名 被管理主机的地址=(可使用的身份) 授权命令(绝对路径)

%wheel ALL=(ALL) ALL
#%组名 被管理主机的地址=(可使用的身份) 授权命令(绝对路径)
```

| 模块             | 含义                                                         |
| ---------------- | ------------------------------------------------------------ |
| 用户名或群组名   | 表示系统中的那个用户或群组，可以使用 sudo 这个命令。         |
| 被管理主机的地址 | 用户可以管理指定 IP 地址的服务器。这里如果写 ALL，则代表用户可以管理任何主机；如果写固定 IP，则代表用户可以管理指定的服务器。如果我们在这里写本机的 IP 地址，不代表只允许本机的用户使用指定命令，而是代表指定的用户可以从任何 IP 地址来管理当前服务器。 |
| 可使用的身份     | 就是把来源用户切换成什么身份使用，（ALL）代表可以切换成任意身份。这个字段可以省略。 |
| 授权命令         | 表示 root 把什么命令命令授权给用户，换句话说，可以用切换的身份执行什么命令。需要注意的是，此命令必须使用绝对路径写。默认值是 ALL，表示可以执行任何命令。 |

例如：授权用户 lamp 可以重启服务器，由 root 用户添加

```shell
$ visudo
lamp ALL=/sbin/shutdown -r now
```

##  命令语法

```shell
sudo(选项)(参数)
```

##  选项 

```shell
-b：在后台执行指令；
-E：继承当前环境变量
-h：显示帮助；
-H：将HOME环境变量设为新身份的HOME环境变量；
-k：结束密码的有效期限，也就是下次再执行sudo时便需要输入密码；。
-l：列出目前用户可执行与无法执行的指令；
-p：改变询问密码的提示符号；
-s<shell>：执行指定的shell；
-u<用户>：以指定的用户作为新的身份。若不加上此参数，则预设以root作为新的身份；
-v：延长密码有效期限5分钟；
-V ：显示版本信息。
```

## 示例

### 使用sudo命令安装软件

```shell
$ sudo apt-get install vi
```

### 使用sudo命令以另一个用户的身份执行命令

可以使用-u选项来指定用户的名字或者ID

```shell
$ sudo -u bob whoami
```

这个命令会使用sudo命令来以bob用户的身份执行whoami命令，这个命令用来显示当前用户的名字。你需要输入你的密码来确认你有sudo权限。系统会显示bob。

也可以使用-g选项来指定用户组的名字或者ID

```shell
$ sudo -g staff ls -l /home/bob
```

这个命令会使用sudo命令来以staff用户组的身份执行ls -l /home/bob命令，这个命令用来显示bob用户的家目录下的文件的详细信息。你需要输入你的密码来确认你有sudo权限。系统会显示文件的列表。

### 查看当前用户所拥有的权限

```shell
$ sudo -l
```

### 切换到root用户

```shell
$ sudo -i
```

