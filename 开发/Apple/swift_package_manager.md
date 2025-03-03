
本地缓存地址 
`/Library/org.swift.swiftp`

如果xcode无法下载依赖
```shell
export http_proxy=socks5://127.0.0.1:1081/
export https_proxy=socks5://127.0.0.1:1081/
export HTTP_PROXY=socks5://127.0.0.1:1081/
export HTTPS_PROXY=socks5://127.0.0.1:1081/
xport ALL_PROXY=socks5://127.0.0.1:1081/
export all_proxy=socks5://127.0.0.1:1081/
xcodebuild -resolvePackageDependencies -scmProvider system
```