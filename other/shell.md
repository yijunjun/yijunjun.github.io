# 常用shell脚本

## 获取脚本所在目录

```bash
dirname $0

cd `dirname $0`

pwd
```

## 查看系统启动运行时间

```bash
uptime
当前时间、系统已经运行了多长时间、目前有多少登陆用户、系统在过去的1分钟、5分钟和15分钟内的平均负载。
```

## 解决ssh超级慢(去除服务端利用dns反查客户端)

```bash
vi /etc/ssh/sshd_config
UseDNS no
GSSAPIAuthentication no
```