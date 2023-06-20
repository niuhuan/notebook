

## 访问超时

在Podfile中添加

```
source 'https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git'
```

解决 `cdn.jsdelivr.net` 访问超时

```
CDN: trunk URL couldn't be downloaded: https://cdn.jsdelivr.net/cocoa/Specs/c/0/4/Flutter/1.0.0/Flutter.podspec.json Response: Timeout was reached
```