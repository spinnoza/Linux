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

### 5.基本命令

lscpu 查看CPU

cat /proc/cpuinfo



free 查看内存

cat /proc/meminfo



lsblk 查看硬盘大小

cat /proc/partitions



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









































​	



