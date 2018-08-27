# 记录常用第三方工具

## dep官方第三方版本管理工具

### 安装

```bash
go get -u github.com/golang/dep/cmd/dep
```

### 初始化项目

```bash
dep init
```

### 添加第三方库

```bash
dep ensure
```

## 最符合国情的第三方版本管理工具

### 安装

```bash
go get -u -v github.com/kardianos/govendor
```

### 初始化项目

```bash
govendor init
```

### 添加第三方库

```bash
govendor add +e
```

## 号称最快的json处理库
[json-iterator](https://github.com/json-iterator/go)