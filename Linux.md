# Linux

## 1.linux基础操作

### 1.用户登录信息察看命令

- whoami 显示当前登录有效用户
- who 系统当前所有登录会话
- w 系统当前所有登录会话及所做的操作

- tty 查看当前登录设备

- hostnamectl set-hostname rocky 修改主机名

- 设置命令提示符格式

  ~~~shell
   # echo 'PS1="\[\e[1;32m\][\t \[\e[1;33m\]\u\[\e[35m\]@\h\[\e[1;31m\] \W\[\e[1;32m\]]\[\e[0m\]\\$"' > /etc/profile.d/env.sh
   # . /etc/profile.d/env.sh
  ~~~

### 2.内部命令 外部命令

- 判断一个命令是否是内部命令还是外部命令

  ~~~shell
  #type echo
  ~~~

- 系统路径顺序

  ~~~shell
  # echo $PATH
  ~~~

- 外部命令可以被hash

- 给一个命令起别名(临时)

  ~~~bash
  # alias cdnet="cd /etc/sysconfig/network-scripts/"
  ~~~

- 永久修改别名需要修改.bashrc文件

### 3.命令的格式

COMMAND [OPTIONS...] [ARGUMENTS...]

COMMAND [COMMAND] [COMMAND] ....

### 4.命令帮助的用法

- type -a rm 显示一个命令的所有出处

- help 专门查看内部命令的帮助

- 外部命令查看帮助  : --help

  ~~~shell
  timedatectl --help
  ~~~

- 查看更详细的帮助命令 man

  空格翻页  /搜索内容  n向下搜索 N向上搜索  q退出

- where is 列出一个命令的程序路径和相关文档的路径

- whatis 列出命令的帮助文档索引

### 5.基本命令

lscpu 查看CPU

cat /proc/cpuinfo



free 查看内存

cat /proc/meminfo



lsblk 查看硬盘大小

cat /proc/partitions

df -h



uname -r 查看内核版本

cat /etc/os-release

lsb_release -a



### 6.日期和时间

### 7.命令嵌套

``

$()

' ' 字符串正常输出

" " 可以识别变量,不能识别命令

### 8.字符集和编码

查看一个文件的16进制形式

~~~ shell
# hexdump -C test.txt
~~~



## 2.文件管理

### 1.文件目录结构

![image-20230625200829103](./assets/image-20230625200829103.png)

- 文件和目录被组织成一个单根倒置树结构

- 文件系统从根目录下开始，用“/”表示

- 根文件系统(rootfs)：root filesystem

- 标准Linux文件系统（如：ext4），文件名称大小写敏感，例如：MAIL, Mail, mail, mAiL

- 以 . 开头的文件为隐藏文件

- 路径分隔的 /

- 文件名最长255个字节

- 包括路径在内文件名称最长4095个字节

- 蓝色-->目录 绿色-->可执行文件 红色-->压缩文件 浅蓝色-->链接文件 灰色-->其他文件

- 除了斜杠和NUL,所有字符都有效.但使用特殊字符的目录名和文件不推荐使用，有些字符需要用引

  号来引用

- 每个文件都有两类相关数据：元数据：metadata，即属性， 数据：data，即文件内容





/boot：引导文件存放目录，内核文件(vmlinuz)、引导加载器(bootloader, grub)都存放于此目录

/bin：所有用户使用的基本命令；不能关联至独立分区，OS启动即会用到的程序

/sbin：管理类的基本命令；不能关联至独立分区，OS启动即会用到的程序

/lib：启动时程序依赖的基本共享库文件以及内核模块文件(/lib/modules)

/lib64：专用于x86_64系统上的辅助共享库文件存放位置

/etc：配置文件目录

/home/USERNAME：普通用户家目录

/root：管理员的家目录

/media：便携式移动设备挂载点

/mnt：临时文件系统挂载点

/dev：设备文件及特殊文件存储位置

 b: block device，随机访问

 c: character device，线性访问

/opt：第三方应用程序的安装位置

/srv：系统上运行的服务用到的数据

/tmp：临时文件存储位置

/usr: universal shared, read-only data

 bin: 保证系统拥有完整功能而提供的应用程序

 sbin:

 lib：32位使用

 lib64：只存在64位系统

 include: C程序的头文件(header files)

 share：结构化独立的数据，例如doc, man等

​    local：第三方应用程序的安装位置

  bin, sbin, lib, lib64, etc, share

/var: variable data files

 cache: 应用程序缓存数据目录

 lib: 应用程序状态信息数据

 local：专用于为/usr/local下的应用程序存储可变数据

 lock: 锁文件

 log: 日志目录及文件

 opt: 专用于为/opt下的应用程序存储可变数据

 run: 运行中的进程相关数据,通常用于存储进程pid文件

 spool: 应用程序数据池

 tmp: 保存系统两次重启之间产生的临时数据

/proc: 用于输出内核与进程信息相关的虚拟文件系统

/sys：用于输出当前系统上硬件设备相关信息虚拟文件系统

/selinux: security enhanced Linux，selinux相关的安全策略等信息的存储位置



### 2.文件类型

- -普通文件
- d 目录文件directory
- l 符号链接文件link
- b 块设备block 
- c 字符设备character
- p 管道文件pipe
- s 套接字文件socket



### 3.文件操作

- 基名和目录名

  ~~~bash
  [21:26:23 root@rocky ~]#dirname /etc/sysconfig/network-scripts/ifcfg-ens160
  /etc/sysconfig/network-scripts
  [21:26:30 root@rocky ~]#basename /etc/sysconfig/network-scripts/ifcfg-ens160
  ifcfg-ens160
  ~~~

- 查看一个文件的文件类型

  ~~~shell
  [20:38:15 root@rocky ~]#file /tmp/dbus.log
  /tmp/dbus.log: ASCII text
  ~~~

- 文件名通配符

  ~~~
  * 匹配零个或多个字符，但不匹配 "." 开头的文件，即隐藏文件
  ? 匹配任何单个字符,一个汉字也算一个字符
  ~ 当前用户家目录
  ~mage 用户mage家目录
  [0-9] 匹配数字范围
  [a-z] 一个字母
  [A-Z] 一个字母
  [wang] 匹配列表中的任何的一个字符
  [^wang] 匹配列表中的所有字符以外的字符
  [^a-z] 匹配列表中的所有字符以外的字符
  . 和 ~+ 当前工作目录
  ~-   前一个工作目录
  ~~~

  ~~~shell
  [:digit:]：任意数字，相当于0-9
  [:lower:]：任意小写字母,表示 a-z
  [:upper:]: 任意大写字母,表示 A-Z 
  [:alpha:]: 任意大小写字母
  [:alnum:]：任意数字或字母
  [:blank:]：水平空白字符
  [:space:]：水平或垂直空白字符
  [:punct:]：标点符号
  [:print:]：可打印字符
  [:cntrl:]：控制（非打印）字符
  [:graph:]：图形字符
  [:xdigit:]：十六进制字符
  ~~~

  

- 复制文件和目录

  cp

- 文件删除和目录管理

  安全的删除正在使用的文件
  
   

### 4.文件属性

文件inode number

文件编号满了就算有空间也无法创建文件了



### 5.标准输入输出错误和重定向

- 概念阐述

  1.打开一个文件

  ~~~shell
  [21:31:26 root@rocky ~]#tail -f /var/log/messages
  Jul  1 21:31:26 rocky systemd[1]: Started User Manager for UID 0.
  Jul  1 21:31:26 rocky systemd[1]: Started Session 4 of user root.
  Jul  1 21:31:26 rocky systemd[2396]: Started D-Bus User Message Bus.
  Jul  1 21:31:26 rocky dbus-daemon[2450]: [session uid=0 pid=2450] Activating via systemd: service name='org.gtk.vfs.Daemon' unit='gvfs-daemon.service' requested by ':1.1' (uid=0 pid=2448 comm="flatpak --installations " label="unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023")
  Jul  1 21:31:26 rocky systemd[2396]: Starting Virtual filesystem service...
  Jul  1 21:31:26 rocky dbus-daemon[2450]: [session uid=0 pid=2450] Successfully activated service 'org.gtk.vfs.Daemon'
  Jul  1 21:31:26 rocky systemd[2396]: Started Virtual filesystem service.
  Jul  1 21:31:31 rocky systemd[1]: fprintd.service: Succeeded.
  Jul  1 21:31:31 rocky systemd[1]: systemd-localed.service: Succeeded.
  Jul  1 21:31:31 rocky systemd[1]: systemd-hostnamed.service: Succeeded.
  Jul  1 21:31:42 rocky systemd-logind[1079]: New session 6 of user root.
  
  ~~~

​	   2.查看该命令的进程编号

 ~~~ shell
 [21:31:43 root@rocky ~]#pidof tail
 2480
 ~~~

​     3.查看进程编号关联的文件描述符

~~~ shell
[21:37:58 root@rocky ~]#ls /proc/2480/fd -l
total 0
lrwx------. 1 root root 64 Jul  1 21:37 0 -> /dev/pts/1
lrwx------. 1 root root 64 Jul  1 21:37 1 -> /dev/pts/1
lrwx------. 1 root root 64 Jul  1 21:37 2 -> /dev/pts/1
lr-x------. 1 root root 64 Jul  1 21:37 3 -> /var/log/messages
lr-x------. 1 root root 64 Jul  1 21:37 4 -> anon_inode:inotify
~~~



- /dev/null设备是一个特殊的字符设备,类似于一个黑洞,可以用重定向的功能删除一个文件

- 1> 标准输出

- 2> 标准错误

- 追加 1>> 2>>

- 标准输入 < 

- 多行重定向

  ~~~ shell
  [22:24:12 root@rocky test]#cat > a.txt <<EOF
  > line1
  > line2
  > line3
  > EOF
  [22:24:41 root@rocky test]#cat a.txt 
  line1
  line2
  line3
  ~~~



### 6.硬链接和软链接

- 硬链接两个文件的节点编号一样,链接数加1
- 跨分区不能创建硬链接
- 修改软链接的任何一个文件都会修改源文件
- 软链接的应用: 版本升级...
- 注意删除软链接的/
- 软链接可以跨分区



### 7.管道

- tee 命令和tr命令

- 管道符号 |    : 把一个命令的标准输出 给到一个命令的标准输入





## 3.用户,组和权限管理

- 用户
- 组



### 1.**用户和组的主要配置文件**

- /etc/passwd：用户及其属性信息(名称、UID、主组ID等）
- **passwd**文件格式

1. login name：登录用名（wang）
2. passwd：密码 (x)
3. UID：用户身份编号 (1000)
4. GID：登录默认所在组编号 (1000)
5. GECOS：用户全名或注释
6. home directory：用户主目录 (/home/wang)
7. shell：用户默认使用shell (/bin/bash)



- /etc/shadow：用户密码及其相关属性

- **shadow**文件格式

1. 登录用名
2. 用户密码:一般用sha512加密
3. 从1970年1月1日起到密码最近一次被更改的时间
4. 密码再过几天可以被变更（
5. 0表示随时可被变更）
6. 密码再过几天必须被变更（99999表示永不过期）
7. 密码过期前几天系统提醒用户（默认为一周）
8. 密码过期几天后帐号会被锁定
9. 从1970年1月1日算起，多少天后帐号失效



- /etc/group：组及其属性信息

- **group**文件格式

1. 群组名称：就是群组名称
2. 群组密码：通常不需要设定，密码是被记录在 /etc/gshadow 
3. GID：就是群组的 ID 
4. 以当前组为附加组的用户列表(分隔符为逗号)



- /etc/gshadow：组密码及其相关属性

- **gshdow**文件格式

1. 群组名称：就是群的名称
2. 群组密码：
3. 组管理员列表：组管理员的列表，更改组密码和成员
4. 以当前组为附加组的用户列表：多个用户间用逗号分隔



### 2.用户管理命令

- useradd
- usermod
- userdel

### 3.组账号维护命令

- groupadd
- groupmod
- groupdel





























​	



