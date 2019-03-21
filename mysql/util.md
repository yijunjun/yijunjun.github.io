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

# 设置连接超时时间
```sql
show variables like '%timeout%';
--604800=60*60*24
set interactive_timeout=604800; 
set wait_timeout=604800;
```

# 免密码登陆

```bash
vi ~/.my.cnf
[client]
host = "localhost"
user = "user"
password = "pwd"
```