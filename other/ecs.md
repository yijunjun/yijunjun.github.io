# 阿里云ECS高危漏洞问题处理

```bash
# 升级系统及软件就能解决多数
yum -y upgrade
```

# 服务器vi乱码
```bash
cd ~
vi .vimrc

set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
set number
filetype on
syntax on
```