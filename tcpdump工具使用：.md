tcpdump工具使用：

格式说明：

```
tcpdump [option] [not] protocol directory type
```

option：

```
-nn 以数字的方式显示抓包信息，用！
-c  指定抓包的个数，用！ 还有一个-s是指定包的长度的，默认包长度是65535字节，包的长度越小越好，但是不能影响一个正常协议的包的长度
-i 指定网卡
```

protocol：

```
tcp
udp
icmp
...
这个写各种协议的名称
```

directory：

```
src
dst
src or dst
这个是代表数据包的方向
```

type：

```
net  网段 比如 10.0.0.0/24
host 主机IP 比如 10.0.0.3
port 端口	比如 22,23
portrange 端口范围，应该是这样 22-25
```

例子：

```
tcpdump -c 10 -nn -i eth0 icmp and src or dst 10.0.0.7 
```

![image-20200917163348314](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200917163348314.png)