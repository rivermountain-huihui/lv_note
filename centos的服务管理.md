## centos的服务管理

#### centos6的服务管理

下载一个服务，直接去/etc/rc.d/init.d/下面找到，然后start，就能跑起来，之后就可以用service来管理了，需要添加开机自启动，就去用：

```
chkconfig xxxx   这样就开机自启动了，其实就是生成了软链接
```

使用service是这样的：

```
service chronyd status
service start chronyd
service stop chronyd
```

使用chkconfig是这样的：   其实就是个软链接，删了软链接也行

```
chkconfig   这样是列出所有的开机启动或禁止程序
chkconfig chronyd    这样是看某一个服务的开机启动禁止情况
chkconfig chronyd on
chkconfig chronyd off   开机启动和禁止的命令
```

下面是centos6的init进程启动后执行脚本的顺序

1.服务都应该在 /etc/rc.d/init.d/  里面，是可执行 start/stop等命令的文件，其他的文件都是软链接到这里来调用的。

2./etc/rc.d/rc.sysinit 就是init执行的第一个初始化脚本

3./etc/rc.d/rc*.d/ 这几个文件就是启动级别，0,1,2,3,4,5,6，文件夹里面放的就是这个级别启动的时候需要运行或者禁止的服务，K开头的就是禁止，S开头的就是启动，全是软链接

```
[root@localhost rc3.d]$ll
total 0
lrwxrwxrwx. 1 root root 19 Aug 22 04:14 K10saslauthd -> ../init.d/saslauthd
lrwxrwxrwx. 1 root root 18 Aug 22 04:14 K61nfs-rdma -> ../init.d/nfs-rdma
lrwxrwxrwx. 1 root root 21 Aug 22 04:13 K87restorecond -> ../init.d/restorecond
lrwxrwxrwx. 1 root root 20 Aug 22 04:13 K89netconsole -> ../init.d/netconsole
lrwxrwxrwx. 1 root root 15 Aug 22 04:13 K89rdisc -> ../init.d/rdisc
lrwxrwxrwx. 1 root root 18 Aug 22 18:04 K92iptables -> ../init.d/iptables
lrwxrwxrwx. 1 root root 22 Aug 22 04:14 S02lvm2-monitor -> ../init.d/lvm2-monitor
lrwxrwxrwx. 1 root root 14 Aug 22 04:14 S05rdma -> ../init.d/rdma
lrwxrwxrwx. 1 root root 19 Aug 22 04:14 S08ip6tables -> ../init.d/ip6tables
lrwxrwxrwx. 1 root root 17 Aug 22 04:13 S10network -> ../init.d/network
lrwxrwxrwx. 1 root root 16 Aug 22 04:14 S11auditd -> ../init.d/auditd
lrwxrwxrwx. 1 root root 17 Aug 22 04:14 S12rsyslog -> ../init.d/rsyslog
lrwxrwxrwx. 1 root root 19 Aug 22 04:14 S15mdmonitor -> ../init.d/mdmonitor
lrwxrwxrwx. 1 root root 26 Aug 22 04:14 S25blk-availability -> ../init.d/blk-availability
lrwxrwxrwx. 1 root root 15 Aug 22 04:13 S25netfs -> ../init.d/netfs
lrwxrwxrwx. 1 root root 19 Aug 22 04:13 S26udev-post -> ../init.d/udev-post
lrwxrwxrwx. 1 root root 15 Aug 22 04:14 S50kdump -> ../init.d/kdump
lrwxrwxrwx. 1 root root 14 Aug 22 04:14 S55sshd -> ../init.d/sshd
lrwxrwxrwx  1 root root 17 Sep  9 19:49 S59chronyd -> ../init.d/chronyd
lrwxrwxrwx. 1 root root 17 Aug 22 04:14 S80postfix -> ../init.d/postfix
lrwxrwxrwx. 1 root root 15 Aug 22 04:14 S90crond -> ../init.d/crond
lrwxrwxrwx. 1 root root 11 Aug 22 04:13 S99local -> ../rc.local

```

4.还有一个/etc/rc.d/rc.local脚本，这里面放你自己写的脚本，写到这里就可以在开机的时候执行

```
vim /etc/rc.d/rc.local
  1 #!/bin/sh
  2 #                                                                        
  3 # This script will be executed *after* all the other init scripts.
  4 # You can put your own initialization stuff in here if you don't
  5 # want to do the full Sys V style init stuff.
  6 
  7 touch /var/lock/subsys/local
```

5.执行等待登录的程序 /bin/login

通过/etc/inittab 可以修改和查看当前的默认运行级别

超级守护进程：**Xinetd**



#### centos7之后的服务管理

/lib/systemd/system/

/usr/lib/systemd/system/   这两个文件下都是service 和 target，代替了脚本和启动级别

/etc/systemd/system  这个文件下放了默认启动级别、每个启动级别要启动和禁止的服务

其他的全部都是systemctl来管理，其实还是软链接的管理

systemctl的使用：

```
systemctl start name.service
systemctl stop name.service
systemctl restart naem.service
systemctl status name.service
systemctl is-active name.service
systemctl is-enabled name.service
systemctl mask name.service 
systemctl unmask name.service 
systemctl list-units -t service #列出运行的服务
systemctl list-units --type service -a  #列出所有服务
systemctl enbale name.service
systemclt disable name.service
ll /etc/systemd/system/*.wants/ #列出哪些服务在哪些运行级别下启用和禁用
systemctl --failed --type service #列出失败的服务
systemctl list-dependencies name.service #列出服务的依赖关系
systemctl kill unitname #杀掉进程
systemctl list-unit-files #列出unit的状态
```

systemd的执行流程：

1.执行initrd.target所有单元，进行系统基本硬件初始化

2.执行sysint.target初始化系统，执行basic.target所有单元准备操作系统

3.执行启动级别下的所有单元，比如multi-user.target下的单元

4.执行等待登录的那个程序，getty.target



systemctl获取和改变默认启动级别

```
systemctl get-default   获取默认启动级别
systemctl set-default   设定默认启动级别
runlevel				获取当前运行级别，其实就是runlevel是个软链接，链接到target上了
```

超级守护进程：**systemd -->daemon**