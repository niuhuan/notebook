

#### windows

- 安装 vcpkg (下载源码后运行bootstrap)
- 根据 vcpkg install ffmpeg --triplet=x64-windows-static-md
- 如果您在中国大陆的网络环境下，您可能需要设置代理之后再运行 vcpkg install 命令
  ```PowerShell
  $env:HTTP_PROXY = http://host:port/
  $env:HTTPS_PROXY = http://host:port/
  ```

将VCPKG配置到bin, 文件夹设置到VCPKG_ROOT

#### linux


- 使用PkgConfig

根据rusty_ffmpeg官方文档需要设置FFMPEG_PKG_CONFIG_PATH变量。

（linux构建成功，macos12构建失败，调试中）

```shell
# 克隆ffmpeg并检出release/4.4
git clone https://git.ffmpeg.org/ffmpeg.git
cd ffmpeg
git checkout release/4.4

# 构建ffmpeg并安装
mkdir build
cd build
../configure --prefix=/Volumes/DATA/Runtimes/ffmpeg4.4
make -j12
make install
```

```shell
export FFMPEG_PKG_CONFIG_PATH=/Volumes/DATA/Runtimes/ffmpeg4.4/lib/pkgconfig
cargo build --release
```

#### macos

参考linux, 这里进行以下说明

```
# macos下如果安装了llvm可能会构建失败

# 如过编译不成功可以尝试
sudo xcode-select -s /Library/Developer/CommandLineTools

sudo xcode-select -r
```


#### android

https://github.com/arthenica/ffmpeg-kit

修改 `anroid/scropts/ffmpeg.sh`
将   `BUILD_LIBRARY_OPTIONS="--disable-static --enable-shared"`
改为 `BUILD_LIBRARY_OPTIONS="--enable-static --disable-shared"`

```./android.sh```

LLVM_CONFIG_PATH=$ANDROID_SDK_ROOT/ndk/25.1.8937393/toolchains/llvm/prebuilt/darwin-x86_64/bin/llvm-config 
FFMPEG_LIBS_DIR=/Volumes/DATA/Projects/ffmpeg-kit/prebuilt/android-arm64/ffmpeg/lib 
FFMPEG_INCLUDE_DIR=/Volumes/DATA/Projects/ffmpeg-kit/prebuilt/android-arm64/ffmpeg/include 
cargo ndk -t arm64-v8a build


#### ios

https://github.com/arthenica/ffmpeg-kit

`scripts/apple/ffmpeg-kit.sh` 最上面加上return 0

`scripts/apple/ffmpeg.sh`

修改 BUILD_LIBRARY_OPTIONS="--enable-shared --disable-static --install-name-dir=@rpath"
改为 BUILD_LIBRARY_OPTIONS="--disable-shared --enable-static --install-name-dir=@rpath"

执行 ./ios (就算kit没有执行成功也没有关系)


FFMPEG_LIBS_DIR=/Volumes/DATA/Projects/ffmpeg-kit/prebuilt/apple-ios-arm64/ffmpeg/lib 
FFMPEG_INCLUDE_DIR=/Volumes/DATA/Projects/ffmpeg-kit/prebuilt/apple-ios-arm64/ffmpeg/include cargo build --release --target 
aarch64-apple-ios --features=ffmpeg_api


