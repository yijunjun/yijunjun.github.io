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

###  忽略本地已有包

```bash
"ignore": "test xxx.com/",
```

### 添加第三方库

```bash
govendor add +e
```

## 号称最快的json处理库
[json-iterator](https://github.com/json-iterator/go)

## 常用的日志库
[zap](https://github.com/uber-go/zap)
[zerolog](https://github.com/rs/zerolog)

## 操作excel/xlsx
[360出品](https://github.com/360EntSecGroup-Skylar/excelize)
[xlsx](https://github.com/tealeg/xlsx)

## 死锁检测库
[go-deadlock](github.com/sasha-s/go-deadlock)

## 号称最快的web框架
[iris](https://github.com/kataras/iris)

## golang实现lua虚拟机
[gopher-lua](https://github.com/yuin/gopher-lua)

## 全自动apidoc生成
[yaag](https://github.com/betacraft/yaag)

## 类似django框架的web框架
[qor-admin](https://github.com/qor/admin)

## 一些有用的tips
[tips](https://go101.org/article/tips.html)