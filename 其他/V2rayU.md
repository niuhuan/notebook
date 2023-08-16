V2rayU
======

## 解决反复提示`对电脑有害`

1. 在访达`V2rayU.app`中勾选`覆盖恶意软件保护`打勾

2. 执行Shell
```shell
codesign --force --deep --sign - /Applications/V2rayU.app
```

3. 点取消, 重新打开App
