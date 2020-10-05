# 						Linux三剑客

### grep：global search regular expression and print tht line

```
grep [OPTION] PATTERN [FILE]
```

常见选项：

```
-color=auto 对匹配到的文本着色显示
-m	#	匹配#此后停止
-v		显示不被pattern匹配到的行，也就是反选
-i		忽略字符大小写
-n		显示匹配的行号
-c		统计匹配的行数
-o		仅显示匹配到的字符串
-q		静默模式，不输出任何信息
-A	#	after，后#行
-B	#	before，前#行
-C	#	context，前后各#行
-e 		实现多个选项间的逻辑or关系，如：grep -e 'cat' -e 'dog' file.txt
-w		匹配整个单词
-E		使用ERE，相当于egrep，扩展正则表达式
-F		不支持正则表达式，相当于fgrep
-f	file 根据模式文件处理
-r		递归查询目录中的文件，但不处理软链接
-R		递归查询目录中的文件，但处理软链接
```

#### 范例：算出所有人的年龄之和

第一种方法：

```
[root@centos7 data]#cat age.txt 
xiaoming=20
xiaohong=21
xiaoqiang=22

[root@centos7 data]#grep -Eo '[0-9]+' age.txt | paste -s -d + | bc
63
这里的重点是：-o 只显示匹配到的字符；paste命令是否知道 -s 行转列 -d 替换分隔符
```

第二种方法：

```
[root@centos7 data]#grep -Eo '[0-9]+' age.txt |tr -s "\n" + | grep -Eo '.*[0-9]+'|bc
63
```

第三种方法，使用awk：

```
[root@centos7 data]#awk -F "=" '{for (i=1;i<=NR;i++);sum+=$2}END{print sum}' age.txt 
63
```

#### 范例：取出分区利用率最大的值

第一种方法：

```
[root@centos7 data]#df -h|grep '^/dev/sd'|grep -Eo '[0-9]{,3}%'|tr -d %|sort -nr| head -n1
27
```

第二种方法：

```
[root@centos7 data]#df -h|grep '^/dev/sd'|tr -s " " % | cut -d % -f 5|sort -nr| head -n1
27
```

第三种方法：

```
[root@centos7 data]#df -h|grep '^/dev/sd'|grep -Eo '[0-9]{,3}%'|grep -Eo '.*[0-9]'|sort -nr|head -n1
27
```

#### 范例：哪个IP和当前主机连接数最多，取出前三位

第一种方法：

```
[root@centos7 data]#ss -nt | grep '^ESTAB' | tr -s " " : |cut -d : -f 6|sort|uniq -c|sort -nr|head -n3
      2 10.0.0.4
      1 10.0.0.2
```

第二种方法：awk

```
[root@centos7 data]#ss -nt|awk -F " +|:" 'NR!=1{arr[$6]++}END{for (i in arr)print arr[i],i}' | sort -nr|head -n3
2 10.0.0.4
1 10.0.0.2
```

范例：连接状态的统计



```
[root@centos7 data]#ss -nta | tail -n +2 | cut -d " " -f 1 | sort|uniq -c|sort -nr
      8 LISTEN
      3 ESTAB
```

第二种方法：

```
[root@centos7 data]#ss -nta | grep -v '^State' | cut -d " " -f 1|sort|uniq -c|sort -nr
      8 LISTEN
      3 ESTAB
```

第三种方法：

```
[root@centos7 data]#ss -nta | awk 'NR!=1{arr[$1]++}END{for (i in arr)print arr[i],i}'
8 LISTEN
3 ESTAB
```

### sed：stream editor行编辑器

有个缓冲区：模式空间 Pattren Space

一次处理一行，执行速度快，打开大文件也不会卡

基本用法：

```
sed [option]... 'script;script;.....' inputfile
```

常用选项：

```
-n			不输出模式空间内容到屏幕，即不自动打印
-e			多点编辑
-f	file	从指定文件中读取编辑脚本
-r,-E		使用扩展正则表达式
-i,-i.bak	备份文件并原处编辑
```

script格式：

```
‘地址命令’
```

地址格式：

```
1.不给地址：对全文进行处理
2.单地址：
	#：指定的行，$:最后一行
	/pattern/：被此处模式所能匹配到的每一行
3.地址范围
	#,#			从第#行匹配到第#行，3,6 从第3行到第6行
	#，+#	   从#行到+#行，3，+4 表示从3行到第7行
	/part1/,/part2/ 从匹配到part1到匹配到part2
4.步进：~
	1~2 奇数行
	2~2 偶数行
```

命令：

```
p			打印当前模式空间内容，追加到默认输出之后
Ip			忽略大小写输出
d			删除模式空间匹配的行，并立即启用下一轮循环
a [\]text	在指定行后面追加文件，支持使用\n实现多行追加
i [\]text	在指定行前面追加文件
c [\]text	替换行为单行或多行文本
w file		保存模式匹配的行至指定文件
r file		读取指定文件的文本至模式空间中匹配到的行后
=			为模式空间中的行打印行号
！			为模式空间中的匹配行取反
```

#### 范例：获取分区利用率

```
[root@centos7 ~]#df -h | sed -En '/^\/dev/s@.*([0-9]+)%.*@\1@p'
2
5
1
```

范例：获取IP地址：

方法一：sed

```
[root@centos7 ~]#ifconfig eth0 | sed -En '2s/[^0-9]+([0-9.]+).*/\1/p'
10.0.0.4
```

方法二：sed

```
[root@centos7 ~]#ifconfig eth0 | sed -En '2s/[^0-9]+([0-9.]{7,15}).*/\1/p'
10.0.0.4
```

方法三：awk

```
[root@centos7 ~]#ifconfig eth0 | awk 'NR==2{print $2}'
10.0.0.4
```



### awk：报告生成器，gawk

