## awk：

报告生成器，格式化生成文本，GNU AWK，gawk

### awk的工作过程

![image-20200829162438311](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200829162438311.png)

```
awk [options]  'program' var=value file
awk [options] -f programfile var=value file
```

#### options:

* -F “分隔符” 指明输入时用到的字段分隔符，默认的分隔符是若干个连续的空白符
* -v var=value 变量赋值



#### program 一般由三部分组成

* BEGIN数据块：在读入文件之前运行
* 模式匹配数据块：读入文件后匹配的规则
* END数据块：规则匹配之后进行统计排序时运行

#### program格式：

​	pattren{action statements}

​	pattren :比如 /匹配字符串/  

​	action statements :需要执行的操作，比如print,printf

#### 常见符号：

$0：这个代表输入的这一行内容

$1,$2....$N ：这个代表第几个段的字符串，比如``192.168.1.1 hhh www aaaa` 用空格分隔符分隔的话，$1是192.168.1.1，$2是 hhh 

常见例子：

统计

将登陆失败日志中登录次数最多的用户名和登录次数显示出来

```
lastb | awk '!/^btmp/ && !/^$/{arr[$1]++}END{for (i in arr) {print arr[i],i}}'|sort -nr|head -1
```

统计每个硬盘的利用率

```
df -h|awk -F ' +|%' 'NR!=1{print $1,$5}'
```

取nginx的访问日志的IP和时间

```
awk -F "[[ ]" '{print $1,$5}' nginx.access.log-20200428
```

取出ifconfig或者ip中的ipv4地址

```
ip a show eth0 | awk -F " +|/"  'NR==3{print $3}'
ifconfig eth0 | awk '/netmask/{print $2}'
```

#### awk变量

awk的变量分为内置变量和自定义变量

内置变量：

FS ：输入字段分隔符，默认为空白字符，功能相当于 -F

​	**注意：默认情况下FS是空格，此时如果输入行的行首有空格，将会被默认为没有东西；如果使用了-F或者 -v FS='XX' 命令设置了FS，那么情况就不一样了，此时行首的空格将会被认为是一个字段！！！**

OFS：输出字段分隔符

RS：输入记录分隔符，也就是输入换行符

ORS：输出记录分隔符，也就是输出换行符

NF：字段数量

NR：记录的编号，也就是输入的行数

#### awk的操作

if...else..   while do   for do  

#### awk的数组

awk的数组都是关联数组，awk的数组是从序号1开始排序的，但是也可以设置成序号0 arr[1]....arr[N]   arr[0]是我们自己设置的，默认是从1开始

#### awk的字符串可以与数字相加

这是很重要的一点，arr[1]="" 空字符串

arr[1]=arr[1]+1  arr[1]=1

这里意思是说，awk的空字符串或者字符串在与数字相加时，默认为0，利用这一点以及awk数组没有声明直接用时默认为空字符串的特性，可以实现统计功能

```awk
awk '{arr[$1]++}END{for (i in arr) print{i,arr[$i]}}'
```

上面这个命令可以统计每行中第一个字段出现的次数并且打印出字段1和次数

下面这个例子更特殊，每行不是同样的字段数，需要每行循环

```
[root@centos7 data]#cat name.txt 
Allen Phillips
Green Peter
William Aiden James Lee
Angel Devil
Jack Tyler Kevin
Kevin
[root@centos7 data]#awk '{for (i=1;i<=NF;i++) {count[$i]++}}END{for (j in count) {print j,count[j]}}' name.txt 
Tyler 1
Angel 1
James 1
William 1
Green 1
Jack 1
Phillips 1
Peter 1
Kevin 2
Devil 1
Lee 1
Allen 1
Aiden 1

```

#### awk的内置函数

**split**：分隔函数，这个函数可以将字符串按照设定的分隔符进行分隔，并且可以将分隔后的小字符串赋值给数组。

```
[root@centos8 ~]#awk -v ts="大娃:二娃:三娃" 'BEGIN{ split(ts,huluwa,":");for (i in huluwa){print i,huluwa[i]} }'
1 大娃
2 二娃
3 三娃

```

#### awk的三元运算

```
[root@centos8 data]#awk -F: '{usertype=$3<500?"系统用户":"普通用户";print $1,usertype}' /etc/passwd
root 系统用户
bin 系统用户
daemon 系统用户
adm 系统用户
lp 系统用户
sync 系统用户
shutdown 系统用户
halt 系统用户
mail 系统用户
operator 系统用户
games 系统用户
ftp 系统用户
nobody 普通用户
dbus 系统用户
systemd-coredump 普通用户
systemd-resolve 系统用户
tss 系统用户
polkitd 普通用户
unbound 普通用户
sssd 普通用户
sshd 系统用户
chrony 普通用户
lv 普通用户
lv123 普通用户
saslauth 普通用户
apache 系统用户
archlinux 普通用户
tcpdump 系统用户
```

如上面的例子，条件? 结果1 : 结果2 ，同时，usertype也接收了三元运算的结果

#### 打印奇偶行

利用了awk的两条规则：

1.在awk中，如果省略了模式对应的动作，当前行满足模式时，默认动作是打印整行，也就是'{print $0}'

2.在awk中，0或者空字符串表示“假”，非0值或者非空字符串表示“真”

打印奇数行：

```
[root@centos8 data]#cat test.txt 
第1行
第2行
第3行
第4行
第5行
[root@centos8 data]#awk 'i=!i' test.txt 
第1行
第3行
第5行

```

打印偶数行：

```
[root@centos8 data]#cat test.txt 
第1行
第2行
第3行
第4行
第5行
[root@centos8 data]#awk '!(i=!i)' test.txt 
第2行
第4行

```

这里面其实每个条件应该是这么写的

```
awk 'i=!i是0吗？是的话就打印$0，不是的话就不打印'   i=!i 的意思是取反，i=0的时候，i=!i就是1，i=1的时候，i=!i就是0
```

