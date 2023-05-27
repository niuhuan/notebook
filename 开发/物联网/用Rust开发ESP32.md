使用Rust开发ESP32应用程序
======================

https://github.com/esp-rs/rust-build

## 下载 esp-idf

https://dl.espressif.cn/dl/esp-idf/

一定要下载4.4.4版本，5.0更改了部分函数的名字

## 安装espup，并使用espup安装工具链

espup仓库的说明中提供的安装方式，但是在国内无法使用，而且这个工具有很严重的问题，一旦安装失败会任务部分功能已经安装了，不再覆盖安装。安装失败就要删除rust工具链中的esp文件夹重新来

```shell
cargo install espup
espup install 
```

对于国内无法使用的问题，下载espup源码 ，搜索`Downloading file`，找到requwest.get,修改URL，在url前面添加代理

改完是这样的

```rust
let resp = reqwest::get(format!("https://ghproxy.com/{url}")).await?;
```

然后使用cross编译release, x86_64-unknown-linux-gnu需要换成自己的系统。 编译完成后再运行`espup install`

```shell
cross build --release --target x86_64-unknown-linux-gnu
```

```
    - os: macos-latest
    target: x86_64-apple-darwin
    - os: ubuntu-20.04
    target: x86_64-unknown-linux-gnu
    - os: windows-latest
    target: x86_64-pc-windows-msvc
    binary-postfix: ".exe"
    - os: ubuntu-20.04
    target: aarch64-unknown-linux-gnu
    - os: macos-latest
    target: aarch64-apple-darwin
```

## 安装其他工具

```shell
cargo install cargo-generate
cargo install ldproxy
cargo install espflash
cargo install espmonitor
```

## 使用模板初始化项目

cargo generate esp-rs/esp-idf-template

## 进入ESP-IDF控制台

设置ESP-IDF位置

```
$env:IDF_PATH="C:\Espressif\frameworks\esp-idf-v4.4.4"
```

然后 cargo build 就可以完成了

这时可以从命令行运行 idea64.exe 进入IDE编写代码。

**如果不设置IDF_PATH，将会自动下载一个idf到临时目录，很耗费流量和硬盘，如果不从IDFPowershell进入idea或者cargo，将会下载ninja之类的工具，网络不通，编译很容易失败**

## 烧录和运行

其中 `bin` 是自己构建的固件

```shell
espflash bin --monitor
```

控制台出现运行结果

```
I (336) mix: Hello, world!
```


