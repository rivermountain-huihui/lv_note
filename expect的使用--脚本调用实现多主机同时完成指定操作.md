## expect的使用--脚本调用实现多主机同时完成指定操作 

```
  1 #!/bin/bash
  2 #
  3 #
  4 #Author:    lvzhehui
  5 #QQ:        734709865
  6 #Date:      2020-08-27
  7 #FileName:  for_expect.sh
  8 #Descripting:   
  9 #Copyright (C): 2020 All rights reserved
 10 #
 11 NET=10.0.0
 12 IPLIST="
 13 3
 14 5
 15 6
 16 130
 17 "
 18 user=root
 19 passwd=centos
 20 for ID in $IPLIST;do
 21 IP=$NET.$ID
 22 expect <<EOF
 23 set timeout 20
 24 spawn ssh $user@$IP
 25 expect {
 26         "yes/no" { send "yes\n";exp_continue }
 27         "password" { send "$passwd\n" }                                          
 28 }
 29 expect "]#" { send "echo Just a test,---------------------\n"  }
 30 expect "]#" { send "exit\n" }
 31 expect eof
 32 EOF
 33 done
```

set timeout 20   这个是说expect的交互式等待多久，超过20秒expect看不到设定的字符串就会退出

一个spawn语句下面写一个expect，目前来看这样比较好：

```
expect <<EOF
spawn xxxxx
expect {
	"xxx" { send "xxx";exp_continue }
	"xxx" { send "xxx" }
}
spawn xxxxx
expect {
	"xxx" { send "xxx";exp_continue }
	"xxx" { send "xxx" }
}
EOF
```

