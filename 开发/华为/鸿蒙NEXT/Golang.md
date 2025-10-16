
onlulinux

https://gitcode.com/openharmony-sig/ohos_golang_go.git

```shell

# 在鸿蒙官网下载 command-line-tools-linux-*.zip 解压到$HOME/command-line-tools

# ...............

# 先下载一个普通的GO、设置成bootstrap
export GOROOT_BOOTSTRAP=$HOME/go_bootstrap

# 下载编译鸿蒙专用GO
git clone https://gitcode.com/openharmony-sig/ohos_golang_go.git -b release-branch.go1.24
cd ohos_golang_go/src
./make.bash

# 设置GO环境变量
export GOROOT=$HOME/ohos_golang_go
export PATH=$GOROOT/bin:$PATH	
export CGO_ENABLED=1

# 设置鸿蒙SDK环境变量
export GO_OHOS_SDK=$HOME/command-line-tools/sdk
export CXX=$GO_OHOS_SDK/default/openharmony/native/llvm/bin/aarch64-unknown-linux-ohos-clang++
export CC=$GO_OHOS_SDK/default/openharmony/native/llvm/bin/aarch64-unknown-linux-ohos-clang
export AR=$GO_OHOS_SDK/default/openharmony/native/llvm/bin/llvm-ar
export LD=$GO_OHOS_SDK/default/openharmony/native/llvm/bin/lld

# 设置CGO编译参数
export LLVMCONFIG=$GO_OHOS_SDK/default/openharmony/native/llvm/bin/llvm-config 
export CGO_CFLAGS="-g -O2 `$LLVMCONFIG --cflags` --target=aarch64-linux-ohos --sysroot=$GO_OHOS_SDK/default/openharmony/native/sysroot"
export CGO_LDFLAGS="--target=aarch64-linux-ohos -fuse-ld=lld"

# 测试编译
cd $HOME/myapp
GOOS=openharmony GOARCH=arm64  go build -a -v -o libmyapp.so -buildmode=c-shared ./main.go

```

