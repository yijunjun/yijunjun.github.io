# git积累

## post-receive钩子,必须注意git pull引入$GIT_DIR变量
> 因此需要取消变量

```bash
cd xxx
unset $(git rev-parse --local-env-vars);
git pull
```

## 带用户名及密码的git clone 
```bash
git clone http://uer:pwd@xxx.git
```