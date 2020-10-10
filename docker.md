

# Docker

**学习内容：**

> 掌握docker安装
>
> docker常用命令
>
> docker镜像和仓库相关内容

**Docker的应用场景：**

> web应用的自动化打包和发布
>
> 自动化测试和持续集成、发布
>
> 在服务型环境中部署和调整数据库或其他的后台应用
>
> 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PassS环境

## **Docker的概念**：

| Docker镜像（images）      | Docker镜像是用于创建Docker容器的模板，比如Ubuntu系统         |
| ------------------------- | ------------------------------------------------------------ |
| Docker容器（container）   | 容器是独立运行的一个或一组应用，是镜像运行时的实体           |
| Docker客户端（client）    | Docker客户端通过命令行或者其他工具使用Docker SDK与Docker的守护进程通信 |
| Docker主机（host）        | 一个物理或者虚拟的机器，用于执行Docker守护进程和容器         |
| Docker 注册处（Registry） | Docker仓库用来保存镜像，可以理解为代码控制中的代码仓库。一个Docker Registry中可以包含多个仓库（Repository）；每个仓库可以包含多个标签（tag）；每个标签对应一个镜像 |
| Docker Machine            | Docker Machine是一个简单Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、Digital Ocean |

> linux的虚拟化使用了内核中的两个功能
>
> namespaces
>
> chroot



> 容器的资源控制
>
> Control Groups （cgroups）



> LXC LinuX Container
>
> * lxc-create,template
>
> LXC的安装需要配置模板；LXC的迁移复杂；

**容器是linux中的技术，docker是对这个技术的易用和简化的软件**

> Docker 
>
> * 一个容器中运行一个进程
> * 分层镜像
> * 容器变得跟进程一样了，具有生命周期
> * 外置一个存储，容器们都去把数据存储在外存储上
> * 需要容器编排工具，谷歌的kubernetes就是这个
> * CNCF CloudNativeComputingFoundation 
> * OCI Open Container Initiative
> * OCF Open Container Format
> * lxc --> libcontainer引擎 -->runC
> * kubernetes + docker就是现在的用法



> 镜像是静态的程序
>
> 容器是动态的进程

## Docker的安装

> 自己去阿里云下一个仓库，然后屏蔽其他仓库，只使用阿里云的仓库下docker-ce。还有一步很重要的是，先把Centos-Base.repo这个系统自带的仓库放回/etc/yum.repos.d/下面，安装docker-ce有些依赖包在extra源里面



> 配置文件需要自己建
>
> /etc/docker/daemon.json



> Docker镜像加速
>
> 注册阿里云的账号，获得加速器
>
> 中国科技大学
>
> docker cn

注册一个阿里云的加速器，并把这个地址写入到创建好的/etc/docker/daemon.json文件中

> {
>
> "registry-mirrors": ["https://6kqxd7ws.mirror.aliyuncs.com"]
>
> }

## **Docker命令**


> 启动docker服务
>
> systemctl start docker
> 

> 查找镜像
>
> docker search name

> 下载镜像
>
> docker pull name  或者  docker image pull name

> 列出当前主机上的镜像
>
> docker images   或者 docker image ls


> 删除镜像
>
> docker rmi name  或者 docker image rm name 



> 创建容器
>
> docker create name  或者 docker container name



> 运行容器
>
> docker run --name web1  name:lastest     
>
> docker run 的命令
>
> docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
>
> --name 指定容器的名字
>
> -d detach 后台运行并打印容器ID到屏幕
>
> -i interactive 交互式方式 这个命令需要指定终端
>
> -t tty  Allocate a pseudo-TTY
>
> /bin/sh  指定一个shell，这个放在最后面，不加起不来的
>
> 镜像不必提前下载，当你运行容器时，docker会根据你提交的镜像名称自动下载



> 查看docker容器的进程
>
> docker ps   还有 docker ps -a
>
> docker inspect name



> 在docker中运行命令
>
> docker exec  -it name /bin/sh



> 在docker中查看容器的日志
>
> docker logs name



> 停止容器
>
> 在容器中退出，或者使用 docker stop name
>
> 然后 docker rm name 进行容器删除
>
> docker kill name 强行停止容器



> 加载一个本地镜像到docker
>
> docker load -i xxx.tar.xx  注意是个tar文件

## Docker的镜像





> Docker镜像含有启动容器所需要的文件系统及其内容，因此，其用于创建并启动docker容器
>
> 分层结构
>
> 联合挂载
>
> 支持分层结构、联合挂载的文件系统
>
> 最上层才是可写层，下面的层都是只读的



> Docker Registry 
>
> 启动容器时，docker daemon会试图从本地获取相关的镜像；本地镜像不存在时，其将从Registry中下载该镜像并保存到本地；
>
> Registry包含repository和index；
>
> Repository包含TAG，一个软件可以有多个TAG，但是一个TAG只能属于一个软件



> Docker Hub
>
> Docker 默认的拉取镜像地址

> 镜像的制作
>
> * Dockfile
> * 基于容器 打开一个容器，完成需要的操作，然后执行命令：docker commit 参数
>
> 镜像的push，需要docker hub的账号
>
> 镜像的打包和导入
>
> docker save
>
> docker load











