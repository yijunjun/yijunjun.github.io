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

```bash
# FastCGI sent in stderr: "Primary script unknown" while reading response header from upstream,
# fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
```