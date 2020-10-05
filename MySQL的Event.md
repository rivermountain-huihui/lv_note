MySQL的Event

Event介绍：Event，事件，是MySQL在相应的时刻调用的过程式数据库对象。一个事件可调用一次，也可周期性的启动，它由一个特定的线程来管理的，也就是“事件管理器”

事件的优缺点：

* 优点：

  一些对数据定时性操作不再依赖外部程序，而直接使用数据库本身提供的功能，可以实现每秒钟执行一个任务，这在一些对实时性要求较高的环境下就非常使用

* 缺点：

定时触发，不可以直接调用

事件和触发器：

  事件和触发器类似，都是在某些事情发生的时候启动。当数据库上启动一条语句的时候，触发器就启动了，而事件是根据调度事件来启动的。由于他们彼此相似，所以事件也称为临时性触发器。



事件的开启：

--event_scheduler,event_scheduler

根据文档，event_scheduler既是服务选项也是系统变量，那么就既可以在配置文件中写上它从而开启，也可以通过SET global event_scheduler=1 来开启，当然变量方式开启是临时的。

通过查看线程命令可以看到event的线程 show processlist;



创建Event

```
CREATE 
	[DEFINER = {USER|CURRENT_USER}]
	EVENT
	[IF NOT EXISTS]
	event_name
	ON SCHEDULER scheduler #注释1
	[ON COMPLETION [NOT] PRESERVE] #注释2
	[ENABLE | DISABLE | DISABLE ON SLAVE] #注释3
	[COMMENT 'comment'] #这句就是代表注释的意思
	DO event_body #注释4
```

注释1：ON SCHEDULER 计划任务   

scheduler  决定event的执行时间和频率（时间一定要是未来的时间）,有两种形式“at”,“every”

scheduler:

AT TIMESTAMP [+ INTERVAL interval] | EVERY INTERVAL [STARTS timestamp] [ENDS timestamp]

interval:

quantity {YEAR|MONTH|DAY|HOUR|MINUTE|WEEK|SECOND|...}

注释2：ON COMPLETION [NOT] PRESERVE 默认是 ON COMPLETION NOT PRESERVE 任务执行后drop该事件；ON COMPLETION PRESERVE 就是不会DROP

注释3：ENABLE,DISABLE 开启或关闭事件 

查看EVENT

```
SHOW EVENT [{FROM|IN} SCHEMA_NAME] [LIKE 'pattern'|WHERE expr]
```

修改EVENT

```
ALTER
	EVENT event_name
	[ON SCHEDULER scheduler]
	[ON COMPLETION [NOT] PRESERVE]
	[RENAME TO new_event_name]
	[ENABLE|DISABLE]
	[DO event_body]
```

删除Event

```
DROP EVENT [if exists] event_name
```

