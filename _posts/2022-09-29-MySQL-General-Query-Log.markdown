---
layout: post
title:  "MySQL-General-Query-Log"
date:   2022-09-29 18:13:10 +0800
categories: jekyll update
---



### General Query Log是什么

MySQL通用查询日志用于记录 **mysqld**当前执行的sql。记录了所有的客户端连接/断开连接信息，以及接收到的所有的客户端发送过来的SQL语句。当排查错误时，想知道客户端发送到服务端的精确的sql语句时，该日志非常有用。
下面是截取的一段日志内容示例：

![]({{site.url}}\assets\img\20220928132752.png)
### 如何开启
默认情况下，该日志是禁用状态的。可以使用 *--general_log[={0|1}]* 来指定，不带参数或者设置为=1表示开启通用查询日志。使用=0表示禁用。
使用 *--general_log_file=file_name* 来指定日志文件名。如果不指定日志文件名，则默认使用主机名.log，例如 localhost.log

### 运行时开启/关闭
使用系统变量 *general_log* 和 general_log_file 分别控制通用查询日志的开启或者关闭。

![]({{site.url}}\assets\img\20220928134052.png)

上面的例子是开启了通用日志查询，且未指定文件名（使用主机名 DB01-TEST-XM来命名的）

关闭：
> set GLOBAL general_log = 'OFF';

![]({{site.url}}\assets\img\20220928140925.png)打开:

> set GLOBAL general_log = 'ON';

![]({{site.url}}\assets\img\20220928141102.png)

设置日志中的时间时区:
默认情况下，MySQL的时间设置为
> show variables like 'log_timestamps';
>
> ![]({{site.url}}\assets\img\20220928141758.png)

需要将其通用一下语句改为系统时区时间
> set GLOBAL log_timestamps = SYSTEM ;

![]({{site.url}}\assets\img\20220928141948.png)

### 日志清理
1. 使用mysqladmin
> \# 将查询日志重命名，因为此时文件id不变，后续日志会继续写入该文件
>
> mv host_name.log host_name-old.log
>
> 
>
> \# 使用mysqladmin 刷新日志
>
> mysqladmin flush-logs
>
> \# 将日志备份到其他地方
>
> my host_name-old.log backup-dir

![]({{site.url}}\assets\img\20220928152708.png)

2. 关闭备份再打开的方式

1. 将查询日志关闭
> SET GLOBAL general_log = 'OFF' ;
2. 使用命令行或其他方式将日志重命名
3. 再次打开查询日志
>  SET GLOBAL general_log = 'ON' ;