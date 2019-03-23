# 记录常用的nginx配置

## http转向https

```bash
rewrite ^(.*) https://$server_name$1 permanent;
rewrite ^(.*) https://$host$1 permanent;
```

> 两种写法,各有适合场合
> $server_name, 由nginx配置决定
> $host,由请求路径决定

## php-fpm出现Primary script unknown问题

1. 尝试修改nginx配置

```bash
# FastCGI sent in stderr: "Primary script unknown" while reading response header from upstream,
# fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
```
2. 如果仍然不行,则打开php-fpm.conf日志配置

```bash
access.log = /var/log/php-fpm.$pool.access.log
```

3. 再打开nginx日志配置

```bash
; http
log_format scripts '$document_root$fastcgi_script_name > $request';
; server
access_log /usr/local/nginx/scripts.log scripts;
```

4. 重启nginx,和php-fpm 查看日志,一般是路径不对和权限不对


## php-fpm出现无法连接数库,可能是编译参数不对

```bash
./configure --enable-fpm --prefix=/usr/local/php --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd
```

## 查看当前nginx所用配置文件

```bash
# 获取nginx进程号
ps -ef | grep nginx
# 获取nginx路径
cd /proc/pid
ls -a
# 执行相应路径的语法测试,输出就能看到路径
nginx -t
```

## php输出数组

```php
echo "<pre>";print_r($arr);echo "<pre>";
```