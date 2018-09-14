# ungoogled-chromium

## 解决mac下chromium不能播放flash流程

### 下载flash插件

### [下载无google全家桶的chromium](https://github.com/Eloston/ungoogled-chromium)

### 备份浏览器

```bash
cd Chromium.app/Contents/MacOS
mv Chromium _Chromium
```

### 创建启动脚本

```bash
vi Chromium
/Applications/Chromium.app/Contents/MacOS/_Chromium \
--ppapi-flash-path=/Library/Internet\ Plug-Ins/PepperFlashPlayer/PepperFlashPlayer.plugin \
--ppapi-flash-version=30.0.0.154
```

### 增加执行权限

```bash
chmod +x Chromium
```

## google翻译插件默认网址被墙

### 搜索定位到插件地址

```bash
find . -name aapbdbdomjkkjkaonfhkkikfgjllcleb

./Library/Application Support/Chromium/Default/Extensions/aapbdbdomjkkjkaonfhkkikfgjllcleb

./Library/Application Support/Chromium/Default/Local Extension Settings/aapbdbdomjkkjkaonfhkkikfgjllcleb

cd ./Library/Application Support/Chromium/Default/Extensions/aapbdbdomjkkjkaonfhkkikfgjllcleb
```

### 替换网址

```bash
sed -i '.tmp' 's/translate.google.com/translate.google.cn/g' *.js
sed -i '.tmp' 's/translate.google.com/translate.google.cn/g' *.html
rm -rf *.tmp
```

### 重启浏览器