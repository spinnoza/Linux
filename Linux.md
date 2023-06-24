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











​	



