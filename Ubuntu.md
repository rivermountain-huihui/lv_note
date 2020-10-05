### Ubuntu

1.修改网卡名称

```
vim /etc/default/grub
#找到下面这行，修改成GRUB_CMDLINE_LINUX="net.ifnames=0"
GRUB_CMDLINE_LINUX=""
grub-mkconfig -o /boot/grub/grub.cfg
重启 reboot
```

2.修改网卡信息

```
vim /etc/netplan01-netcfg.yaml
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      addresses: [10.0.0.100/24]
      gateway4: 10.0.0.1
      nameservers:
        addresses: [180.76.76.76, 233.6.6.6]
改成这样，注意这个yaml格式很严格，上面的内容格式一个都不能错，gateway4就是有个4
然后需要命令
netplan apply 生效网卡信息
```

3.Ubuntu的命令提示符修改

```
vim /root/.bashrc
找到
#force_color_prompt=yes
去掉注释#就行，自带的颜色挺好看
```

4.Ubuntu修改软件源

这个是阿里巴巴的指导文档
https://developer.aliyun.com/mirror/ubuntu?spm=a2c6h.13651102.0.0.53322f70fghx56

```
vim /etc/apt/sources.list
把里面的
deb httpxxxxx
全改成国内的源的地址，注意全部都要改了
deb http://mirrors.aliyun.com/ubuntu/ bionic multiverse
改完之后
apt update 命令生效
```

