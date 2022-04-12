Android - 修复Wifi显示X号
=======================

### 修改 Generate204 的网址
用来删除非大陆ROM中Wifi或4G网络的X号

```shell
adb shell settings put global captive_portal_https_url https://www.google.cn/generate_204
```

### 恢复设置

```shell
adb shell settings delete global captive_portal_server
```



