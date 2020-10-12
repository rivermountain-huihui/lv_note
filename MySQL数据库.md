##   MySQL是什么

* 关系型数据库管理软件--RDBMS

  * 数据库是数据库，MySQL是管理数据库的软件
  * 实体-联系模型E-R
    * 实体Entity：客观存在并且可以相互区分的客观事物或抽象事件称为实体
    * 属性：实体所具有的特征或性质
    * 联系：联系是数据之间的关联集合，是客观存在的应用语义链
      * 实体和属性之间的联系
      * 实体和实体之间的联系

* 数据冗余和性能的矛盾关系，冗余越小，在查询数据的时候就会越消耗性能

* 数据库的范式，有很多范式，但是一般数据只需满足第三范式（3NF）即可

  * 第一范式：1NF（normal form）

    无重复的列，每一列都是不可分割的基本数据项，同一列中不能有多个值，即实体中的某个属性不能有多个值或者不能有重复的属性，确保每一列的原子性。除去同类型的字段，就是无重复的列。

    总结：消除重复列

    比如下面表中xiaolv，他的年龄只能有一个，不能有两个

  |  name  |           age           |
  | :----: | :---------------------: |
  | xiaoli |           20            |
  | xiaolv | 20,22（两个值是错误的） |

  * 第二范式

    第二范式必须先满足第一范式，属性完全依赖于主键，要求表中的每个行必须可以被唯一的区分，通常为表加上每行的唯一标识PK，非PK的字段需要与整个PK有直接的相关性

    总结：消除部分依赖

    比如下面的ID就是主键，每行都不相同，name和age字段依赖于ID

  |  ID  |  name   | age  |
  | :--: | :-----: | :--: |
  |  1   | xiaolv  |  20  |
  |  2   | xiaohui |  21  |
  |  3   | xiaozhe |  20  |

  * 第三范式

    第三范式必须先满足第二范式，不依赖于其他非主属性。第三范式要求一个数据表中不包含已在其他表中已包含的非主键字信息，非PK的字段间不能有从属关系

    总结：消除全部依赖

* mysql客户端工具：

  * mysql
  * mysqladmin

  ```
  mysql -uroot -ppasswd -Pport
  
  mysql [options] [database]
  -A,--no-auto-rehash 禁止补全
  -u,--user= 用户名，默认为root
  -h,--host= 服务器名，默认为localhost
  -p,--password 用户密码，默认为空密码
  -P,--port= 服务器端口
  -S,--socket 指定连接socket文件路径
  -D，--datebase= 指定默认数据库
  -C，--compress 启用压缩
  -e "SQL" 执行SQL命令
  -V,--version 显示版本
  -v,--verbos 显示详细信息
  --print-defaults 获取程序默认使用的配置
  ```

  ```
  mysqladmin -uroot -ppasswd ping
  mysqladmin [options] command command ...
  ```
  
* 服务器端配置

服务器端（mysqld）配置：

1.命令行选项

2.配置文件

* /etc/my.cnf

* /etc/my.cnf.d/mysql-server.cnf

  * ```
     13 [mysqld]
     14 datadir=/var/lib/mysql
     15 socket=/var/lib/mysql/mysql.sock
     16 log-error=/var/log/mysql/mysqld.log
     17 pid-file=/run/mysqld/mysqld.pid
    ```

socket地址：

* ip socket地址：监听在tcp的3306端口，支持远程通信，侦听3306/tcp端口可以绑定有一个或全部接口的IP上
* Unix sock：监听在sock文件上，仅支持本机通信，如：/var/lib/mysql.sock,说明，host为localhost时自动使用unix sock

## MySQL数据库怎么安装

在实际生产环境中大多使用编译安装或二进制安装

* 编写脚本，实现二进制方式安装MySQL

MySQL的三大分支

* mysql
* mariadb
* percona server

数据库应该尽量少被访问，尽量分担数据库的压力

脚本在二进制安装.md中

* yum install 
* 编译安装
  * 建议内存4G以上
  
  * 下载并解压源码包
  
  * 源码编译安装，使用cmake，指定一些环境
  
  * ```
    cd mariadb-10.2.18/cmake . \
    -DCMAKE_INSTALL_PREFIX=/app/mysql \
    -DMYSQL_DATADIR=/data/mysql/ \
    -DSYSCONFDIR=/etc/  \
    -DMYSQL_USER=mysql \
    -DWITH_INNOBASE_STORAGE_ENGINE=1 \
    -DWITH_ARCHIVE_STORAGE_ENGINE=1 \
    -DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
    -DWITH_PARTITION_STORAGE_ENGINE=1  \
    -DWITHOUT_MROONGA_STORAGE_ENGINE=1 \
    -DWITH_DEBUG=0 \
    -DWITH_READLINE=1 \
    -DWITH_SSL=system \
    -DWITH_ZLIB=system\
    -DWITH_LIBWRAP=0 \
    -DENABLED_LOCAL_INFILE=1  \
    -DMYSQL_UNIX_ADDR=/data/mysql/mysql.sock \
    -DDEFAULT_CHARSET=utf8 \
    -DDEFAULT_COLLATION=utf8_general_ci
    make && make install 
    ```
  
    提示：如果出错，执行rm -f CMakeCache.txt
  
    剩下的步骤与二进制安装相同
* 二进制安装，相比编译安装就是少了编译过程
  * 1.创建mysql组和用户
  * 2.创建数据库存放目录，修改这个目录所有者、所有组的信息
  * 3.下载源码，解压到/usr/local下
  * 4.创建软链接 ln -s /usr/local/mysqlxxxx   mysql
  * 5.配置文件复制到/etc/my.cnf
  * 6.使用自带脚本生成数据库文件
  * 7.使用自带脚本制作开机启动脚本
  * 8.配置开机启动
  * 9.启动安全加固脚本

1.MySQL账户

2.MySQL配置文件

3.MySQL工具

4.MySQL数据库文件

5.MySQL登录方式文件

mysql

mysqladmin



## SQL语言

SQL的注释：

* -- 注释内容 单行注释，注意有空格
* /**/ 多行注释

MySQL注释：

* "#"号后面跟注释内容，有空格

SQL关键字建议大写

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'passwd' WITH GRANT OPTION;
#这个是指定访问的IP和账号
delete from user where host=xxx and user=xxx;
#这个是删除一个用户
flush privileges;
#刷新数据库
```

数据库的组件：

数据库、表、索引、视图、用户、存储过程、函数、触发器、事件调度器

命名规则：

* 必须以字母开头，可包括数字和三个特殊字符（# _ $）
* 不要使用MySQL的保留字

SQL的语句分类：

* DDL:数据定义语言 Data Defination Language

  表：二维关系

  设计表：遵循规范

  定义：字段，索引

  ​	字段：字段名，字段数据类型，修饰符

  ​	约束，索引：应该创建在经常用作查询条件的字段上

  * CREATE

    第一种创建表的方式：直接创建

    ```
    (root@localhost) [ahui]>create table if not exists student (
    id int UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(20) NOT NULL,
    age tinyint UNSIGNED,
    gender ENUM('F','M') default 'M')ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;
    (root@localhost) [ahui]>desc student;
    +--------+---------------------+------+-----+---------+----------------+
    | Field  | Type                | Null | Key | Default | Extra          |
    +--------+---------------------+------+-----+---------+----------------+
    | id     | int(10) unsigned    | NO   | PRI | NULL    | auto_increment |
    | name   | varchar(20)         | NO   |     | NULL    |                |
    | age    | tinyint(3) unsigned | YES  |     | NULL    |                |
    | gender | enum('F','M')       | YES  |     | M       |                |
    +--------+---------------------+------+-----+---------+----------------+
    4 rows in set (0.00 sec)
    (root@localhost) [ahui]>insert student (name,age)values('xiaolv',20);
    (root@localhost) [ahui]>insert student (name,age,gender)values('xiaohong',20,'f');
    (root@localhost) [ahui]>select * from student;
    +----+----------+------+--------+
    | id | name     | age  | gender |
    +----+----------+------+--------+
    | 10 | xiaolv   |   20 | M      |
    | 11 | xiaohong |   20 | F      |
    +----+----------+------+--------+
    2 rows in set (0.00 sec)
    ```

    关于AUTO_INCREMENT，通过更改系统参数可以修改步进值和默认起始的值

    ```
    (root@localhost) [ahui]>show variables like 'auto_increment%';
    +--------------------------+-------+
    | Variable_name            | Value |
    +--------------------------+-------+
    | auto_increment_increment | 10    |
    | auto_increment_offset    | 3     |
    +--------------------------+-------+
    2 rows in set (0.00 sec)
    #上面的参数是修改过的，默认都是1
    (root@localhost) [ahui]>set @@auto_increment_increment=10;
    (root@localhost) [ahui]>set @@auto_increment_offset=3;
    (root@localhost) [ahui]>desc autoincre;
    (root@localhost) [ahui]>desc autoincre;
    +-------+---------+------+-----+---------+----------------+
    | Field | Type    | Null | Key | Default | Extra          |
    +-------+---------+------+-----+---------+----------------+
    | col   | int(11) | NO   | PRI | NULL    | auto_increment |
    +-------+---------+------+-----+---------+----------------+
    1 row in set (0.00 sec)
    
    (root@localhost) [ahui]>select * from autoincre;
    +-----+
    | col |
    +-----+
    |   3 |
    |  13 |
    |  23 |
    |  33 |
    +-----+
    4 rows in set (0.00 sec)
    ```

    第二种创建表的方式：通过查询现存表创建：查询到的数据之间插入表中成为新表的数据

    ```
    (root@localhost) [ahui]>create table human select user,host from mysql.user;
    (root@localhost) [ahui]>select * from human;
    +---------------+-----------+
    | user          | host      |
    +---------------+-----------+
    | root          | 10.0.0.%  |
    | mysql.session | localhost |
    | mysql.sys     | localhost |
    | root          | localhost |
    +---------------+-----------+
    4 rows in set (0.00 sec)
    ```

    第三种方式：通过复制现存表的表结构创建，但是不复制数据

    ```
    (root@localhost) [ahui]>create table teacher like student;
    (root@localhost) [ahui]>desc teacher;
    +--------+---------------------+------+-----+---------+----------------+
    | Field  | Type                | Null | Key | Default | Extra          |
    +--------+---------------------+------+-----+---------+----------------+
    | id     | int(10) unsigned    | NO   | PRI | NULL    | auto_increment |
    | name   | varchar(20)         | NO   |     | NULL    |                |
    | age    | tinyint(3) unsigned | YES  |     | NULL    |                |
    | gender | enum('F','M')       | YES  |     | M       |                |
    +--------+---------------------+------+-----+---------+----------------+
    4 rows in set (0.00 sec)
    (root@localhost) [ahui]>select * from teacher;
    Empty set (0.00 sec)
    #表中没有数据，是空的，只有数据表的结构
    ```

    查看engine

    ```
    (root@localhost) [ahui]>show engines;
    ```

    查看表：

    ```
    (root@localhost) [ahui]>show tables;
    ```

    查看表结构：

    ```
    (root@localhost) [ahui]>desc student;
    ```

    查看创建表命令：

    ```
    (root@localhost) [ahui]>show create table student;
    ```

    查看表状态：

    ```
    (root@localhost) [ahui]>show table  status like 'student'\G
    *************************** 1. row ***************************
               Name: student
             Engine: InnoDB
            Version: 10
         Row_format: Dynamic
               Rows: 2
     Avg_row_length: 8192
        Data_length: 16384
    Max_data_length: 0
       Index_length: 0
          Data_free: 0
     Auto_increment: 12
        Create_time: 2020-09-22 20:33:27
        Update_time: 2020-09-22 20:37:04
         Check_time: NULL
          Collation: utf8_general_ci
           Checksum: NULL
     Create_options: 
            Comment: 
    1 row in set (0.00 sec)
    #\G是竖着显示内容
    ```

    查看数据库中全部表的状态：

    ```
    (root@localhost) [ahui]>show table status from ahui ;
    ```

  * DROP

  ```
  (root@localhost) [(none)]>drop database if exists ahui;
  ```

  * ALTER

    add,change,modify

  ```
  (root@localhost) [(none)]>alter database ahui character set utf8;
  mysql> alter table info change name1 name varchar(20);
  ```

  查看所有数据库

  ```
  SHOW DATABASES;
  ```

  

* DML：数据库操纵语言 Data Manipulation Language
  * INSERT
  
    ```
    insert table_name [(column1,...)] values (value1,...) (value2...)...
    ```
    ```
    mysql>insert info (id,name,gender,age) values (4,'ming','M',24);
    ```
  
  
  
  * DELETE
  
    删除表中的数据，但不会自动缩减数据文件的大小
  
    ```
    DELETE [] FROM TABLE_NAME
    	[WHERE CONDITIONS]
    	[ORDER BY]
    	[LIMIT ROW_COUNT]
    	可以先排序再指定删除的行数
    ```
  
    ```
    mysql>delete from info where id =4;
    ```
  
    注意一定要有限制条件，否则将清空表中的所有数据
  
    如果想清空表，保留表结构，也可以使用下面语句，此语句会自动缩减数据文件的大小
  
    ```
    TRUNCATE TABLE TABLE_NAME
    ```
  
    缩减表大小
  
    ```
    OPTIMIZE TABLE TABLE_NAME
    ```
  
  * UPDATE
  
    ```
    UPDATE [] TABLE_REFERENCE
    	SET COL_NAME={exper1|default}...
    	[WHERE CONDITIONS]
    	[ORDER BY]
    	[LIMIT ROW_COUNT]
    ```
  
    ```
  mysql> update info set id=6 where name = 'hui';
    ```
    
    注意要有限制条件，否则将修改所有行的指定字段，可以用mysql选项避免此错误：
    
    ```
    mysql -U | --safe-updates | --i-am-a-dummy 
    可以把 mysql -U 做成别名，
    alias mysql="mysql -U"
    设置了 -U 选项后使用删除的时候必须跟上一个主键的条件！
    ```
  
* DQL：数据查询语言 Data Query Language

  * SELECT
    

select语句处理的顺序

SQL解析-->FROM-->ON-->JOIN和WHERE并列地位-->GROUP BY-->HAVING-->SELECT-->ORDER BY-->LIMIT

执行查询路径中的组件：

![dfff6efbab0d51a715a36f867daeacf8](C:\Users\a\Desktop\dfff6efbab0d51a715a36f867daeacf8.png)

    * 单表操作

  between ... and

  distinct 

  模糊查询:like %,_ 

  ​	数据库、数据表是区分大小写的，因为他们都是文件（目录）

  ​	字段名、数据是不区分大小写的

  GROUP分组，涉及到前后次序

  ​	groupby xxx having conditions=xxx

  ​	where conditions = xxx groupby xxx

  正则表达式查询：rlike,regexp

  order by asc desc

  字段的别名

  limit N 或者可以跳过前几条后开始显示

  对查询结果加锁

    * 多表查询
    
      子查询
    
      ​	普通子查询
    
      ​	IN 子查询
    
      ​	exists 和 not exists
    
      exists（包括 not exists）子句的返回值是一个BOOL值。exists内部有一个子查询语句，将其称为exists的内查询语句。其内查询语句返回一个结果集，exists子句根据其内查询的结果集空或非空，返回一个布尔值。将外查询的每一行带入内查询作为检验，如果内查询返回的结果为非空值，则exists子句返回true，外查询的这一行数据便可作为外查询的结果返回，否则不能作为结果。
    
      联合查询 union，union all
    
      交叉连接：笛卡尔乘积，两个表完全组合一遍 cross join
    
      内连接 inner join table_name on 涉及到别名
    
      外连接 涉及到别名
    
      ​	左外连接 left outer join table_name on 
    
      ​	有外连接 right outer join table_name on
    
      ​	只取非交集 左连接或右连接再加一个key的where条件
    
      ​	全外连接 full outer join table_name on 
    
      ​	mysql不支持 full outer写法，但是可以通过组合左右连接来union实现
      
      自连接

* DCL：数据控制语言 Data Control Language
  * GRANT
  * REVOKE
  * COMMIT
  * ROLLBACK

软件开发：CRUD 增删改查

```
SELECT *
FROM table1
WHERE price>666;
```

字符集：UTF8MB4 4个字节，生产环境记得修改字符集

* SHOW COLLATION  字符集的排序规则
* SHOW VARIABLES LIKE 'collation%'
* SHOW CHARACTER SET
* SHOW variables like 'character%' 查看当前数据库中使用的字符集

数据类型：

数据长什么样

数据需要多少空间来存放



系统内置数据类型

用户定义数据类型

选择正确的数据类型对于获得高性能至关重要，三大原则：

1.更小的通常更好，尽量使用可正确存储数据的最小数据类型

2.简单就好，简单数据类型的操作通常需要更少的CPU周期

3.尽量避免NULL，包含为NULL的列，对MySQL更难优化

* 整数型

  * tinyint 1个字节
  * int 4个字节
* 浮点型
* 定数型
* 字符串
  * char(n) 固定长度，最多255个字符，注意不是字节
  * varchar(n) 可变长度，最多65535个字符
* 修饰符
  * NULL	数据列可包含NULL值，默认值
  * NOT NULL 数据列不允许包含NULL值，*为必填选项
  * DEFAULT 默认值
  * ENUM（'',''）限制值只能是设定的
  * PRIMARY KEY 主键，所有记录中此字段的值不能重复，且不能为NULL
  * UNIQUE KEY  唯一键，所有记录中此字段的值不能重复，但可以为NULL
  * CHARACTER SET name 指定一个字符集
  * 适合数值型的修饰符
    * AUTO_INCREMENT 自动递增，适用于整数类型 最大是2^32次方-1 4294967295
    * UNSIGNED 无符号
  * 日期时间类型
    * data 日期 '2020-09-22'
    * time 时间 '20:20:20'
    * datatime 日期时间 '2020-09-22 20:20:30'
    * timestamp 自动存储记录修改时间

视图 VIEW

视图是虚拟的表，类似表的别名，视图存在于数据库目录下，是以文件形式存放的

视图可以隐藏数据表的结构

视图可以修改原始表的数据，视图中的数据实际存储在基表中，因此对视图数据的修改也会对基表修改，视图的修改操作受基表限制

创建视图

```
CREATE VIEW view_name [(column_list)]
	as select_statement
	[with[cascaded|local] check option]
```

查看视图定义：

```
#显示创建视图和表的过程
show create table v_name;
或
show create view v_name;
```

删除视图

```
DROP VIEW [if exists]
	view_name[,view_name] ...
	[restrict|cascade]
```

函数 FUNCTION

​	函数必须有()

​	内置函数和自定义函数

​	mysql数据库的proc表中

​	函数只能被调用select function_name()，不能独立使用;

创建函数

```
CREATE [AGGREAGTE] FUNCTION function_name(parameter_name type,....)
	RETURN {STRING|INTEGER|REAL}
	runtime_body
```

说明：

* 参数可以有多个，也可以没有
* 无论有没有参数，必须有()
* 必须有且只有一个返回值

查看函数列表

```
SHOW FUNCTION STATUS;
```

查看函数定义

```
SHOW CREATE FUNCTION function_name
```

删除自定义函数

```
DROP FUNCTION function_name
```

调用函数

```
select function_name();
```

Mysql中的变量

​	系统变量 @@var_name 引用

​	普通变量 @var_name 适用于当前会话

​	局部变量 var_name 函数体内部有效，其他地方无效

存储过程 PROCEDURE

​	存储过程：多表SQL的语句的集合，可以独立执行，存储过程保存在mysql.proc表中

​	call procdure_name;

创建存储过程

```
CREATE PROCEDURE procedure_name ([parameter]...)
routime_body
proc_parameter :[IN|OUT|INOUT] parameter_name type
```

查看存储过程列表

```
SHOW PROCEDURE STATUS
```

查看存储过程定义

```
SHOW CREATE PROCEDURE procedure_name
```

调用存储过程

```
call procedure_name [(parameter)]
#说明：当没有参数时可以不写 ()
```

存储过程通过ALTER只能修改注释等无关紧要的东西，不能修改存储过程体，所以要修改存储过程，方法就是删除重建

删除存储过程

```
DROP PROCEDURE [if exists] procedure_name
```

触发器 TRIGGER

​	触发器的执行不是由程序调用，也不是由手动启动，而是由时间来触发、激活从而实现执行

​	触发器只对insert,delete,update这三个动作

```
CREATE [ DEFINER = {USER|CURRENT_USER} ]
	TRIGGER trigger_name
	trigger_time {before|after} trigger_event
	on table_name for each row
	trigger_body
```

触发器文件在/var/lib/mysql/dbname/trigger_name.TRN

查看触发器

```
#通过show命令查看
SHOW TRIGGERs ;
#通过查看information_schema.triggers表
mysql> select * from information_schema.triggers where trigger_name = 'student_count_delete';
```

删除触发器

```
DROP TRIGGER trigger_name
```

事件 EVENT

​	存放在mysql.event表中

​	秒钟级别

​	环境变量：@@event_scheduler

查看事件管理器是否开启，默认是关闭0,1是开启

```
mysql> select @@event_scheduler;
```

事件管理器开启后会开启一个线程

```
SHWO PROCESSLIST
```

可以通过在配置文件中添加 event_scheduler = on  来永久开启





### 用户账户管理

mysql.user表中存放用户信息

用户账户：

```
'USERNAME'@'HOST'
USERNAME :操作系统的用户名
@HOST：主机名，IP地址或者Network
	通配符：% 表示所有  _ 表示任意一个字符
	示例：172.16.%.% user@192.168.1.%
```



#### 账号创建 CREATE USER

```
CREATE USER 'USERNAME'@'HOSTNAME' [IDENTIFIED BY '密码']
新建用户默认权限：USEAGE
mysql> create user 'lv'@'10.0.0.%' identified by 'centos';
```

用户重命名：

```
RENAME USER old_user_name TO new_user_name;
```



#### 账号删除

```
DROP USER 'user_name'@'hostname'
```

#### 账号密码修改

注意：

* 新版MYSQL中用户密码可以保存在mysql.user表的authentication_string字段
* 旧版的密码在mysql.password字段，如果两个字段都存在数据，那么authentication_string优先级更高

```
set password for root@'localhost'  password('centos') 
```

官方推荐这样操作

```
alter user user_name'@'hostname identified by 'password' ;
```

忘记密码的操作

1.关闭验证环节

```
--skip-grant-tables
--skip-networking
```

2.登录mysql数据库，修改密码

#### 账号授权

权限的类型

* 管理类
  * 用户管理等
* 程序类
  * 函数、存储过程等的控制
* 数据库级别
  * 数据库的增改删
* 表级别
  * 数据表的增改删
* 字段级别
  * 规定哪些字段可以操作，比如select ((col1),(col2)...)

操作符：GRANT,REVOKE

所有权限：ALL PRIVILEGES 或 ALL

授权：GRANT

```
GRANT priv_type [(column_list)]，...ON [object_type] priv_level TO 'user'@'host'
[IDENTIFIED BY 'PASSWORD'] [WITH GRANT OPTION];

priv_type：all [privileges]
object_type：table | function | procedure
priv_level：*(所有库) | *.* | db_name.* | db_name.table_name | table_name(当前库的表) | db_name.routine_name(指定库的函数，存储过程，触发器)
with_option：grant option
	| max_queries_per_hour count
	| max_updates_per_hour count
	| max_connections_per_hour count
	| max_user_connections count
```

例如：

```
GRANT  ALL PRIVILEGES ON *.* TO 'USER_NAME'@'HOSTNAME' IDENTIFIED BY 'PASSWORD';
```

```
mysql> grant all on hellodb.* to 'lv'@'10.0.0.%' ;
#把hellodb下的所有权限发给lv@10.0.0.%用户
```

```
mysql> grant select on mysql.* to 'lv'@'10.0.0.%';
```

取消授权：REVOKE

```
REVOKE priv_type [(column_list)]... on [object_type] priv_level from user..
```

```
mysql> revoke select on mysql.* from 'lv'@'10.0.0.%' ;
```

查看用户权限：

```
SHOW GRANTS FOR XXX@XXX;
```

例如

```
mysql> show grants for 'lv'@'10.0.0.%';
+--------------------------------------------------------+
| Grants for lv@10.0.0.%                                 |
+--------------------------------------------------------+
| GRANT USAGE ON *.* TO 'lv'@'10.0.0.%'                  |
| GRANT ALL PRIVILEGES ON `hellodb`.* TO 'lv'@'10.0.0.%' |
| GRANT SELECT ON `mysql`.* TO 'lv'@'10.0.0.%'           |
+--------------------------------------------------------+
3 rows in set (0.00 sec)
```
注意：数据库启动的时候会自动读取mysql库中的全部权限表到内存，对于不能及时重读授权表的命令，可以手动执行

```
mysql> flush privileges;
```




## MySQL架构和性能优化

![image-20200925200135006](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200925200135006.png)

Mysql是单进程多线程的

连接器：一个API，支持各种语言开发的连接工具

连接池：认证，线程控制，内存检查

SQL接口：解释器，进行语法检查

解析器：查询翻译、权限检查

寻路优化：访问路径统计

缓存和缓冲区：这个不是上面的缓存访问，而是跟内存和磁盘打交道的缓存

插件存储引擎：面试常问，工作中对于引擎的选择是比较固定的

文件系统：

文件和日志：

### 存储引擎

mysql是插件式存储引擎，它能够替换使用选择多种不同的引擎

存储引擎负责把具体分析结果完成对磁盘上文件路径访问的转换，数据库中的行数据是存储在磁盘块上的，因此存储引擎要把数据库数据映射为磁盘块，并把磁盘块加载至内存中；（逻辑关系转化成物理数据文件）

```
show engines
show variables like '%engine%'
--default_storage_engines,default_storage_engines
storage_engine
show table status from table_name
show create table table_name
create TABLE table_name () ENGINE=INNODB
ALTER TABLE table_name ENGINE=INNODB
```

**MyISAM和InnoDB的区别**

| 属性       | MyISAM | InnoDB |
| ---------- | ------ | ------ |
| 锁的颗粒度 | 表级锁 | 行级锁 |
| 集群索引   | 不支持 | 支持   |
| MVCC       | 不支持 | 支持   |
| 外键       | 不支持 | 支持   |
| 存储空间   | 256TB  | 64TB   |
| 事务       | 不支持 | 支持   |
| 速度       | 相对快 | 相对慢 |
> InnoDB：Supports transactions, row-level locking, foreign keys and encryption for tables
>
> MyISAM：Non-transactional engine with good performance and small data footprint

MVCC：多版本并发控制机制 

​	数据的时间戳，INSERT和DELETE

InnoDB的数据文件是两个，一个数据定义，一个存放数据  

> name.frm  数据定义
>
> name.ibd   真实数据以及索引

MyISAM数据文件三个，一个索引，一个数据，一个定义

行-->页-->extent-->segment

数据库的读取是以一页作为基本单位，一个页16KB，对比文件系统的块，文件系统的块Block是4KB

存储引擎是对表来说的，是对表的管理，同一个数据库中的表可以用不同的存储引擎

### MySQL中的系统数据库

* MySQL数据库

  是MYsql的核心数据库，主要负责存储数据库的用户、权限设置、关键字等Mysql自己需要使用的控制和管理信息

* performance_schema

  MYsql 5.5 新增的数据库，主要用户手机数据库服务器性能参数，库里表的存储引擎均为PERFORMANCE_SCHEMA，用户不能创建存储引擎为PREFORMANCE_SHCEMA的表

* information_schema

  一个虚拟数据库，物理上并不存在information_schema数据库类似于"数据字典"，提供了访问数据库元数据的方式，即数据的数据。比如数据库名或表明，列类型，访问权限等

* sys

  MYSQL5.7之后新增加的数据库，库中的所有数据来源于performance_schma，目标是把performance_schema的复杂程度降低，让DBA能更好的阅读这个库里的内容。

### 服务器配置和状态

通过mysqld的选项、服务器系统变量和服务器状态变量进行MySQL的配置和查看状态

官方文档：https://dev.mysql.com/doc/refman/5.7/en/server-option-variable-reference.html

https://mariadb.com/kb/en/library/full-list-of-mariadb-options-system-and-status-variables/

#### 服务器选项

写入配置文件，持久保存

Default options are read from the following files in the given order:

```
/etc/my.cnf /etc/mysql/my.cnf ~/.my.cnf
```



```
#打印出当前服务启动时的默认选项
[root@localhost local]#mysqld --print-defaults
mysqld would have been started with the following arguments:
--datadir=/data/mysql --socket=/data/mysql/mysql.sock --skip_name_resolve=on --port=3306
#打印出可用选项
[root@localhost local]#mysql --verbose --help
```



#### 服务器系统变量

变量临时修改，无法持久保存

系统变量分为全局（GLOBAL）和会话（session）两种

```
show global variables;#查看全局变量
show [session] variables;#查看所有变量
select @@variable;#查看某个变量的值
SET GLOBAL system_var_name=value;
SET @@GLOBAL.SYSTEM_VAR_NAME=value;
SET system_var_name=value;
SET @@system_var_name=value;
```

max_connection 最大连接数，一个数据库1000左右的连接数

```
--character_set_server,character_set_server
--max_connections,max_connections
#如果该最大连接数不成功，应该是ulimits限制的，可以在/etc/my.cnf中加一句
LimitNOFILE=65535
```





#### 服务器状态变量

```
show status like 'variables';
show global status;
show status;
```



#### 服务器变量SQL_MODE

SQL_MODE：对其设置可以完成一些约束检查的工作，可分别进行全局的设置或当前会话的设置

文档：https://mariadb.com/kb/en/library/sql-mode/

https://dev.mysql.com/doc/refman/5.7/en/server-options.html#option_mysqld_sql-mode

```
--sql_mode,sql_mode
select @@sql_mode;
```

常见MODE：

* NO_AUTO_CREATE_USER：禁止GRANT创建密码为空的用户
* NO_ZERO_DATE：在严格模式，不允许使用"0000000"的时间
* ONLY_FULL_GROUP_BY：对于GROUP BY聚合操作，如果在SELECT中的列，没有在GROUP BY中出现，那么将认为这个SQL是不合法的
* NO_BACKSLASH_ESCAPES：反斜杠“\”被视为普通字符而不是逃逸符
* PIPES_AS_CONCAT：将“||”视为连接操作符而非“或”运算符

### Query Cache 查询缓存

Redis代替

能用别的应用解决，就不要用数据库实现

查询缓存的启用和使用，hash算法，因此对语句的要求很高，必须一模一样，连空格什么的都得一样才能从缓存中读取

查询缓存优化



### 索引 INDEX

#### 索引介绍

索引是排序的快速查找的特殊数据结构，定义作为查找条件的字段上，又称为键key，索引通过存储引擎实现

适合读操作；不适合频繁更改的数据表

优点

* 索引可以降低服务器需要扫描的数据量，减少了IO次数
* 索引可以帮助服务器避免排序和使用临时表
* 索引可以帮助将随机I/O转为顺序I/O

缺点

* 占用额外空间，影响插入速度

索引类型：

* B+ TREE,HASH,R TREE,FULL TEXT
* 聚簇索引，非聚簇索引：数据和索引是否存储在一起
* 主键索引、二级索引
* 稠密索引、稀疏索引：是否引用了每一个数据项
* 简单索引、组合索引
* 左前缀索引：取前面的字符做索引
* 覆盖索引：从索引中即可取出要查询的数据，性能高

##### 索引结构

参考链接 : https://www.cs.usfca.edu/~galles/visualization/Algorithms.html

* 二叉树

* 红黑树

* B树

* Hash索引

  基于哈希表实现，只有精确匹配索引中的所有列的查询才有效，索引自身只存储索引列对应哈希值和数据指针，索引结构紧凑，查询性能好；Memory存储引擎支持显式HASH索引，适用于：= <=>

  ```
  <=> 符号是专门比较NULL值的，表示等于NULL，也就是 IS NULL
  ```

  

* B+树，INNODB就是用B+树作为索引

B+树索引的特点：

1.查询类型

* 全值匹配：精确所有索引列，如：姓lv,名zhehui,年龄25
* 匹配最左侧前缀：即只使用索引的第一列，如：姓lv
* 匹配列前缀：只匹配一列值的开头部分，如：姓以l开头的记录
* 匹配范围值：如：姓lv和wang之间
* 精确匹配某一列范围并范围匹配另一列：如：姓lv，名以z开头的记录
* 只访问索引的查询
* 如果不从最左列开始，则无法使用索引，如：查询名为zhehui,或姓的结尾是g
* 不能跳过索引中的列：？如：查找姓wang，年龄30，只能使用索引第一列

mysql中 a% 可以利用索引模糊查询 %a就不可以利用索引了

B+树具有查询次数少，可查询数据数量大的特点

```
B+树的数据存放量计算：
1.B+树的高度
2.根节点的指针数
3.单个叶子记录的行数
1*2*3 得到的就是总的行数据
例子：
1.B+树高2层
2.根节点的指针数：大小16KB，一个指针6B，主键ID的数据类型是bigint，占8B，那么指针数就是：16KB/14B=1170个指针
3.单个叶子记录的行数:大小16KB，一个记录1KB，那么就是16条记录
所以总的记录数是：16*1170=18720
当B+树是3层的时候就是 16*1170*1170=21902400，两千万级别的数据量
```



#### 索引优化

* 独立地使用列：尽量避免其参与运算，独立的列指索引列不能是表达式的一部分，也不能是函数的参数，在where条件中，始终将索引列单独放在比较符号的一侧，尽量不要在列上进行运算
* 左前缀索引：构建指定索引字段的左侧的字符数，要通过索引选择性（不重复的索引值和数据表的记录总数的比值）来评估，尽量使用短索引，如果可以，应该制定一个前缀长度
* 多列索引：AND操作时适合使用多列索引，而非为每个列创建单独的索引
* 选择合适的索引列顺序：无排序和分组时，将选择性最高的列放左侧
* 只要列中含有NULL值，就最好不要在此列设置索引，复合索引如果有NULL值，此列在使用时也不会使用索引
* 对于经常在where子句使用的列，最好设置索引
* 对于有多个列where或者order by子句，应该建立复合索引
* 对于like子句，以%或_开头的列不会使用索引，以%结尾会使用索引
* 尽量不要使用not in 和 <> 操作，虽然可以使用索引，但是性能不高
* 不要使用RLIKE正则表达式，这会导致索引失效
* 查询时，能不要使用“*”就不要使用，尽量写字段名，比如：select id,name,age from students;
* 大部分情况连接效率远大于子查询
* 在有大量记录的表分页时使用limit
* 对于经常使用的查询，可以开启查询缓存
* 多使用explain和profile分析查询语句
* 查看慢查询日志，找出执行时间长的SQL语句优化

#### 管理索引

* 创建索引

```
CREATE INDEX INDEX_NAME ON table_name(col(length));

ALTER TABLE table_name ADD  INDEX index_name(index_col_name[length]);
```

* 查看索引

```
SHOW INDEXES FROM table_name;

set global userstat=1;#开启统计
show index_statistics;#统计有多少查询利用了索引
```

* 优化表空间

```
OPTIMIZE TABLE table_name;
```



* 删除索引

```
DROP INDEXES INDEX_NAME ON table_name;
```

ExPLAIN工具

参考资料：https://dev.mysql.com/doc/refman/5.7/en/explain-output.html

```
explain select_statment
例子：
MariaDB [hellodb]> explain select stuid,name,age,classid from students where stuid=10;
+------+-------------+----------+-------+---------------+---------+---------+-------+------+-------+
| id   | select_type | table    | type  | possible_keys | key     | key_len | ref   | rows | Extra |
+------+-------------+----------+-------+---------------+---------+---------+-------+------+-------+
|    1 | SIMPLE      | students | const | PRIMARY       | PRIMARY | 4       | const | 1    |       |
+------+-------------+----------+-------+---------------+---------+---------+-------+------+-------+
1 row in set (0.000 sec)
```

explain输出信息说明：

| 列名          | 说明                                                      |
| ------------- | --------------------------------------------------------- |
| id            | 执行编号，标识select所属的行                              |
| select_type   | SIMPLE\|PRIMARY                                           |
| table         | 访问引用哪个表                                            |
| type          | 关联类型或访问类型，即MYSQL决定的如何去查询表中的行的方式 |
| possible_keys | 查询可能会用到的索引                                      |
| key           | 显示mysql决定采用哪个索引来优化查询                       |
| key_len       | 显示mysql在索引里使用的字节数                             |
| ref           | 根据索引返回表中匹配某单个值的所有行                      |
| rows          | 为了找到所需的行而需要读取的行数，估算值，不精确          |
| extra         | 额外信息                                                  |



type类型：type类型是访问类型，结果值从好到坏依次是：

system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > all，一般来说，得保证查询至少到达range级别，最好到ref

| 类型   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| ALL    | 最坏的情况，全表扫描                                         |
| index  | 和全表一样。                                                 |
| range  | 范围扫描，一个有限制的索引扫描                               |
| ref    | 一种索引访问，它返回所有匹配某单个值的行                     |
| eq_ref | 最多只返回一条符合条件的记录                                 |
| const  | 当确定最多只会有一行匹配的时候，mysql优化器会在查询前读取它而且只读取一次，因此非常快 |
| system | 这个const连接类型的一种特例，表仅有一行满足条件              |
| null   | 意味着mysql能在优化阶段分解查询语句，在执行阶段甚至用不到访问表或索引 |



### 并发控制

#### 锁机制

锁：

* 读锁，共享锁，也称S锁，只读不可写（包括当前事务），多个读互不阻塞
* 写锁：独占锁，排他锁，也称为X锁，写锁会阻塞其他事务的读和写

读锁和读锁是兼容的，写锁和其他锁都不兼容

```
例如：
  S锁和S锁是兼容的，X锁和其他锁都不兼容，举个例子，事务T1获取了一个行r1的S锁，另一个事务T2还可以获得行r1的S锁，此时T1和T2共同获得r1的S锁，此种情况称为锁兼容；但是当另外一个事务T3此时如果想获得行r1的X锁，则必须等到r1的全部锁释放之后才可以，此种情况称为锁冲突
```

锁的颗粒度

* 表级锁：MyISAM
* 行级锁：InnoDB

实现：

* 存储引擎
* 服务器级

分类：

* 隐式锁：由搜索引擎自动加锁
* 显式锁：用户手动请求

锁策略：在锁颗粒度及数据安全性寻求的平衡机制

#### 显式使用锁

帮助:https://mariadb.com/kb/en/lock-tables/

加锁：

```
lock tables table_name lock_type;
lock_type:READ和WRITE
```

解锁：

```
unlock  tables;
```

关闭正在打开的表（清除查询缓存），通常在备份前加全局读锁

```
FLUSH TABLES [table_name] [with read lock]
```



#### 事务

事务：Transactions 一组原子性的SQL语句，或一个独立的工作单元

事务日志：记录事务信息，实现undo,redo等故障恢复功能

##### 事务特性

**ACID特性：**

A：automicity 原子性；整个事务中的所有操作要么全部成功执行，要么全部失败后回滚

C：consistency 一致性；数据库总是从一个一致性状态转换成另一个一致性状态

I：isolation 隔离性；一个事务所做出的的操作在提交之前，是不能为其他事务所见；隔离有多种隔离级别，实现并发

D：durability 持久性；一旦事务提交，其所做的修改会永久保存于数据库中

Transcation 生命周期

![image-20200926192208219](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200926192208219.png)

##### 管理事务

```
--autocommit,autocommit
```

显示启动事务：

```
BEGIN
BEGIN WORK
START TRANSACTION
```

结束事务：

```
#提交
COMMIT
#回滚
ROLLBACK
```

注意：1.只有事务性存储引擎支持（存储引擎支持事务）

​            2.只有DML语句可以,DDL语句无法回滚!(CREATE,DROP,TRUNCATE,ALTER)

自动提交：

```
set autocommit={1|0}
默认为1，为0时设为非自动提交
建议：显示请求和提交事务，而不要使用“自动提交”功能
```

事务支持保存点：

```
SAVEPOINT identifier
ROLLBACK [WORK] TO [SAVEPOINT] identifier
RELEASE SAVEPOINT identifier
```

查看事务：

```
#查看当前正在进行的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;
#查看当前锁定的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;
#查看当前等锁的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;
#查看当前进程，如果有事务被锁可以通过进程号杀死
show processlist;
#杀死
kill id;
```



##### 事务隔离级别

MYsql支持四种隔离级别

| 隔离级别 | 脏读       | 不可重复读 | 幻读       | 加读锁 |
| -------- | ---------- | ---------- | ---------- | ------ |
| 读未提交 | 可以出现   | 可以出现   | 可以出现   | 否     |
| 读提交   | 不允许出现 | 可以出现   | 可以出现   | 否     |
| 可重复读 | 不允许出现 | 不允许出现 | 可以出现   | 否     |
| 序列化   | 不允许出现 | 不允许出现 | 不允许出现 | 是     |

读未提交：READ UNCOMMITTED

​	可读取到未提交数据，产生脏读

读提交：READ COMMITTED

​	可读取到提交数据，但未提交数据不可读，产生不可重复读，即可读取到多个提交数据，导致每次读取数据不一致

可重复读：REPETABLE READ

​	可重复读，多次读取数据都一致，产生幻读，即读取过程中，即使有其他提交的事务修改数据，仍只能读取到未修改前的旧数据。此为mysql默认设置

序列化：SERIALIZABLE

​	可串行化，未提交的读事务阻塞修改事务（加读锁，但不阻塞读事务），或者未提交的修改事务阻塞读事务（加写锁，其他事务的读，写都不可以执行）。会导致并发性能差

MVCC和事务的隔离级别：

MVCC（多版本并发控制机制）只在REPETABLE READ 和 READ COMMITED 两个隔离级别下工作。其他两个隔离级别都和MVCC不兼容，因为READ UNCOMMITED总是读取最新的数据行，而不是符合当前事务版本的数据行。而SERIALIZABLE则会对所有读取的行都加锁

```
MVCC：Multi-Version Concurrency Control
```

插入时间戳，根据时间戳来管理要查询到的数据；MVCC只对读有效果，对写无效；

幻读：幻读产生的原因是事务的写操作；T1事务第一次select出来的内容是A，然后T2事务执行了一个插入数据的操作并提交了，本来这样的操作在可重复读的隔离级别中T1是看不到数据改变的，但是T1接下来执行了一个UPDATE操作，update操作修改的数据恰好匹配到了T2刚刚提交的数据，那么这个数据就会被T1修改，接下来T1执行select就会看到T2提交的数据，当然已经被update修改过了。这就是幻读，第一次select和第二次select看到的内容不一致。

指定事务隔离级别：

变量：tx_isolation

服务器选项：transaction-isolation

* 服务器变量tx_isolation指定，默认为REPETABLE READ，可在GLOBAL和SESSION级进行设置

```
SET tx_isolation='READ-UNCOMMITED|READ-COMMITED|REPETABLE-READ|SERIALIZABLE'
```

* 服务器选项中指定

```
vim /etc/my.cnf
[myslqd]
transaction-isolation=SERIALIZABLE
```

死锁：

两个或多个事务在同一资源互相占用，并请求锁定对方占用的资源的状态

当要出现死锁时，数据库会自动发现并解决，解决方式是停止执行时间短的那个事务

### 日志管理

Mysql支持丰富的日志类型，如下：

* 事务日志：transaction log

> 事务日志的写入类型为“追加”，因此其操作为“顺序IO”；通常也被称为：预写式日志 write ahead logging

> 事务日志文件：ib_logfile0,ib_logfile1

* 错误日志 error log
* 通用日志 general log
* 慢查询日志 slow query log
* 二进制日志 binary log
* 中继日志 reley log，在主从复制架构中，从服务器用于保存从主服务器的二进制日志中读取的事件

#### 事务日志

事务日志：transaction log

* redo log：实现WAL（Write Ahead Log），数据更新前先记录redo log  具体名字是：ib_logfile 默认最多50M，默认两个文件，写满就重新写，建议把这个存储值的大小调整大一些，文件个数调整成3个，存放的位置建议是一个干净的只放日志的分区，这样速度更快
* undo log：保存与执行的操作相反的操作，用于实现rollback  具体的文件是在每个数据表的name.ibd中

事务型存储引擎自行管理和使用，建议和数据文件分开存放

Innodb事务日志相关配置：

```
MariaDB [hellodb]> show variables like '%innodb_log%';
+-----------------------------+-----------+
| Variable_name               | Value     |
+-----------------------------+-----------+
| innodb_log_buffer_size      | 16777216  |
| innodb_log_checksums        | ON        |
| innodb_log_compressed_pages | ON        |
| innodb_log_file_size        | 100663296 |
| innodb_log_files_in_group   | 1         |
| innodb_log_group_home_dir   | ./        |
| innodb_log_optimize_ddl     | OFF       |
| innodb_log_write_ahead_size | 8192      |
+-----------------------------+-----------+
8 rows in set (0.000 sec)

innodb_log_file_size #每个日志文件大小
innodb_log_files_in_group #日志组成员个数
innodb_log_group_home_dir #事务文件路径
innodb_flush_log_at_trx_commit #默认为1
```

事务日志性能优化

![image-20201009104011312](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20201009104011312.png)

```
innodb_flush_log_at_trx_commit=0|1|2
1 此为默认值，日志缓冲区将写入日志文件，并在每次事务后执行刷新到磁盘。完全遵循ACID特性，但是磁盘I/O会变多，对数据来说最安全
0 提交时没有写磁盘的操作；而是每秒执行一次将日志缓冲区的提交的事务写入刷新到磁盘。这样可提供更好的性能，但数据库服务崩溃可能丢失最后一秒的事务，速度最快，I/O
2 每次提交后都会写入OS的缓冲区，但每秒才会进行一次刷新到磁盘文件的操作。性能比0略差一些，但操作系统或停电可能导致最后一秒的数据丢失。
1模式最慢但最安全；
0模式最快但不安全；
2模式中等速度中等安全
推荐2模式
```



#### 错误日志

mysql启动和关闭过程中输出的事件信息

记录mysql运行过程中产生的错误信息

event_scheduler运行一个event时产生的日志信息

在主从复制架构中的从服务器上启动从服务器线程时产生的信息

错误文件路径

```
SHOW GLOABL VARIABLES LIKE 'log_error';
+---------------+------------------------+
| Variable_name | Value                  |
+---------------+------------------------+
| log_error     | /data/mysql/mysqld.log |
+---------------+------------------------+
1 row in set (0.001 sec)
```

记录哪些警告信息至错误日志文件

```
log_warnings=0|1|2|3...
MariaDB [(none)]> show variables like 'log_warnings';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| log_warnings  | 2     |
+---------------+-------+
1 row in set (0.000 sec)
```

#### 通用日志

通用日志：记录对数据库的通用操作，包括错误的SQL语句

通用日志可以保存在file(默认值)或table(mysql.general_log)

通用日志相关设置

```
general_log=ON|OFF
general_log_file=HOSTNAME.log
log_output=TABLE|FILE|NONE
```

范例：查找执行次数最多的前三个

```
select argument,count(argument) from general_log group by argument order by count(argument) desc limit 3
```

范例：对访问的语句进行排序

```
[root@localhost mysql]#mysql -uroot -p'centos' -e "select argument from mysql.general_log" | awk '{sql[$0]++}END{for (i in sql) {print sql[i],i}}'|sort -nr
```



#### 慢查询日志

慢查询日志：记录执行查询时长超出指定时长的操作

慢查询相关变量：

```
slow_query_log=ON|OFF #开启或关闭慢查询，支持全局和会话，只有全局设置才会生成慢查询
long_query_time=N #慢查询的阈值，单位秒，默认为10s
slow_query_log_file=HOSTNAME-slow.log #慢查询日志
log_slow_filter=admin,filesort,filesort_on_disk,full_join,full_scan,query_cache,query_cache_miss,tmp_table,tmp_table_on_disk
#只有是上述查询类型且查询时长超过long_query_time阈值，才记录日志
log_queries_not_using_indexes=ON #不使用索引或使用全索引扫描的情况下，不论是否达到慢查询阈值。是不是要记录，默认是不记录的
log_slow_rate_limit=1 #多少次查询才记录，mariadb特有
log_slow_verbosity=query_plan,explain #记录内容
log_slow_queries=off #同slow_query_log
```

慢查询分析工具

```
#mysqldumpslow -s c -t 10 /data/mysql/slow.log
```



#### 使用profile工具

```
#开启profile工具
set profiling = on
#查看查询语句的执行时间
show profiles;
#显示语句的详细执行步骤和时长
show profile for query N;  # N 是通过show profiles 看到的ID
#显示CPU使用情况
show profile cpu for query N # N 是 看到的ID
```



#### 二进制日志（备份）

##### 二进制日志的作用：

* 记录导致数据改变或潜在导致数据改变的SQL语句
* 记录已提交的日志
* 不依赖于存储引擎类型

通过“重放”二进制日志文件中的事件来生成数据副本

建议二进制文件和数据文件分开存放

##### 二进制日志记录的三种模式

* 基于“语句”记录：statement，记录语句，默认模式（mariadb 10.2.3以下），日志量较少
* 基于“行”记录：row，记录数据，日志量较大，更加安全，建议使用的格式
* 混合模式：mixed，让系统自行判断该基于哪种方式进行，默认模式（mariadb 10.2.4以上）

##### 格式配置

```
#mariadb 10.5.5 默认是mixed
MariaDB [hellodb]> show variables like 'binlog_format';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | MIXED |
+---------------+-------+
1 row in set (0.001 sec)

#MYsql 8.0 中默认是row
mysql> show variables like 'binlog_format';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | ROW   |
+---------------+-------+
1 row in set (0.06 sec)

```

##### 二进制文件的构成

```
有两类文件
1.日志文件，mysql|mariadb-bin.文件名后缀，二进制格式，如：mariabd-bin.00001
2.索引文件：mysql|mariadb-bin.index，文本格式
```

二进制日志相关的服务器变量：

```
sql_log_bin=ON|OFF #是否记录二进制文件，默认on，支持动态修改，系统变量，不是服务器参数

log_bin=/path/bin_log_file #指定文件位置，默认为OFF
将需要存储二进制日志的目录加上权限：chown -R mysql:mysql /data

binlog_format=statement|row|mixed #二进制日志记录的格式，默认为
statement

max_binlog_size=1073741824 #单个二进制文件的最大体积，到达最大值会自动滚动，默认为1G。但是文件达到上限时的文件大小未必为指定的精确值

binlog_cache_size=4M #此变量确定在每次事务中保存二进制日志更改记录的缓存的大小

max_binlog_cache_size=512M #限制用于缓存多事务查询的字节的大小

sync_binlog=1|0 #设定是否启动二进制日志即时同步磁盘功能，默认为0，由操作系统负责同步日志到磁盘

expire_logs_days=N #二进制日志可以自动删除的天数，默认为0，即不自动删除
```

##### 二进制日志相关的配置

查看数据库自行管理使用中的二进制日志文件列表和大小

```
show binary logs;
```

查看使用中的二进制日志文件

```
show master status
```

查看二进制文件中的指定内容

```
show binlog events [in 'log_name'] [from pos][limit [offset,] row_count]
```

范例：

```
show binlog events in 'bin.xxxxx' from number1 limit xxx,xxx
```

生成新的二进制文件的方式：

1.重启数据库服务

2.单个文件达到最大值，max_binlog_size

3.flush logs;

##### myslqbinlog

mysqlbinlog:二进制日志的客户端命令工具，支持离线查看二进制日志

命令格式：

```
mysqlbinlog [options] logfile...
	--start-position= #开始位置
	--stop-position=
	--start-datatime= #开始时间，时间格式是：YYYY-MM-DD hh:mm:ss
	--stop-datetime=
	--base64-output[=name]
	-v -vvv
```

范例：

```
[root@centos8 mysql]#mysqlbinlog --start-position=678 --stop-position=752 binlog.000001 -v
```

二进制日志事件的格式：

```
# at 328
#151105 16:31:30 server id 1 end_log_pos 431 query thread_id=1
exec_time=0 error_code=0
use `mydb`/*!*/;
set timestamp=1446712300/*!*/;
create table tb1 (id int,name char(30))
/*!*/;
事件发生的日期和时间：151105 16:31:30
事件发生的服务器标识：server id 1
事件的结束位置：end_log_pos 431
事件的类型：query
事件发生时所在服务器执行此时间的线程的ID：thread_id=1
语句的时间戳与将其写入二进制文件中的时间差：exec_time=0
错误代码：error_code=0
事件内容：
use `mydb`/*!*/;
set timestamp=1446712300/*!*/;
create table tb1 (id int,name char(30))
/*!*/;
```

清除指定二进制日志：

```
PURGE {BINARY|MASTER} LOGS {TO 'log_name' |BEFROE datetime_expr}
```

范例：

```
PURGE BINARY LOGS TO 'bin.xxxx';
PURGE BINARY LOGS BEFROE '2018-01-01'
```

删除所有二进制日志，index文件重新计数

```
RESET MASTER [TO NUMBER]；#删除所有二进制日志文件，并重新生成日志文件，文件名从'NUMBER'开始计数，默认从1开始，一般是master主机第一次启动时执行
```

切换日志文件：

```
FULSH LOGS;
```






## MySQL备份和恢复



### 备份恢复概述

1.为什么要备份：

​	灾难恢复；硬件故障；软件故障；自然灾害；黑客攻击；误操作；

2.备份的类型：

* 完全备份，部分备份
  * 完全备份：整个数据库全部数据
  * 部分备份：只备份数据，是一个子集
* 完全备份、增量备份、差异备份
  * 增量备份：在完全备份以及之前的增量备份的基础上，只备份变化的数据，备份较快，恢复复杂，也就是下一次增量备份是建立在之前备份的基础上的，增量备份文件个数会越来越多，他的备份参照物是第一次完全备份文件以及之后每次的增量备份文件
  * 差异备份：在完全备份的基础上，只备份与完全备份时不相同的数据。备份较慢，恢复简单。他的参照物就是第一次的完全备份，之后每次的差异备份都会把当今的数据和完全备份的数据进行对比，然后把有差异的部分备份下来。他的备份文件就是两部分，一部分是完全备份，一部分是差异备份，但是时间长了，差异备份文件就会变大，备份起来也更慢

![增量备份](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200928095706768.png)

​									图1.增量备份



![差异备份](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200928095754867.png)

​									图2.差异备份

* 冷备份、温备份、热备份

  * 冷备份：读、写操作均不可进行，数据库停止服务
  * 温备份：读操作可执行，但写操作不可执行
  * 热备份：读、写操作均可执行

  注：MyISAM支持冷备份和温备份，不支持热备份；InnoDB三种备份都支持

* 物理备份和逻辑备份

  * 物理备份：直接复制数据文件进行备份，与存储引擎有关，占用较多的空间，速度快
  * 逻辑备份：从数据库中“导出”数据另存而进行的备份，与存储引擎无关，占用空间少，速度慢，可能丢失精度



3.备份什么

* 数据

* 二进制日志、InnoDB事务日志

* 用户账户，权限设置，程序代码（存储过程、函数、触发器、事件调度器）

* 服务器的配置文件

* 表、视图、触发器存放在对应数据库的文件夹下

  函数、存储过程存放在mysql.proc表中

  事件存放在mysql.event表中

物理备份：

* /data/mysql/* 所有数据文件
* /etc/my.cnf 服务配置文件
* /data/logbin/* 二进制日志

逻辑备份：

* mysql库（这里也包括了函数、存储过程、事件）
* 用户数据库

4.备份注意要点

* 能容忍最多丢失多少数据
* 备份产生的负载
* 备份过程的时长
* 温备份的持续锁多久
* 恢复数据需要在多长时间内完成
* 需要备份和恢复哪些数据

5.还原要点

* **做还原测试，用于测试备份的可用性**
* **还原演练，写成规范的技术文档**

6.备份工具

* cp,tar等复制归档工具；物理备份工具，适用于所有存储引擎；只支持冷备；完全和部分备份
* LVM的快照：先加读锁，做快照后解锁，几乎热备；借助文件系统工具进行备份
* mysqldump：逻辑备份工具，适用于所有存储引擎，支持完全备份和部分备份；对InnoDB存储引擎支持热备，结合binlog的增量备份；对MyISAM存储引擎进行温备份；
* xtrabackup：由Percona提供的支持对InnoDB做热备（物理备份）的工具，支持完全备份、增量备份
* MariaDB Backup：从MariaDB10.1.26开始集成
* mysqlbackup：热备份
* mysqlhotcopy：Perl语言实现，几乎冷备份，仅适用于MyISAM存储引擎，使用LOCK TABLES、FLUSH TABLES和cp或scp来快速备份数据库

### mysqldump备份工具

避免DDL语句的出现

#### mysqldump 说明

```
mysqldump database [table]  #支持指定数据库和数据表的备份，但是数据库本身的定义没有备份，不推荐使用
OR
mysqldump [options] -B database1 [db2,db3...]  #支持指定数据库备份，包含数据库本身定义也会备份，推荐使用
OR
mysqldump [options] -A [options]   #备份所有数据库，包含数据库本身定义也会备份，注意information_schema和performance_schema表不会备份，这两个是内存中使用的表
```

参数说明：

```
-A,--all-databases #备份所有数据库，包含数据库定义语句；不备份Information_schema和performance_schema表
-B --database db_name  #指定备份的数据库，包括数据库定义语句
-E --events  #备份相关的所有event scheduler
-R --routines  #备份所有函数和存储过程
--triggers  #备份所有触发器，默认启用
--default-character-set=  #指定默认字符集
--master-data[=N]  #这个选项必须开启二进制日志使用
#选项是1或者2；默认是1；
#这个选项本身的意思是在生成的备份文件中（*.sql）添加一句 CHANGE MASTER TO 语句，1是不注释这句话，适合用于主从复制，2是注释这句话，适合单机使用
#CHANGE MASTER TO 指明了备份完的数据库后面的内容从哪里继续了，给人一个明确的文件和文件位置
#这个选项会默认打开-x或者--lock-all-tables功能，也就是锁全表功能，除非打开了--single-transaction选项
-F --flush-logs  #锁表完成后刷新日志，从新的日志文件开始，配合-A或-B的话会导致多次刷新日志，可以配合--single-transaction，--master-data，这样就只刷新一次
--compact   #去掉注释的内容，适合调试时使用，节约一些空间，生产中不用
-d --no-data  #只备份数据结构，不备份数据，即备份create xxxx
-t --no-create-info  #只备份数据，不备份结构，即没有create xxx
-n --no-create-db  #不备份create database，可以被-A和-B覆盖
--flush-privileges  #刷新权限，备份mysql库时要使用
-f --force #忽略SQL错误，继续执行
--hex-blob  #使用十六进制符号转储二进制列，当包含BINARY,VARBINARY,BLOB,BIT的数据类型的列时使用
-q --quick  #不缓存查询，直接输出，加快备份速度

与MyISAM存储引擎有关的备份选项：
MyISAM不支持事务，只能支持温备，不支持热备，所以备份前要锁定要备份的数据库，而后启动备份操作
-x --lock-all-tables  #加全局读锁，锁定所有库的所有表
-l --lock-tables  #对于需要备份的每个数据库，在启动备份之前分别锁定其所有表

与InnoDB存储引擎有关的备份选项：
InnoDB存储引擎支持事务，可以利用事务的相应的隔离级别，实现热备，也可以实现温备，但不建议使用
--single-transaction
此选项在备份前开启一个事务，start transaction。
注意，事务的基础是MVCC，多版本并发控制。MVCC是由存储引擎支持的（只有InnoDB支持）。
MVCC可以管理DML语句，但是DDL语句就无能为力了，因此在开启一个事务时其他人不要使用DDL语句（CREATE,ALTER,DROP,TRUNCATE,RENAME）。
```



InnoDB备份建议命令：

```
mysqldump -uroot -p -A -F -E -R --single-transaction --master-data=1 --flush-privileges --default-character-set=utf8mb4 --hex-blob | gzip > /backup/full.sql.gz
```



MyISAM备份建议命令：

```
mysqldump -uroot -p -A -E -R -F -x --master-date=1 --flush-privileges --default-character-set=utf8mb4 --hex-blob | gzip > /backup/full.sql.gz
```



#### mysql分库备份实战

```
mysqldump -B database  | gzip > backup.sql.gz
```

范例：分库备份

```
for db in `mysql -uroot -pcentos -e 'show databases' | grep -vE "Database|information_schema|performance_schema"`;do mysqldump -uroot -pcentos -B $db | gzip > /backup/$db-`date +%F-%H-%M-%S`.sql.gz;done;
```

```
mysql -uroot -pcentos -e 'show databases' | grep -Ev "Database|information_schema|performance_schema"|while read db;do mysqldump -uroot -pcentos -B $db | gzip > /backup/$db-`date +%F_%H-%M-%S`.sql.gz;done
```



#### mysqldump 备份还原实战案例

如何开启二进制日志；二进制日志的结构，是不是可以直接阅读的；是不是可以直接修改二进制日志文件中的drop,delete等行就可以把数据恢复回来了；数据备份了如何还原；

没有二进制文件也可以备份，把想要备份的数据库备份了就行

数据库的恢复：在mysql命令行中 source /backup/xxx.sql  直接进行恢复

分数据库进行备份

下面的操作是把需要备份的数据库名字找到，找到后使用 mysqldump -B 进行备份

```
[root@localhost ~]#mysql -uroot -p -e 'show databases'|grep -Ev '^(Database|information_schema|performance_schema)$'|while read db;do echo "备份$db";done
Enter password: 
备份hellodb
备份mysql
```

全部备份，实现增量备份

```
备份文件放在/backup下
日志文件放在/data/mysql/mylogbin-xxxx 
```

1.全部备份，依靠二进制文件，生成一个.sql文件

2.对数据库进行操作，增加删除数据，修改数据表等操作

3.发现删错数据了，想要还原

4.还原需要依靠一直开着的二进制日志才能完成，要不然只能回复到备份状态，中间所有操作都会丢失。开启二进制日志的话，不管中间做了什么，都能保留下来，只把删除语句删除了就行

5.去完全备份文件中找到备份结束的标志：CHANGE MASTER TO ，并查看它把下一步日志指向了哪个二进制日志文件，位置是哪里

```
-- CHANGE MASTER TO MASTER_LOG_FILE='mylogbin.000002', MASTER_LOG_POS=383;
```

6.找到这个二进制日志文件，找到这个位置，用mysqlbinlog

```
myslqbinlog /data/mysql/mylogbin.000002 --start-position=383
```

7.把从这个位置之后的内容重定向到一个新文件inc.sql中，这代表了从完全备份后的增量日志文件，通过这个文件可以把增量的数据都找到

```
mysqlbinlog /data/mysql/mylogbin.000002 --start-position=383
 > /backup/inc.sql
```

8.因为是错误删除数据了，所以去这个增量文件中找到删除命令 'DROP TABLE',然后把这行删除就行了，这样再把这个文件还原了就能获得没有被删除的数据了

```
sed -i.bak '/^DROP TABLE/d' inc.sql
```

9.停止访问数据库，在mysql客户端命令行中关闭二进制日志，准备还原 

```
set sql_log_bin=0
```

10.还原，先还原完全备份，然后再还原增量文件

```
source /backup/full_backup.sql
source /backup/inc.sql
```

11.查看数据库，发现数据都回来了

### xtrabackup备份工具



#### xtrabackup工具介绍

官方文档：https://www.percona.com/doc/percona-xtrabackup/LATEST/index.html

自MariaDB10.2.7（含）以上版本，不再支持使用Percona XtraBackup工具在线物理热备份。使用mariabackup





#### xtrabackup安装

```
yum -y install xtrabackup
```



```
perl-Digest-MD5
```

#### xtrabackup用法

xtrabackup使用分三步：

1.备份

```
[root@centos8 ~]#xtrabackup --user=root --password='centos' -S /tmp/mysql.sock --backup --target-dir=/backup/
```

2.预准备

这一步是为了让二进制日志文件和数据库中的数据保持一致性。

因为在对数据库进行操作时，会先把操作写入日志，然后经过提交事务后才会对数据库进行操作。所以要把日志中还没提交的事务回滚，把已经提交的事务同步给数据库。这样就能让日志和数据库保持一致性

```
[root@centos8 data]#xtrabackup --prepare --target-dir=/backup/
```



3.还原

这一步中有要求：

* 数据库服务要停了
* 存放数据文件的文件夹必须为空

然后执行还原命令

```
[root@centos8 data]#xtrabackup --copy-back --target-dir=/backup/
```

最后把数据文件的所有者和所有组修改成mysql

#### 利用xtrabackup实现完全备份及还原



#### 利用xtrabackup实现完全，增量备份及还原

增量备份

1.完全备份

```
xtrabackup --user=root --password='centos' -S /tmp/mysql.sock --backup --target-dir=/backup/bak
```

2.修改数据，实现增量数据

3.增量备份一次

```
xtrabackup --user=root --password=centos -S /tmp/mysql.sock --backup --target-dir=/backup/inc --incremental-basedir=/backup/bak/
```

4.再次修改数据，实现增量数据

5.增量备份一次

```
xtrabackup --user=root --password=centos -S /tmp/mysql.sock --backup --target-dir=/backup/inc2 --incremental-basedir=/backup/inc/
```

6.出现错误想要还原数据

7.相比完全备份的还原，增量备份的还原在prepare阶段需要进行一次完全备份准备以及多次增量备份准备（增量备份了几次就还原几次）

```
xtrabackup --prepare --apply-log-only --target-dir=/backup/bak/
```



```
xtrabackup --prepare --apply-log-only --target-dir=/backup/bak/ --incremental-dir=/backup/inc/
```



```
xtrabackup --prepare  --target-dir=/backup/bak/ --incremental-dir=/backup/inc2/
```

8.还原过程与完全备份一样，需要停止服务，清空数据文件的存放目录，以及最后把所有者和所有组改成mysql

```
service mysqld stop
rm -rf /data/mysql/*
```



```
xtrabackup --copy-back --target-dir=/backup/bak/
```



```
chown -R mysql.mysql /data/mysql/
```



#### xtrabackup单表导出和导入

单表导出

```
xtrabackup --user=root --password=centos --backup --target-dir=/backup/ --tables='^hellodb[.]students' -S /tmp/mysql.sock
```



## MySQL集群Cluster



### MySQL主从复制

#### 1.主从复制架构和原理

1.1 服务性能扩展方式

* Scale Up，向上扩展，垂直扩展
* Scale Out，向外扩展，横向扩展

1.2 MySQL的扩展

* 读写分离
* 复制：每个节点都有相同的数据集，向外扩展，基于二进制日志的单向复制

1.3 复制的功用

* 实现数据分布
* 实现负载可以均衡地读
* 实现备份
* 实现高可用和故障切换
* MySQL升级测试

1.4 复制的架构

一主一从的复制架构

![image-20200929165555349](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200929165555349.png)

一主多从的复制架构

![image-20200929165621541](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200929165621541.png)

1.5 主从复制的原理

![image-20200929171429834](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200929171429834.png)

主从复制相关线程：

主节点：

* dump Thread：为每个Slave的 I/O Thread启动一个dump线程，用于向其发送binary log events

从节点：

* I/O Thread：向Master请求二进制日志事件，并保存于中继日志(RELAY LOG)中
* SQL Thread：从中继日志中读取日志事件，在本地完成重放

根复制相关的文件：

* master.info：用于保存slave连接至master时的相关信息，例如账号、密码、服务器地址等
* relay-log.info：保存在当前slave节点上已经复制的当前二进制日志和本地relay log日志的对应关系
* mariadb-relay-bin.00000#：中继日志，保存从主节点复制过来的二进制日志，本质就是二进制日志

1.6 主从复制特点

* 异步复制
* 主从数据不一致比较常见

1.7 各种复制架构

* 一主一从
* 一主多从
* 从服务器可以再有从服务器
* 主主
* 一从多主：适用于多个不同数据库
* 环状复制

![image-20200929173016961](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20200929173016961.png)

复制时需要考虑二进制日志事件的记录格式

* STATEMENT（5.0之前）
* ROW（目前推荐）
* MIXED

#### 2.实现主从复制配置

配置分为主节点的配置和从节点的配置

**主节点配置：**

（1）启用二进制日志

```
[mysqld]
log_bin 
#或者指定log_bin=/backup/logbin，这样指定路径的方式一定要给备份路径更改用户组和用户为mysql，否则会导致服务无法启动
```

（2）为当前节点设置一个全局唯一的ID号

```
[mysqld]
server-id = NUMBER 
log-basename = master #可选性，设置二进制日志名称，确保不依赖于主机名
```

说明：server-id的取值范围

1 to 4294967295 (2^32) （mariadb 10.2.2 以上版本）默认值为1

0 to 4294967295 (2^32) （mariadb 10.2.1 以下版本）默认值为0

（3）创建有复制权限的用户账号

```
CREATE USER 'repluser'@'10.0.0.%' identified by 'centos';

GRANT REPLICATION SLAVE ON *.* TO 'repluser'@'10.0.0.%';

```

（4）查看从二进制日志的文件和位置开始进行复制

```
SHOW MASTER status;
 binlog.000016 |      725 
```

**从节点配置**

（1）启动中继日志

```
[mysqld]
server-id = NUMBER #为当前节点设置一个全局唯一的ID号
log-bin
read_only = ON   #设置数据库只读，针对supper user无效，注意这个意思是从服务器的超级用户是可以更改数据的！！！
relay_log = relay-log  #relay log的文件路径，默认值hostname-relay-bin
relay_log_index = relay-log.index #默认值hostname-relay-bin.index
```

（2）登录本机的数据库，并启动复制线程

```
1.登录本机的数据库
2.开始设置：
CHANGE MASTER TO MASTER_HOST='192.168.8.8',
MASTER_USER='repluser',
MASTER_PASSWORD='centos',
MASTER_PORT=3306,
MASTER_LOG_FILE='mariadb-bin.000002',
MASTER_LOG_POS=545;

START SLAVE;  #启动复制线程
SHOW SLAVE STATUS;

change master to  是个命令，不是二进制日志专用的

```

如果主节点是新建的库，那么就按照这个步骤执行；如果主节点已经运行了，新增从节点，那么就需要做这几件事：

1.给主节点全部备份

```
mysqldump -uroot -pcentos -A -E -R -F --single-transaction --master-data=1 --flush-privileges --hex-blob --default-character-set=utf8mb4 | gzip > /backup/test1/full.sql.gz 
```

2.查看主节点当前的二进制日志和日志中的位置

```
show master status
binlog.xxxx   727
```

3.从节点导入全部备份，并且设置开始复制的主节点二进制日志和日志中的位置，这样设置的意思就是，之前的内容我从节点都有了，之后的内容就从你现在的位置开始复制

#### 3.主从复制相关设置

1.解决复制冲突

```
--sql_skip_errors,sql_slave_skip_counter
可以在从服务器忽略几个主服务器的复制事件，或跳过事件的ID
通过 SHOW SLAVE STATUS; 可以看到错误码，然后两种方法跳过：
第一种：
stop slave; #关闭slave线程
set global sql_slave_skip_counter = 1;
start slave; #开启slave线程
第二种：
systemctl stop mysqld
vim /etc/my.cnf
[mysqld]
slave_skip_errors=ERROR_CODE | ALL  #这里填写错误码
systemctl start mysqld
```

2.保证主从复制的事务安全

在master节点启用参数

```
sync_binlog=1;#每次写后都把二进制日志同步到磁盘，性能差
#Innodb存储引擎的设置
innodb_flush_log_at_trx_commit=1 #每次事务提交立即同步日志写磁盘
innodb_support_xa=on  #分布式mariadb10.3.0废除
sync_master_info=NUMBER #number次事件后，master.info同步到磁盘
```

在slave节点启用服务器选项：

```
skip-slave-start=ON
```

在slave节点启用参数：

```
sync_relay_log=NUMBER #number次后同步relay_log到磁盘
sync_relay_log_info=NUMBER #number次事务后同步relay-log.info到磁盘
```

范例：当一个主服务器宕机，临时提升一个从服务器成为新的主服务器

```
步骤：
1.查看哪个从节点的数据是最新的，让它成为新的主服务器
cat /var/lib/mysql/relay-log.info
2.新的主服务器修改配置文件，关闭read-only配置，清除旧主服务器的复制信息
3.在新主服务器上全量备份
4.分析旧的主服务器的二进制日志，把没有同步给新主服务器的内容导出来，还原到新主服务器，尽可能恢复数据
5.其他所有从服务器重新还原数据库，指向新的主服务器
```

#### 4.实现级联复制

三台服务器：

master

级联

slave

就是级联服务器上配置了一项，然后最下级服务器的主服务器指向级联服务器

```
log_slave_updates   #级联服务器也可以修改二进制日志
```

slave服务器的主服务器指向了级联服务器

#### 5.主主复制

两个节点都可以更新数据，并且互为主从，容易差生数据不一致的问题，要慎用

#### 6.半同步复制

官方文档：https://mariadb.com/kb/en/semisynchronous-replication/#enabling-semisynchronous-replication

默认情况下，MySQL的复制功能是异步的，异步复制可以提供最好的性能，主库把binary log发送给从库即结束，并不验证从库是否接收完毕。这意味着当主服务器或从服务器端发生故障时，有可能从服务器没有接收到主服务器发送过来的binlog日志，这就会导致主服务和从服务器数据不一致。

半同步会有确认的过程，这个是通过一个插件实现的  Plugin: rpl_semi_sync_master

这个插件有两个部分，master和slave，通过INSTALL SONAME的方式安装以及在配置文件中写上插件选项及名称

```
The semisynchronous replication plugin is actually two different plugins--one for the master, and one for the slave. Shared libraries for both plugins are included with MariaDB. Although the plugins' shared libraries distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default prior to [MariaDB 10.3.3](https://mariadb.com/kb/en/mariadb-1033-release-notes/). There are two methods that can be used to install the plugin with MariaDB.
The first method
INSTALL SONAME 'semisync_master';INSTALL SONAME 'semisync_slave';
#这是我查看帮助命令看到的语法
INSTALL PLUGIN plugin_name SONAME 'shared_library_name'

The second method
[mariadb]
...
plugin_load_add = semisync_master

[mariadb]
...
plugin_load_add = semisync_slave
```

但是在mariadb 10.3.3开始这个插件就内置在数据库了，不需要去安装

在主从服务器上都打开这个选项

```
#这两个都是既是服务器选项又是全局变量
#主服务配置
rpl_semi_sync_mastere_enabled = ON
rpl_semi_sync_master_timeout=20000 #最大等待时间，这个代表20s
#从服务器配置
rpl_semi_sync_slave_enabled = ON
```



#### 7.复制过滤器

让从节点仅复制指定的数据库，或指定数据库的指定表

复制过滤器的两种实现方式：类似黑名单，一个是二进制日志黑名单，一个是中继日志黑名单

（1）服务器选项：主服务仅向二进制日志中记录与特定数据库相关的事件

也有优点：仅需要在主服务器上配置一次，从服务器都会生效；

缺点：主服务器仅把白名单中的数据库备份到二进制日志，其他数据库不记录，会造成数据风险；

不建议使用

（2）从服务器SQL_THREAD在读取relay log中的事件时，仅读取与特定数据库（表）相关的事件并应用于本地

会造成网络和磁盘I/O浪费

#### 8.复制主从加密

因为mysql的主从复制默认是不加密的，通信中的数据都是明文，很危险

通过SSL/TLS加密 transport layer security protocol，ssl是不安全的了，只是大家都这么叫这个协议，所以还留着他的名字

官方文档：https://mariadb.com/kb/en/library/replication-with-secure-connections/

需要CA Certificate Authority x509 非对称密钥对 openssl

需要开启ssl，需要生成一个需要ssl认证的账户

#### 9.GTID

Global Transaction Identity 全局事务标识符

mysql5.6之后支持，GTID不像传统的复制方式（异步复制、半同步复制）需要找到binlog文件名和position，它只需要知道master的IP、端口、账户、密码即可。开启GTID后，执行

change master to master_auto_position=1即可，它会自动寻找到相应的位置开始同步



3个实验：

1.主从复制 OK

2.一主多从

3.主从半同步复制

4.主从复制加密

5.GTID方式复制







#### 10.复制的监控和维护

#### 11.复制的问题和解决方案



### MySQL中间件代理服务器

#### 1.关系型数据库和非关系型数据库(NO SQL)数据库

#### 2.数据切分

#### 3.MySQL中间件各种应用

#### 4.Mycat

#### 5.ProxySQL



### MySQL高可用

#### 1.MySQL高可用解决方案

#### 2.MHA Master High Availability

#### 3.Galera Cluster

#### 4.TiDB

## 性能优化



### 压力测试工具



### 生产环境my.cnf配置案例



### MySQL配置最佳实践







重点：

1.表DML ！！！

2.表DQL 单表和多表 ！！！

3.视图、函数、触发器、存储过程、事件 作为了解

4.用户账号的创建、删除、修改密码  ！

5.用户授权管理  ！



1.存储引擎的区别

2.索引

3.事务

4.备份

5.数据库的主从



1.备份工具的使用：mysqldump,xtrabackup,mariadbbackup





两个问题：

1.如何把多行命令写进去，或者说CHANGE MASTER TO 到底什么要求

2.写进去了为什么还是连接不上级联节点

要做的事情，

重新配置级联节点和从服务器，如果写不进去也先手写进去，先保证写进去了能连接





CHANGE MASTER TO  MASTER_HOST='source2.example.com',  MASTER_USER='replication',  MASTER_PASSWORD='*password*',  MASTER_PORT=3306,  MASTER_LOG_FILE='source2-bin.001',  MASTER_LOG_POS=4,  MASTER_CONNECT_RETRY=10;





