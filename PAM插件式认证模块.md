# PAM

Pluggable Authentication Modules 插件式认证模块

PAM认证顺序：开启应用程序-->检查PAM配置文件-->调用pam_*.so程序进行认证-->返回认证结果到应用程序-->应用程序根据返回结果进行操作

注1：一个应用程序是否调用了PAM可以通过`ldd which "命令"`来进行判断，调用了pam.so模块的应用程序就是用PAM进行认证管理的。

注2：*.so文件（shared object）是一个类似动态链接库的文件，里面是C/C++程序，函数库，它可以被其他程序调用里面的程序代码

配置文件路径：/etc/pam.d/  /etc/security/

 .so文件路径：/lib64/security/

配置文件的格式：

| Type     | control  | modul-path                                       | arguments        |
| -------- | -------- | ------------------------------------------------ | ---------------- |
| 模块类型 | 控制方式 | 模块绝对路径或者/lib64/security/下可以写模块名称 | 传递给模块的参数 |

**模块的类型：**

authentication；

account；

session；

password；

**模块的控制方式：**

required

request

sufficient

optional

include

官方文档：http://www.linux-pam.org/Linux-PAM-html/sag-configuration-file.html

常用模块：

| 模块名称          | 模块功能                                                     |
| ----------------- | ------------------------------------------------------------ |
| pam_shells.so     | 检查有效shells                                               |
| pam_securetty.so  | 只允许root用户在/etc/securetty列出的安全终端上登录           |
| pam_nologin.so    | 如果/etc/nologin文件存在，将导致非root用户不能登录；当非root用户登录时会显示/etc/nologin中的内容提示 |
| pam_limits.so     | 在用户级别实现对其可用资源的限制，这个模块有自己的配置文件，用于对用户做资源限制，比如打开多少进程、打开多少文件等 |
| pam_succeed_if.so | 根据参数中的所有条件都满足才返回成功                         |

注1：pam_limilts.so还有一个ulimit命令

注2：pam_limits.so的配置文件在/etc/security/limits.conf  /etc/security/limits.d/*.conf

pam_limits.so的配置文件格式：

```
<domain> 	<type> 		<item> 		<value>
例如：
root		soft 		nproc		1024
#对root用户限制，软限制，用户可以自己更改自己的限制，限制内容是打开的进程个数，个数是1024
例如：
*			hard		nofile		100000  
#对所有用户，硬限制，用户不能自己更改，限制内容是打开的文件个数，个数是1000000
```

下图为PAM的工作流程：

```
+----------------+
  | application: X |
  +----------------+       /  +----------+     +================+
  | authentication-[---->--\--] Linux-   |--<--| PAM config file|
  |       +        [----<--/--]   PAM    |     |================|
  |[conversation()][--+    \  |          |     | X auth .. a.so |
  +----------------+  |    /  +-n--n-----+     | X auth .. b.so |
  |                |  |       __|  |           |           _____/
  |  service user  |  A      |     |           |____,-----'
  |                |  |      V     A
  +----------------+  +------|-----|---------+ -----+------+
                         +---u-----u----+    |      |      |
                         |   auth....   |--[ a ]--[ b ]--[ c ]
                         +--------------+
                         |   acct....   |--[ b ]--[ d ]
                         +--------------+
                         |   password   |--[ b ]--[ c ]
                         +--------------+
                         |   session    |--[ e ]--[ c ]
                         +--------------+
```

