# find命令

第一部分：查找名称查找文件的基本查找命令

第二部分：根据他们的权限查找文件

第三部分：基于所有者和组的搜索文件

第四部分：根据日期和时间查找文件和目录

第五部分：根据大小查找文件和目录

第六部分：配合执行命令

NAME
       find - search for files in a directory hierarchy

SYNOPSIS
       find  [-H]  [-L] [-P] [-D debugopts] [-Olevel] [starting-point...] [expression]

***

## 第一部分 - 查找名称查找文件的基本查找命令

1.列出当前目录下和子目录下的所有的文件

```
[root@centos8 shell_test1]#find
.
./argsunm.sh
./work_menu.sh
./answermyquestion.sh
./hostping.sh
./disk_check.sh

```

2.查找特殊的目录或路径

下面的命令会查找当前目录下**test文件夹**中的文件

```
[root@centos8 scripts]#find ./t1
./t1
./t1/1.txt
./t1/s1.sh
./t1/2.txt
./t1/s2.sh
./t1/s3.sh
```

下面的命令用于查找指定名称的文件

```
[root@centos8 scripts]#find ./t1 -name s1.sh
./t1/s1.sh
```

也可以配合通配符使用，使用通配符需要用引号引起来

```
[root@centos8 scripts]#find ./t1 -name "*.sh"
./t1/s1.sh
./t1/s2.sh
./t1/s3.sh
```

忽略大小写

```
[root@centos8 scripts]#find ./t1 -iname abc.sh
./t1/ABC.sh
```

3.限制目录查找的深度

find命令会递归查找整个目录树，这样很消耗时间和资源。好在目录查找深度可以手动指定，例如我们想查找一层或两层以内的子目录，可以通过maxdepth选项来指定

```
[root@centos8 data]#find ./scripts/ -maxdepth 1 -name "test*"
./scripts/test.txt
[root@centos8 data]#find ./scripts/ -maxdepth 2 -name "test*"
./scripts/test.txt
./scripts/shell_test1/test
./scripts/shell_test1/test2
```

与maxdepth对应的有一个mindepth，它是至少到哪层才开始查询

4.反向查找

当我们知道需要排除哪些文件的时候就可以用反向查找,-not也可以用感叹号来代替``! ``

```
[root@centos8 data]#find ./scripts/  -not -name  "*.sh"
./scripts/
./scripts/test.txt
./scripts/shell_test1
./scripts/shell_test1/user.log
```

5.结合多个查找条件

多个条件组合起来查询,find默认条件之间是`` and ``关系，通过``-o``选项可以变成或者的关系

```
[root@centos8 data]#find ./scripts/ -name "*.sh" ! -name "rm.*"
./scripts/while_sum.sh
./scripts/dustbin.sh
```

``-o``或者

```
[root@centos8 data]#find ./scripts/ -name "*.txt" -o -name "login.*"
./scripts/test.txt
./scripts/shell_test1/login.sh
./scripts/shell_test1/1.txt
```

6.只查找文件或目录

通过``-type``选项可以指定要搜索的是文件还是目录

文件和目录都要

```
[root@centos8 data]#find ./scripts/ -name "t1*"
./scripts/t1.txt
./scripts/t1
```

只要文件

```
[root@centos8 data]#find ./scripts/ -type f -name "t1*"
./scripts/t1.txt
```

只要目录

```
[root@centos8 data]#find ./scripts/ -type d -name "t1*"
./scripts/t1
```

7.同时在多个目录下查找

find可以同时在多个目录下查找文件或目录

```
[root@centos8 data]#find ./scripts/ ./syncfile/ -name test.txt
./scripts/test.txt
```

8.查找隐藏文件

Linux的隐藏文件就是用英文``.``开头的文件

```
[root@centos8 data]#find ~ -maxdepth 1 -name ".*"
/root/.bash_logout
/root/.bash_profile
/root/.vscode-server
/root/.bashrc
/root/.wget-hsts
/root/.cshrc
/root/.tcshrc
/root/.bash_history
/root/.vimrc
/root/.config
/root/.lesshst
/root/.gnupg
/root/.viminfo
/root/.ssh
```

## 第二部分 - 基于文件权限和属性的查找

9.查找指定权限的文件

通过``perm``选项可以指定权限

```
[root@centos8 data]#find ./scripts/ -perm 0755
./scripts/
./scripts/while_sum.sh
./scripts/dustbin.sh
./scripts/CA.sh
```

通过这个命令可以去查找错误权限的文件

```
find ./scripts/ -perm 777
```

10.查找具有SGID/SUID属性的文件

```
find / -perm 2644
```

同样可以查找设置了粘滞位（sticky bit）的文件

```
find / -perm 1644
```

perm还可以支持/u /a /g /o 的写法

11.意思是，user中包含s权限的

```
[root@centos8 bin]#find -perm /u=s
./fusermount
./chage
./gpasswd
./newgrp
./umount
./mount
./sudo
./su
```

12.查找所有用户可执行的文件

```
[root@centos8 data]#find  -type f -perm  /a=x
./scripts/while_sum.sh
./scripts/dustbin.sh
./scripts/CA.sh
./scripts/init.sh
```

## 第三部分 - 基于文件所有者和用户组的查找

13.查找属于特定用户的文件

```
[root@centos8 data]#find /home -user lv
/home/lv
/home/lv/.bash_logout
/home/lv/.bash_profile
/home/lv/.bashrc
/home/lv/.bash_history
```

14.查找属于特定用户组的文件

```
[root@centos8 data]#find /home -group lv
/home/lv
/home/lv/.bash_logout
/home/lv/.bash_profile
/home/lv/.bashrc
/home/lv/.bash_history
```

## 第四部分 - 基于日期和时间的查找

基于时间的查找是个很棒的功能，包括文件的修改时间、访问时间等。

atime access time 文件被访问的时间

mtime modify time 文件内容被修改的时间，block数据被修改的时间

ctime change tiem 文件状态被修改的时间，meta数据被修改的时间

15.查找过去N天被修改过的文件

```
find -mtime -50
```

16.查找过去N天内被访问过的文件

```
[root@centos8 data]#find /etc/ -atime -5
```

17.查找过去某段时间范围内被修改过内容的文件，+0 -30 表示 从今天到前30天内

```
[root@centos8 data]#find  -type f -mtime +0 -mtime -30
./scripts/while_sum.sh
./scripts/dustbin.sh
./scripts/checkdata.sh
./scripts/test.txt
./scripts/CA.sh
./scripts/init.sh
./scripts/shell_test1/user_for.sh
```

18.查找过去的N分钟内状态发生改变的文件

```
[root@centos8 data]#find -cmin -60
./scripts
./scripts/t1.txt
```

19.查找过去的1小时内被修改过内容的文件

```
[root@centos8 data]#find -mmin -60
./scripts
./scripts/t1.txt
./scripts/t1
./scripts/t1/ABC.sh
./scripts/t1/123dsaf.sh
```

20.查找过去的1小时内被访问过的文件

```
[root@centos8 data]#find -type f -amin -120
./scripts/openvpn-user-crt.sh
./scripts/t1.txt
./scripts/t1/ABC.sh
./scripts/t1/123dsaf.sh
```

## 第五部分 - 基于文件大小的查找

-size n[cwbkMG]
              File uses n units of space,  rounding  up.   The  following
              suffixes can be used:

              `b'    for  512-byte blocks (this is the default if no suf‐
                     fix is used)
    
              `c'    for bytes
    
              `w'    for two-byte words
    
              `k'    for Kilobytes (units of 1024 bytes)
    
              `M'    for Megabytes (units of 1048576 bytes)
              `w'    for two-byte words
    
              `k'    for Kilobytes (units of 1024 bytes)
    
              `M'    for Megabytes (units of 1048576 bytes)
    
              `G'    for Gigabytes (units of 1073741824 bytes)
    
              The size does not count indirect blocks, but it does  count
              blocks  in  sparse  files  that are not actually allocated.
              Bear in mind that the `%k' and `%b'  format  specifiers  of
              -printf  handle  sparse  files differently.  The `b' suffix
              always denotes 512-byte blocks and never 1 Kilobyte blocks,
              which  is  different  to the behaviour of -ls.  The + and -
              prefixes signify greater than and less than, as usual,  but
              bear  in  mind that the size is rounded up to the next unit
              (so a 1-byte file is not matched by -size -1M).
21.查找指定大小的文件

```
[root@centos8 /]#find -size +50M 2> /dev/null
./boot/initramfs-0-rescue-4ced7a3cfb2d4e2899d3dfb26801c8e1.img
./proc/kcore
./sys/devices/pci0000:00/0000:00:0f.0/resource1
./sys/devices/pci0000:00/0000:00:0f.0/resource1_wc
```

22.查找大小在一定范围内的文件

```
[root@centos8 /]#find -size +50M -size -100M 2> /dev/null
./boot/initramfs-0-rescue-4ced7a3cfb2d4e2899d3dfb26801c8e1.img
./var/www/html/epel/Packages/c/chromium-84.0.4147.89-1.el8.x86_64.rpm
./var/www/html/epel/Packages/c/chromium-headless-84.0.4147.89-1.el8.x86_64.rpm
```

23.查找最大和最小的文件，配合sort,head命令

最大文件

```
[root@centos8 data]#find -type f -exec ls -s {} \; | sort -nr | head -n 1
7924 ./syncfile/backup/etc-2020-09-09-13.tar.xz
```

最小文件

```
[root@centos8 data]#find -type f -exec ls -s {} \; | sort -n | head -n 1
0 ./backup/etc/sysconfig/run-parts
```

24.查找空文件和空目录

加上``empty``选项

查找空文件：

```
[root@centos8 data]#find -type f -empty
./scripts/t1.txt
./scripts/shell_test1/test/1.bak
./scripts/shell_test1/test/2.bak
```

查找空目录：

```
[root@centos8 data]#find -type d -empty
./scripts/shell_test1/test2
./backup/etc/sysconfig/rhn/allowed-actions/configfiles
./backup/etc/sysconfig/rhn/allowed-actions/script
```

## 第六部分 - 高级操作

find命令不仅可以同特定条件来查询文件，还可以对查找到的文件使用任意Linux命令进行操作。

```
#常用格式
find 指定目录 查找条件 -exec command {} \;
```

25.使用ls命令列出文件信息

```
[root@centos8 data]#find ./scripts/ -exec ls  {} \;
backup2.sh	 config		      ping.sh	     t1.txt
backup.sh	 dustbin.sh	      rm.sh	     test.txt
CA.sh		 init.sh	      shell_test1    user_add.sh
```

26.删除找到的文件

```
[root@centos8 data]#find . -name "age.txt" -exec rm -f {} \;
```

