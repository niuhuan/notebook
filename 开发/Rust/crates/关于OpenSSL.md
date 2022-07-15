
mac/linux/windows 如果没有安装openssl依赖, 以及android/ios 不带openssl库,  可增加feature=vendored, 编译并连接

```toml
openssl = { version = "0.10.38", features = ["vendored"] }
```

PS:

Windows编译openssl连接的话, 自带的perl将无法编译成功, 需要使用完整的StrawberryPerl, 下载安装既可以
https://strawberryperl.com/

IOS-SIM 编译openssl库会报错 不能使用 vendored, 去除后编译成功, mac上已安装openssl, 未做更多测试
