# 解决mac下chromium缺少 Google API 密钥

## [申请 Google API](https://cloud.google.com/console)

## 导出环境变量

```bash
export GOOGLE_API_KEY '生成的API密钥'
export GOOGLE_DEFAULT_CLIENT_ID '生成的客户端ID'
export GOOGLE_DEFAULT_CLIENT_SECRET '生成的客户端密钥'
```

## 重启浏览器