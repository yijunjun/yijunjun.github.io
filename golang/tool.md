# dep官方第三方版本管理工具

## 安装

```bash
go get -u github.com/golang/dep/cmd/dep
```

## 初始化项目

```bash
dep init
```

## 添加第三方库

```bash
dep ensure -add github.com/foo/bar github.com/baz/quux
```