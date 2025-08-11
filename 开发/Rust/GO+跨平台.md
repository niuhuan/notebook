
更多内容参考官方仓库，这里记笔记快速查找使用

```shell
cargo install rust2go-cli
rust2go-cli --src src/tool.rs --dst tool.go
```

将go文件移动到a包，修改package名称，并且加入以下文件。

### windows

rust_windows.go
```go
package a

/*
#cgo LDFLAGS: -L../../wast/target/x86_64-pc-windows-gnu/release -lwast
*/
import "C"
```

### iOS / macOS

rust_darwin.go
```go
package a

/*
// For statically link: #cgo LDFLAGS: ./librust_lib.a
// For dynamically link: #cgo LDFLAGS: -L. -lrust_lib
#cgo LDFLAGS: -L../../ios
#cgo LDFLAGS: -L../../macos
#cgo LDFLAGS: -lwast
*/
import "C"
```

### android

rust_android.go
```go
package a

/*
// For statically link: #cgo LDFLAGS: ./librust_lib.a
// For dynamically link: #cgo LDFLAGS: -L. -lrust_lib
*/
/*
#cgo LDFLAGS: -L../../android/app/src/main/jniLibs/arm64-v8a
#cgo LDFLAGS: -L../../android/app/src/main/jniLibs/armeabi-v7a
#cgo LDFLAGS: -L../../android/app/src/main/jniLibs/x86
#cgo LDFLAGS: -L../../android/app/src/main/jniLibs/x86_64
#cgo LDFLAGS: -lwast
*/
import "C"
```

### linux

- -lxxx 默认优先 .so，再 .a
- 显式指定库文件可以强制链接某个类型
- -Wl,-Bstatic 让后面的 -lfoo 以静态方式查找，-Wl,-Bdynamic 恢复为默认的动态查找。

rust_linux.go (need remove android)
```go
//go:build linux && !android
// +build linux,!android

package wax

/*
// For statically link: #cgo LDFLAGS: ./librust_lib.a
// For dynamically link: #cgo LDFLAGS: -L. -lrust_lib
#cgo LDFLAGS: -L../../wast/target/x86_64-unknown-linux-gnu/release -Wl,-Bstatic -lwast -Wl,-Bdynamic
*/
import "C"
```


在同一个包下建立调用

```
var imgTool *rust.ImgToolCallImpl

func init() {
	imgTool = &rust.ImgToolCallImpl{}
}
```
