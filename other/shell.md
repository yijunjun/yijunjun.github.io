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


## 列出所有监听tcp端口程序

```bash
netstat -ltpn
```

## 查看网卡流量

```bash
sar -n DEV 1 10
```
> 注：每1秒 显示 1次 显示 10次

## 利用git部署更新脚本

```bash
#!/usr/bin/env bash

cd ${gitdir}
# 批量杀死监控进程 shell脚本或专用管理程序
ps -ef|grep xxx | grep -v grep | awk '{print $2}' | xargs kill -9
# 批量杀死目标进程
ps -ef|grep yyy | grep -v grep | awk '{print $2}' | xargs kill -9
# 拉取最新程序
git pull
# 跑起监控进程
nohup ./xxx.sh >/dev/null 2>&1 &
```

## 监控脚本

```bash
#!/usr/bin/env bash

while true
do
   # 查看目标进程还在不在
   procnum=` ps -ef|grep "yyy$"|grep -v grep|wc -l`
   if [ $procnum -eq 0 ]; then
   		cd ${basedir}
   		nohup ./yyy >/dev/null 2>&1 &
   fi

   # 延时30秒
   sleep 30
done
```