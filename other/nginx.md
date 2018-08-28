# 记录常用的nginx配置

## 1.http转向https

```bash
rewrite ^(.*) https://$server_name$1 permanent;
rewrite ^(.*) https://$host$1 permanent;
```

> 两种写法,各有适合场合<br>
>   $server_name, 由nginx配置决定
>   $host,由请求路径决定