## centos的软件包管理

1.rpm是什么，产生的契机是什么，解决了什么问题，如何使用的

2.yum是什么，如何产生的，解决了什么问题，如何使用

### 1. rpm

#### 1.1 rpm是什么

rpm是一个软件包管理器，应用于RedHat系的Linux系统上。

英文全称是：**RPM is  Package Manager**

#### 1.2 产生的契机是什么

Linux的二进制程序安装、库文件管理、配置文件管理很繁琐，不方便，因此人们需要一个便捷的软件管理工具

#### 1.3 怎么使用

rpm [options]...

**常用选项：**

```
-i ：install		安装	
-v : verbose	可观察安装过程
-h : 			显示安装进度
-e : 			卸载软件包
-q : query		查询
-qi : 			查询软件包的详细信息
-ql : 			列出RPM包中所包含的文件
-qf ： 			查询某个文件是哪个RPM包生成的
-qa :			列出所有已安装的包
```

常用组合：

```
#软件包的安装
rpm -ivh xxx.rpm
#查询已经安装的软件包
rpm -qa xxx.rpm
#查询安装包的详细信息
rpm -qi xxx.rpm
#查询文件是哪个软件包产生的
rpm -qf filename
#查询软件包都包含了什么文件
rpm -ql xxx.rpm
#卸载rpm软件包
rpm -e xxx.rpm
```



#### 1.4 缺陷是什么

软件包和软件包在安装的时候是有依赖性的，rpm只能安装单个的软件包，它无法处理依赖关系包，因此在安装某些软件包的时候很麻烦，需要自己一个一个的先安装前置软件包。

### 2. yum

#### 2.1 yum是什么

yum是一个软件包管理器，它基于RPM，可以通过http协议、ftp协议从网络主机获取软件包，或者通过本地软件池获取软件。

yum的英文全称是：**Yellow Dog Updater Modified**

#### 2.2 yum产生的契机是什么

yum解决了linux安装软件包时的软件依赖问题

#### 2.3 yum怎么用

yum的使用分两部分，第一部分是配置yum源，第二部分是命令。

##### 2.3.1 yum的基本原理

yum除了软件本身，还需要软件仓库和配置文件。软件仓库可以是网络主机，也可以是本地主机。

软件仓库中需要有两部分东西，第一个是**元数据（metedata）**,另一个是软件包。

元数据中记录了这个仓库中所有的软件包的详细信息，以及最最重要的，软件包之间的依赖关系，yum在访问一个仓库的时候，首先就要申请下载元数据，然后根据元数据进行软件包管理。

##### 2.3.1 我怎么用yum

1.3.1.1 配置yum仓库

```
#配置yum源，注意文件名称无所谓，后缀一定要是.repo
vim /etc/yum.repos.d/xxx.repo
#yum的全局配置文件，一般不配置
/etc/yum.conf
```

2.3.1.2 yum命令

```
yum [options] [command] [package]
1.安装
yum install package1
yum groupinstall group1 安装程序组
2.查找某个文件或命令属于哪个包
yum provides xxx 
3.卸载包
yum remove package
3.查找和显示软件包
yum list 
yum info package
yum repolist 
4.清楚缓存
yum clean package
yum clean;yum clean all
5.历史记录和回滚
yum history
yum history list
yum history info id
yum history undo id #撤销某次操作
yum history redo id #重做某次操作
yum history help #查看一下history的参数
```

#### 2.4 yum的缺陷

如果某次安装的过程中出现意外中止的话，下次重启将无法解决包的依赖关系，因此无法分析上次安装成功与否。dnf可以解决这个问题，dnf用法与yum相同

### 3. dnf

使用dnf命令下载仓库，制作本地软件仓库

```
#注意，使用这个命令之前要先配置好yum源，指定好镜像网站
dnf reposync --repoid=repoid --download-metadata -p /data/xxx  
这里的repoid是你要下载的仓库名称，-p后面跟你要把仓库下载到哪里
```

