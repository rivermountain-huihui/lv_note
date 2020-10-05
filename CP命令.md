# CP命令

1.强制覆盖

```
#1.cp本身是被设置成了别名 cp -i 了，所以去掉别名就可以了
#2.通过这个指令，强制覆盖
\cp file1 /data/file1  
```

2.复制同时给目的文件备份一个

```
#给file2备份一个，名字是 file2~
cp -b file1 file2
[root@centos8 ~]#cp -b TT TEST
[root@centos8 ~]#ll TEST*
-rw-r--r-- 1 root root    35 Sep 25 19:34 TEST
-rw-r--r-- 1 root root 32937 Aug 15 20:26 TEST~
```

