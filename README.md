Notebook
========

https://github.com/niuhuan/notebook/

- 开发
    - Rust
        - [环境搭建](开发/Rust/env/环境搭建.md)
        - [笔记](开发/Rust/笔记.md)
          <br />
          [编译](开发/Rust/笔记.md#编译) ：
          [当用户选择一些features不合理时使得编译无法通过](开发/Rust/笔记.md#当用户选择一些features不合理时使得编译无法通过) 、
          <br />
          [字符串](开发/Rust/笔记.md#字符串) ：
          [根据字符截取](开发/Rust/笔记.md#根据字符截取) 、
          <br />
          [GO+跨平台](开发/Rust/GO+跨平台.md)
          <br />
        - Crates ：
          <br />
          [静态变量](开发/Rust/crates/静态变量.md) ： lazy_static 、 async_once 、 once_cell
          <br />
          [JSON (serde)](开发/Rust/crates/JSON.md) 、
          [关于OpenSSL](开发/Rust/crates/关于OpenSSL.md) 、
          [摘要及加密 (rsa/hex/md5)](开发/Rust/crates/摘要及加密.md) 、
          [rsmpeg (ffmpeg)](开发/Rust/crates/rsmpeg.md) 、
          [safer_ffi/python](开发/Rust/safer_ffi/python.md) 、 
    - Golang
        - [安装和配置](开发/Golang/安装和配置.md)
        - [自定义http域名解析](开发/Golang/自定义http域名解析.md)
        - [处理字符串](开发/Golang/处理字符串.md)
        - [EmbedFS](开发/Golang/EmbedFS.md)
        - FFI ：
          [使用CSharp调用Go](开发/Golang/使用CSharp调用Go.md)
          [CGO](开发/Golang/CGO.md)
        - Mods ：
          [解析类型不规范的json (json-iterator)](开发/Golang/FuzzyJson/解析类型不规范的json.md) 、
          [JWT-GO](开发/Golang/mods/JWT-GO.md) 、
    - java
        - [M1_ZULU_JDK](开发/Java/m1_zulu_jdk.md) 、
          [AOP_DEMO.zip](开发/Java/aop_lock.zip) 、
          [Graalvm](开发/Java/Graalvm.md) 、
          [Reactive](开发/Java/Reactive.md) 、
    - Node.js
        - [Yarn](开发/Node.js/Yarn.md)、
          [镜像](开发/Node.js/镜像.md)、
    - Flutter
        - [环境搭建](开发/Flutter/环境搭建.md)
          <br />
          配置 ：
          [镜像设置](开发/Flutter/环境搭建.md#环境变量) 、
          [Mirror](开发/Flutter/环境搭建.md#Mirror) 、
          [flutter2在m1上不能使用pod的解决方法](开发/Flutter/环境搭建.md#flutter2在m1上不能使用pod的解决方法) 、
          <br />
          构建 ：
          [构建对应架构的apk](开发/Flutter/环境搭建.md#构建对应架构的apk) 、
          [构建未签名的ipa、精简IPA体积](开发/Flutter/环境搭建.md#构建未签名的ipa) 、
          [构建linux桌面应用](开发/Flutter/环境搭建.md#构建linux桌面应用) 、
        - [常见问题](开发/Flutter/常见问题.md) 、
        - 依赖 ：
          [ObjectBox桌面端动态库](开发/Flutter/ObjectBox桌面端动态库.md)
    - Android
        - [AVD将证书导入ROOT](开发/Android/AVD将证书导入ROOT.md)
        - [读取图片并保存到相册](开发/Android/读取图片并保存到相册.md)
        - [读取图片并转成PNG](开发/Android/读取图片并转成PNG.md)
    - Apple
        - [超级签证书](开发/Apple/超级签证书.md) 、 [在手机端获取UDID](开发/Apple/其他.md#在手机端获取UDID) 、 [查看IPA签名有效期](开发/Apple/其他.md#查看IPA签名有效期)
        - [Pods](开发/Apple/Pods.md) ： [Pods访问超时](开发/Apple/Pods.md#访问超时) 、[iOS构建跳过签名](开发/Apple/iOS构建跳过签名.md)
        - [Swift package manager](开发/Apple/swift_package_manager)
    - 华为
      - 鸿蒙NEXT
        - [基础](开发/华为/鸿蒙NEXT/基础.md)
        - [发布](开发/华为/鸿蒙NEXT/发布.md)
    - 任天堂游戏机开发
        - [devkitPro开发环境搭建](开发/任天堂/devkitPro开发环境搭建.md)
    - Git
        - [Git](开发/Git/Git.md)
    - BuildSystem ：
      [Meson](开发/BuildSystem/Meson.md) 、
      [Gradle](开发/BuildSystem/Gradle.md) 、
      [CMake](开发/BuildSystem/CMake.md) 、
    - LLVM
        - [构建LLVM](开发/LLVM/构建LLVM.md)
    - 工具
        - [使用Web在远端使用VSCode](开发/工具/CodeServer.md) 、
          [网站工具](开发/工具/网站工具.md) 、
        - [制作ICO](开发/工具/制作ICO.md) 、
        - [IDEA](开发/工具/IDEA.md)
        - [OPENSSL](开发/工具/Openssl.md)
    - 正则表达式
        - [ipv4](开发/正则/ipv4.md) 、
          [ipv6](开发/正则/ipv6.md)
    - 聊天机器人
        - Telegram
            - [Telethon](开发/聊天机器人/Telegram/Telethon.md)
    - 物联网
        - [Micropython](开发/物联网/Micropython.md) 、
        - [ESP-DEV](开发/物联网/ESP-DEV.md) 、
        - [用Rust开发ESP32](开发/物联网/用Rust开发ESP32.md) 、
        - [用Golang开发单片机](开发/物联网/用Golang开发单片机.md) 、 
- 运维
    - AWS
        - [亚马逊云LINUX安装docker](运维/亚马逊云/亚马逊云LINUX安装docker.md)
    - 阿里云
        - [Serverless](运维/阿里云/Serverless.md)
    - Docker
        - [常用服务](运维/Docker/常用服务.md)
        - [常见问题](运维/Docker/常见问题.md) (
          [请求其他服务证书不信任解决](运维/Docker/常见问题.md#请求其他服务证书不信任解决) 、
          [日志过大](运维/Docker/常见问题.md#日志过大) 、
          [查看容器的设置](运维/Docker/常见问题.md#查看容器的设置) 、
          [清理容器的日志](运维/Docker/常见问题.md#清理容器的日志) 、
          )
        - [阿里云镜像仓库使用](运维/Docker/阿里云镜像仓库使用.md)
        - [新版Debian或Ubuntu执行`pip Install`](运维/Docker/新版Debian或Ubuntu执行pip_install.md)  
      - [安装](运维/Docker/安装.md)
        [OracleLinux](运维/Docker/安装.md#Oracle-linux) 、
      - [卸载](运维/Docker/卸载.md)
    - 服务
        - OpenVPN : [Centos搭建OpenVPN](运维/服务/OpenVPN/Centos搭建OpenVPN.md)
        - Mongo : [鉴权](运维/服务/Mongo/鉴权.md)
        - MySQL : [MySQL绿色启动](运维/服务/MySQL/MySQL绿色启动.md)
        - Nginx : [编译Nginx](运维/服务/Nginx/编译Nginx.md)
        - Matrix : [Matrix服务](运维/服务/Matrix/Matrix服务.md) 、 [MatrixDockerCompose（带推送）](运维/服务/Matrix/MatrixDockerCompose.md)
    - 工具和其他
        - [CURL](运维/工具和其他/CURL.md)([发送邮件](运维/工具和其他/CURL.md#发送邮件))
        - [systemctl](运维/工具和其他/systemctl.md)
    - 文件系统 
        - [解决XFS系统挂载失败](运维/文件系统/解决XFS系统挂载失败.md)
        - [解决卸载分区显示Busy](运维/文件系统/解决卸载分区显示Busy.md)
     
- 系统
    - MacOS
        - [制作安装U盘](系统/MacOS/制作安装U盘.md) 、
          [开启NFS服务](系统/MacOS/开启NFS服务.md) 、
          [开放DockerTCP端口](系统/MacOS/开放DockerTCP端口.md)
        - [将命令行注册为系统服务](系统/MacOS/将命令行注册为系统服务.md) 、
          [WindowsTerminalSSH乱码解决](系统/MacOS/WindowsTerminalSSH乱码解决.md)
        - [软件](系统/MacOS/软件.md)
    - Android
        - [修复Wifi显示X号](系统/Android/修复Wifi显示X号.md)
    - SBC
        - [使用树莓派当无线网卡](系统/SBC/使用树莓派当无线网卡.md)
    - VMWare
        - [解锁MacOS系统](系统/VMWare/解锁macOS系统.md)
    - QEMU
        - [基本操作](系统/Qemu/基本操作.md) 、 [M1上运行linux虚拟机](系统/Qemu/M1上运行linux虚拟机.md)
    - Windows
        - 应用 ：
          [OhMyPosh](系统/Windows/OhMyPosh.md) 、
          [WINGET](系统/Windows/WINGET.md)
        - 远程或无人值守 ：
          [禁用断电重启后进入恢复模式](系统/Windows/禁用断电重启后进入恢复模式.md) 、
          [Win10启用SSH服务器并使用证书登录](系统/Windows/Win10启用SSH服务器并使用证书登录.md) 、
          [将可执行程序注册成服务](系统/Windows/将可执行程序注册成服务.md) 、
          [通过命令进行端口转发](系统/Windows/通过命令进行端口转发.md) 、
        - 高级功能 ：
          [Windows沙盒](系统/Windows/Windows沙盒.md)
        - WSL ：
          [WSL开启SSH](系统/Windows/WSL开启SSH.md) 、
          [WSL禁用Windows环境变量](系统/Windows/WSL禁用Windows环境变量.md) 、
          [WSL启动MYSQL](系统/Windows/WSL启动MYSQL.md) 、
        - WSA ：
          [Win11安装WSA](系统/Windows/Win11安装WSA.md) 、
        - Windows设置 ：
          [Windows11跳过OOBE登录](系统/Windows/Windows11跳过OOBE登录.md) 、
          [Win10家庭版开启gpedit](系统/Windows/Win10家庭版开启gpedit.md) 、
          [Win10/11家庭版开启Hyper-V](系统/Windows/Win10_11家庭版开启hyper_v.md) 、
          [STLC安装APPX](系统/Windows/STLC安装APPX.md) 、
          [开机启动指定应用](系统/Windows/开机启动指定应用.md) 、
          [多用户远程连接](系统/Windows/多用户远程连接.md) 、
        - 其他 ：
          [解决端口没被占用但是无法使用](系统/Windows/解决端口没被占用但是无法使用.md) 、
          [BitLocker](系统/Windows/BitLocker.md) 、
          [PowerShell](系统/Windows/PowerShell.md) 、
    - Linux
        - [Debian历史版本](系统/Debian/历史版本.md)
        - [UbuntuServer安装时未配置网络](系统/Ubuntu/UbuntuServer安装时未配置网络.md)
- 游戏
    - 工具
        - [远程游玩](游戏/工具/远程游玩.md)
    - 宝可梦                    azx
        - [图鉴与翻译](游戏/宝可梦/图鉴与翻译.md)
    - 怪物猎人
        - [NS平台XX/GU启用60帧](游戏/怪物猎人/NS平台XX-GU启用60帧.md)
    - [原神](游戏/原神.md)
    - 平台
        - [Steam](游戏/平台/Steam.md) :  [自定义Cover和Banner下载](游戏/平台/Steam.md#自定义cover和banner下载)
    - 设备
        - [SteamDeck](游戏/设备/SteamDeck.md)
- 其他
    - [GoogleVoice自动保号](其他/GoogleVoice自动保号.md)
    - [AppStore取消购买和退款](其他/AppStore取消购买和退款.md)
    - [Telegram](其他/Telegram.md)<br />
        - [设置成中文](其他/Telegram.md#设置成中文) 、
          [删除账号](其他/Telegram.md#删除账号) 、
          [关闭Apple平台的敏感信息过滤](其他/Telegram.md#关闭apple平台的敏感信息过滤) 、
    - [V2rayU](其他/V2rayU.md)

