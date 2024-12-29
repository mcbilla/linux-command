crontab
===

提交和管理用户的需要周期性执行的任务

## 补充说明

crontab命令是Linux系统中用来定期执行任务的一个工具。它可以让用户在指定的时间或者间隔执行一些命令或者脚本。crontab命令的名字来自于cron table的缩写，表示一个时间表的意思。crontab命令有以下几个特点：

- crontab命令可以让用户编辑自己的定时任务，也可以让管理员编辑其他用户的定时任务，只要有相应的权限。
- crontab命令的执行依赖于crond服务，这是一个后台进程，每分钟会检查一次是否有需要执行的任务，如果有，就会自动执行。
- crontab命令的执行结果会通过邮件发送给用户，除非用户指定了输出重定向。
- crontab命令的语法和格式有一些特殊的规则，需要注意遵守。

### crontab的基本组成

```
                                 系统服务                         配置工具
    ---------                    ------                         --------
    |配置文件|    ---------->     |crond|        <------------   |crontab|
    --------                     ------                         --------
 文件方式设置定时任务       每分钟都会从配置文件刷新定时任务       用于调整定时任务
```

cron、crond和crontab命令的区别：

- cron是Linux内置定时任务服务的`名称`。
- crond是系统级别执行定时任务的`守护进程`，会默认安装并自动启动。crond进程每分钟会定期检查是否有要执行的任务，如果有要执行的任务，则自动执行该任务。crond守护进程是在系统启动时由init进程启动的，受init进程的监视，如果它不存在了，会被init进程重新启动。crond 守护进程每分钟唤醒一次，检查 `/etc/crontab 文件`、`etc/cron.d/ 目录` 和 `/var/spool/cron 目录`中的改变。如果发现了改变，它们就会被载入内存。这样，当某个 crontab 文件改变后就不必重新启动守护进程了。
- crontab是用来创建定时任务的`命令`。这个命令在某些系统里面需要自行安装。

### crontab的任务分类

Linux下的任务调度分为两类： **系统任务调度** 和 **用户任务调度** 。

 **系统任务调度：** 系统周期性所要执行的工作，比如写缓存数据到硬盘、日志清理等。在`/etc`目录下有一个crontab文件，这个就是系统任务调度的配置文件。但是也有一些cron的实现版本没有`/etc/crontab`文件，这些版本的系统调度配置文件放在 `/var/spool/cron/crontabs/root` 或 `/etc/cron.d/` 目录下面。

`/etc/crontab`文件包括下面几行：

```shell
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=""HOME=/

# run-parts
51 * * * * root run-parts /etc/cron.hourly
24 7 * * * root run-parts /etc/cron.daily
22 4 * * 0 root run-parts /etc/cron.weekly
42 4 1 * * root run-parts /etc/cron.monthly
```

前四行是用来配置crond任务运行的环境变量，第一行SHELL变量指定了系统要使用哪个shell，这里是bash，第二行PATH变量指定了系统执行命令的路径，第三行MAILTO变量指定了crond的任务执行信息将通过电子邮件发送给root用户，如果MAILTO变量的值为空，则表示不发送任务执行信息给用户，第四行的HOME变量指定了在执行命令或者脚本时使用的主目录。

 **用户任务调度：** 用户定期要执行的工作，比如用户数据备份、定时邮件提醒等。用户可以使用 crontab 工具来定制自己的计划任务。所有用户定义的crontab文件都被保存在`/var/spool/cron`或 `/var/spool/cron/crontabs` 目录中，生成一个和用户名一致的文件，例如 `/var/spool/cron/username`，文件内容就是我们编辑的定时任务。

```shell
/var/spool/cron/   所有用户crontab文件存放的目录,以用户名命名
```

另外还可以通过下面两个文件控制哪些用户可以使用crontab命令：

```
/etc/cron.deny     该文件中所列用户不允许使用crontab命令
/etc/cron.allow    该文件中所列用户允许使用crontab命令
```

使用特点：

* 格式都是每行一个用户。不允许出现空格。
* 如果两个文件都存在，cron.allow有优先权。
* 如果两个文件都不存在，只有root可以提交任务。
* 两个文件只使用一个来限制就够了，因此建议只要保留一个即可。一般来说，系统默认是保留 `/etc/cron.deny` 。
* 如果cron.deny文件为空文件，所有的用户都可以使用crontab。
* cron.deny不能限制root用户。

### crontab的文件格式

用户所建立的crontab文件中，每一行都代表一项任务，每行的每个字段代表一项设置，它的格式共分为六个字段，前五段是时间设定段，第六段是要执行的命令段，格式如下：

```shell
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

其中：

*   minute： 表示分钟，可以是从0到59之间的任何整数。
*   hour：表示小时，可以是从0到23之间的任何整数。
*   day：表示日期，可以是从1到31之间的任何整数。
*   month：表示月份，可以是从1到12之间的任何整数。
*   week：表示星期几，可以是从0到7之间的任何整数，这里的0或7代表星期日。
*   command：要执行的命令，可以是系统命令，也可以是自己编写的脚本文件。

在以上各个字段中，还可以使用以下特殊字符：

*   星号（*）：代表所有可能的值，例如month字段如果是星号，则表示在满足其它字段的制约条件后每月都执行该命令操作。
*   逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，“1,2,5,7,8,9”
*   中杠（-）：可以用整数之间的中杠表示一个整数范围，例如“2-6”表示“2,3,4,5,6”
*   正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，例如*/10，如果用在minute字段，表示每十分钟执行一次。

**注意crontab的最小时间单位为`1分钟`，如果要实现秒级的定时执行，需要通过特殊的方法。**

### crontab的日志文件

默认情况下，crontab中执行的日志写在 `/var/log/cron` 文件下。我们也可以修改crontab脚本实现日志重定向。

```shell
# 输出日志重定向，注意要将标准错误日志一起重定向，才能获取到正常和错误的日志
0 */3 * * * /bin/sh /usr/crontab/test.sh > /usr/log/crontab/test.log 2>&1

# 对于执行shell脚本，还可以通过添加参数-x来获取更加详细的执行过程
0 */3 * * * /bin/sh -x /usr/crontab/test.sh > /usr/log/crontab/test.log 2>&1

# 清空日志输出
0 */3 * * * command > /dev/null 2>&1
```

## 适用的Linux版本

crontab命令是一个通用的Linux命令，几乎所有的Linux发行版都支持它，包括Ubuntu, Debian, CentOS, Fedora, Red Hat, SUSE等。不过，不同的发行版可能有一些细微的差别，比如crontab文件的位置，crond服务的管理方式，crontab命令的选项等。可以通过下面方式查看cron的实现版本。

```shell
# ubuntu，20.04的默认实现版本为Vixie Cron
$ man cron

# centos，默认实现版本为cronie
whereis -b crontab | cut -d' ' -f2 | xargs rpm -qf
```

下面是一些常见的发行版的crontab命令的特点：

- Ubuntu和Debian：crontab文件的位置在`/var/spool/cron/crontabs`，每个用户有一个单独的文件，文件名就是用户名。crond服务的管理方式是使用`service cron start|stop|restart|status`命令。crontab命令的选项有`-u`（指定用户），`-e`（编辑），`-l`（列出），`-r`（删除），`-i`（删除前询问）等。
- CentOS和Fedora：crontab文件的位置在`/var/spool/cron`，每个用户有一个单独的文件，文件名就是用户名。crond服务的管理方式是使用`systemctl start|stop|restart|status crond`命令。crontab命令的选项有`-u`（指定用户），`-e`（编辑），`-l`（列出），`-r`（删除），`-i`（删除前询问），`-n`（显示下一次执行的时间）等。
- Red Hat和SUSE：crontab文件的位置在`/var/spool/cron`，每个用户有一个单独的文件，文件名就是用户名。crond服务的管理方式是使用`/etc/init.d/crond start|stop|restart|status`命令。crontab命令的选项有`-u`（指定用户），`-e`（编辑），`-l`（列出），`-r`（删除），`-i`（删除前询问），`-s`（选择编辑器）等。

如果要在不同的Linux发行版之间切换，可以使用`tab`键来显示crontab命令的选项，或者使用`man crontab`来查看详细的帮助信息。

##  命令语法

```shell
crontab [ -u user ] file
crontab [ -u user ] [ -i ] { -e | -l | -r }
```

##  选项

- -u user：用来设定某个用户的crontab服务，例如，“-u ixdba”表示设定ixdba用户的crontab服务，此参数一般有root用户来运行。
- file：file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。
- -e：编辑某个用户的crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件。
- -l：显示某个用户的crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容。
- -r：从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户，则默认删除当前用户的crontab文件。
- -i：在删除用户的crontab文件时给确认提示。

## 使用Crontab服务

### 1、安装/启动/停止/重启

##### debian/ubuntu

```shell
$ sudo apt install cron //安装
$ service cron start    //启动
$ service cron stop     //停止
$ service cron restart  //重启
$ service cron status   //检查状态
$ service cron         //查询cron可用的命令
$ crontab -l           //检查crontab工具是否安装，没有报错即成功安装
```

##### centos

```shell
# vixie-cron 软件包是 cron 的主程序，crontabs 软件包是用来安装、卸装、或列举用来驱动 cron 守护进程的表格的程序。
$ sudo yum install vixie-cron
$ sduo yum install crontabs

$ service crond start     //启动服务
$ service crond stop      //关闭服务
$ service crond restart   //重启服务
$ service crond reload    //重新载入配置
$ service crond status    //查看crontab服务状态

$ chkconfig --level 345 crond on //加入开机自启动
```

### 2、创建crontab任务

#### 1）设置环境变量编辑器

cron进程根据环境变量 `VISUAL` 或 `EDITOR` 来确定使用哪个编辑器编辑crontab文件。执行

```shell
$ vim ~/.profile
```

把下面内容添加到文件末尾

```shell
EDITOR=vim; export EDITOR
```

退出，执行下面命令使其生效

```shell
$ source ~/.profile
```

#### 2）选择创建命令

如果是普通用户，只能使用 `crontab -e` 来设置
如果是root用户，有4种设置方式

1. 使用命令 `crontab -e`，内容在文件`/var/spool/cron/root`
2. `/etc/crontab`文件
3. `/etc/cron.d`目录下新建一个文件
4. `/etc/cron.daily/` 、`/etc/cron.hourly/`、`/etc/cron.weekly/` 、`/etc/cron.monthly/`

一般情况下用`/etc/cron.d`

#### 3）编辑脚本内容

用户第一次直接执行`crontab -e`会出现一个只有注释的空白文件，在下面添加自己的任务内容即可。
如果是root用户可以修改`/etc/crontab`文件，`/etc/crontab`的文件内容如下：

```shell
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

# user-defined script
0 15 * * 1-5 /bin/Is
```

前四行是用来配置crond任务运行的环境变量

- 第一行SHELL变量指定了系统要使用哪个shell，这里是bash。
- 第二行PATH变量指定了系统执行命令的路径。
- 第三行MAILTO变量指定了crond的任务执行信息将通过电子邮件发送给root用户，如果MAILTO变量的值为空，则表示不发送任务执行信息给用户。
- 第四行的HOME变量指定了在执行命令或者脚本时使用的主目录 。

脚本修改完成后保存即可自动生效。

### 3、查看任务列表

```shell
# 查看当前用户的cron作业
$ crontab -l

# 查看其它用户的cron作业
$ crontab -l -u username
```

### 4、删除定时任务

```shell
$ crontab -r

# 删除且无需再次确认
$ crontab -ir
```

运行 `crontab -r` 需要特别谨慎。它从 Crontab 目录（/var/spool/cron）中删除用户的 crontab 文件。删除了该用户的所有 crontab。

如果不小心误删了crontab文件，如果还有备份，那么可以将其拷贝到/var/spool/cron/<username>，其中<username>是用户名。如果由于权限问题无法完成拷贝，可以用

```shell
$ crontab <filename>
```

会加载file的内容作为当前用户的cron作业列表。

## 示例

```shell
实例1：每1分钟执行一次command
命令：
* * * * * command

实例2：每小时的第3和第15分钟执行
命令：
3,15 * * * * command

实例3：在上午8点到11点的第3和第15分钟执行
命令：
3,15 8-11 * * * command

实例4：每隔两天的上午8点到11点的第3和第15分钟执行
命令：
3,15 8-11 */2 * * command

实例5：每个星期一的上午8点到11点的第3和第15分钟执行
命令：
3,15 8-11 * * 1 command

实例6：每晚的21:30重启smb
命令：
30 21 * * * /etc/init.d/smb restart

实例7：每月1、10、22日的4 : 45重启smb
命令：
45 4 1,10,22 * * /etc/init.d/smb restart

实例8：每周六、周日的1 : 10重启smb
命令：
10 1 * * 6,0 /etc/init.d/smb restart

实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb
命令：
0,30 18-23 * * * /etc/init.d/smb restart

实例10：每星期六的晚上11 : 00 pm重启smb
命令：
0 23 * * 6 /etc/init.d/smb restart

实例11：每一小时重启smb
命令：
* */1 * * * /etc/init.d/smb restart

实例12：晚上11点到早上7点之间，每隔一小时重启smb
命令：
* 23-7/1 * * * /etc/init.d/smb restart

实例13：每月的4号与每周一到周三的11点重启smb
命令：
0 11 4 * mon-wed /etc/init.d/smb restart

实例14：一月一号的4点重启smb
命令：
0 4 1 jan * /etc/init.d/smb restart

实例15：每小时执行/etc/cron.hourly目录内的脚本
命令：
01 * * * * root run-parts /etc/cron.hourly
说明：
run-parts这个参数了，如果去掉这个参数的话，后面就可以写要运行的某个脚本名，而不是目录名了
```

要检验时间表达式是否满足需求，可以上 https://tool.lu/crontab/。

## 注意事项

- 新创建的cron job，不会马上执行，至少要过2分钟才执行。如果重启cron则马上执行。
- 当crontab突然失效时，可以尝试/etc/init.d/crond restart解决问题。或者查看日志看某个job有没有执行/报错tail -f /var/log/cron。
- /etc/crontab的权限。不要随意改动这个文件的属性，这个文件属性应该设置成644或者600，否则会报(system) BAD　FILE MODE (/etc/crontab )

## 常见问题

### 实现秒级执行

Linux crontab 命令，最小的执行时间是一分钟。如需要在小于一分钟内重复执行，可以有两个方法实现。

##### 1、crontab脚本中使用sleep延时

```shell
* * * * * command
* * * * * sleep 10; command
* * * * * sleep 20; command
* * * * * sleep 30; command
* * * * * sleep 40; command
* * * * * sleep 50; command
```

使用sleep N延时，实现每10秒执行一次脚本。
N是60必须能整除间隔的秒数（没有余数），例如间隔的秒数是2，4，6，10，12等。
如果间隔的秒数太少，例如2秒执行一次，这样就需要在crontab 加入60/2=30条语句。不建议使用此方法，可以使用下面介绍的第二种方法。

##### 2、编写shell脚本

编写 `/usr/crontab/test.sh`

```shell
#!/bin/bash

step=2 #间隔的秒数，不能大于60

for (( i = 0; i < 60; i=(i+step) )); do
    your command
    sleep $step
done

exit 0
```

保存退出，然后执行 `crontab -e`，在最后添加

```shell
* * * * * /usr/crontab/test.sh
```

注意：如果60不能整除间隔的秒数，则需要调整执行的时间。例如需要每7秒执行一次，就需要找到7与60的最小公倍数，7与60的最小公倍数是420（即7分钟）。然后设置 `test.sh` 的 `step` 的值为7，循环结束条件 `i<420`。

### 环境变量问题

`crontab 任务无法访问环境变量`。有时我们创建了一个crontab，但是这个任务却无法自动执行，而手动执行这个任务却没有问题，这种情况一般是由于在crontab文件中没有配置环境变量引起的。

在 crontab文件中定义多个调度任务时，需要特别注意的一个问题就是环境变量的设置，因为我们手动执行某个任务时，是在当前shell环境下进行的，程 序当然能找到环境变量，而系统自动执行任务调度时，是不会加载任何环境变量的，因此，就需要在crontab文件中指定任务运行所需的所有环境变量，这 样，系统执行任务调度时就没有问题了。

不要假定cron知道所需要的特殊环境，它其实并不知道。所以你要保证在shelll脚本中提供所有必要的路径和环境变量，除了一些自动设置的全局变量。所以注意如下3点：

1. 脚本中涉及文件路径时写全局路径。
2. shell脚本执行要用到java或其他环境变量时，通过source命令引入环境变量。

```shell
#!/bin/sh

source /etc/profile

export RUN_CONF=/home/d139/conf/platform/cbp/cbp_jboss.conf

/usr/local/jboss-4.0.5/bin/run.sh -c mev &
```

3. crontab脚本中直接引入环境变量。

```shell
0 * * * * . /etc/profile;/bin/sh /usr/crontab/test.sh
```

### 引号问题

命令行双引号中使用 `%` 时，要加反斜线`\`。在 crontab 中 `%` 是有特殊含义的，表示换行的意思。如果要用的话必须进行转义，如经常用的 date `‘+%Y%m%d’`在 crontab 里是不会执行的，应该换成 date `‘+\%Y\%m\%d’`。

### 磁盘占满问题

/var/spool/clientmqueue目录过大，占用磁盘满了。原因：/var/spool/clientmqueue是如果系统中有用户开启了cron，而cron中执行的程序有输出内容，输出内容会以邮件形式发给cron的用户，而sendmail没有启动所以就产生了这些文件。解决：将输出重定向，如> /dev/null 2>&1。注意错误输出也要重定向
