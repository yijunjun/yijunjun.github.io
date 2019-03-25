# 利用explain查看及优化sql

```sql
explain select col from table where con group by xx order by yy;  
```

输出说明:
1. table 显示该语句涉及的表
2. type 这列很重要，显示了连接使用了哪种类别,有无使用索引，反映语句的质量。
3. possible_keys 列指出MySQL能使用哪个索引在该表中找到行
4. key 显示MySQL实际使用的键（索引）。如果没有选择索引，键是NULL。
5. key_len 显示MySQL决定使用的键长度。如果键是NULL，则长度为NULL。使用的索引的长度。在不损失精确性的情况下，长度越短越好
6. ref 显示使用哪个列或常数与key一起从表中选择行。
7. rows 显示MySQL认为它执行查询时必须检查的行数。
8. extra 包含MySQL解决查询的详细信息。
9. 其中：Explain的type显示的是访问类型，是较为重要的一个指标，结果值从好到坏依次是：
    system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL（优-->差）　一般来说，得保证查询至少达到range级别，最好能达到ref，否则就可能会出现性能问题

小米出品[soar](https://github.com/XiaoMi/soar)工具,建议使用一下.本机安装在~/gopath/bin.


# 碎片产生的原因

1. 表的存储会出现碎片化，每当删除了一行内容，该段空间就会变为空白、被留空，而在一段时间内的大量删除操作，会使这种留空的空间变得比存储列表内容所使用的空间更大；

2. 当执行插入操作时，MySQL会尝试使用空白空间，但如果某个空白空间一直没有被大小合适的数据占用，仍然无法将其彻底占用，就形成了碎片；

3. 当MySQL对数据进行扫描时，它扫描的对象实际是列表的容量需求上限，也就是数据被写入的区域中处于峰值位置的部分；

4. 清除不要数据,记得要optimize table xx;不然空间仍旧占用.

> 例如：
一个表有1万行，每行10字节，会占用10万字节存储空间，执行删除操作，只留一行，实际内容只剩下10字节，但MySQL在读取时，仍看做是10万字节的表进行处理，所以，碎片越多，就会越来越影响查询性能。

# 通用日志,调试好帮手,需要root权限

```sql
show variables like '%general%';
set @@global.general_log=1;
set @@global.general_log=0;
```

# IF条件表达式

```sql
IF(expr1,expr2,expr3)
如果 expr1 为真(expr1 <> 0 以及 expr1 <> NULL)，那么 IF() 返回 expr2，否则返回 expr3。IF() 返回一个数字或字符串，这取决于它被使用的语境：
```

# 查询表中重复数据

```sql
select col from table group by col having count(col) > 1
```

# 带忽略重复的插入

```sql
insert ignore into table(name)  value('xx')
```

# 常用时间函数

```sql
FROM_UNIXTIME(unix_timestamp)是MySQL里的时间函数。

UNIX_TIMESTAMP('2018-09-17') 是与之相对正好相反的时间函数 。
```

# Binlog,阿里云的rds默认把它也计算在内,要手动设置控制大小.大量数据删除时,会突然增加Binlog文件.

# 查询数据库占用空间及索引空间

```sql
select TABLE_NAME, concat(truncate(data_length/1024/1024,2),' MB') as data_size,
concat(truncate(index_length/1024/1024,2),' MB') as index_size
from information_schema.tables where TABLE_SCHEMA = 'databaseName'
```

# concat把int转varchar类型
```sql
update user set nickname = concat(id,'号') where id > 0
```

# 查看客户端连接详情
```sql
show full processlist;
```

# 字符串替换
```sql
update user set nickname = REPLACE(id,'old', 'now') where id > 0
```

# 修改root密码
```bash
killall mysqld

mysqld_safe --skip-grant-tables &

update mysql.user set password=PASSWORD('newpassword') where user='root';

flush privileges;

mysqld_safe &
```

# 查看默认引擎
```sql
show engines;
```

# 设置默认字符集
```bash
mysql -u user -D db --default-character-set=utf8 -p
```

# 设置连接超时时间,下次登陆有效
```sql
show variables like '%timeout%';
--604800=60*60*24
set @@GLOBAL.interactive_timeout=604800;
set @@GLOBAL.wait_timeout=604800;
```

# 免密码登陆

1. 利用.my.cnf
```bash
vi ~/.my.cnf
[client]
# 注意mysql的库中user表,localhost和127.0.0.1区别
host = "127.0.0.1"
user = "user"
password = "pwd"
database = "xx"
```

2. 利用命令行参数,或者别名

```bash
mysql -hlocalhost -uroot -pxxx
```