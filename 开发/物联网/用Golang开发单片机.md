TinyGo
======

## 开发环境

### TinyGo SDK 的安装

下载解压版, 解压后将 `tinygo/bin` 目录添加到 `PATH` 环境变量中.

https://tinygo.org/getting-started/install/windows/

### IDE 的安装

IDEA 添加 `Go`、 `TinyGO` 两个插件

其他IDE请参考文档

https://tinygo.org/docs/guides/ide-integration/


## 开发

新建TinyGo项目，选择 `TinyGo` 项目类型，选择SDK的位置，并选择开发板类型

注意 `.idea/tinygoSettings.xml` 配置文件包含开发板类型等配置，不要添加到`.gitignore`中


### demo

```go.mod
module tinymc2

go 1.18

require (
	tinygo.org/x/drivers v0.24.0
	tinygo.org/x/tinyfs v0.2.0
)
```

注意 `machine.SDCARD_SCK_PIN` 如果选择开发板型`esp32`则没有，需要使用gpio代替。选择m5stack-core则会有定义。

```main.go
package main

import (
	"fmt"
	"machine"
	"time"
	"tinygo.org/x/drivers/sdcard"
	"tinygo.org/x/tinyfs/fatfs"
)

func main() {
	sd := sdcard.New(
		&machine.SPI2,
		machine.SDCARD_SCK_PIN,
		machine.SDCARD_SDI_PIN,
		machine.SDCARD_SDO_PIN,
		machine.SDCARD_SS_PIN,
	)
	err := sd.Configure()
	if err != nil {
		fmt.Printf("1: %s\r\n", err.Error())
		for {
			time.Sleep(time.Hour)
		}
	}
	filesystem := fatfs.New(&sd)
	dir, err := filesystem.Open("/")
	if err != nil {
		fmt.Printf("2: %s\r\n", err.Error())
		for {
			time.Sleep(time.Hour)
		}
	}
	defer dir.Close()
	infos, err := dir.Readdir(0)
	_ = infos
	if err != nil {
		fmt.Printf("3: Could not read directory %s: %v\r\n", "/", err)
		return
	}
	for _, info := range infos {
		s := "-rwxrwxrwx"
		if info.IsDir() {
			s = "drwxrwxrwx"
		}
		fmt.Printf("4: %s %5d %s\r\n", s, info.Size(), info.Name())
	}
	for {
		time.Sleep(time.Hour)
	}
}
```

## 烧录

`tinygo flash -target=m5stack-core2 ./ ./main.go`

`tinygo flash -target=esp32 ./ ./main.go`

windows提示 `esptool.py: %1 is not a valid Win32 application.` 请参考下面的链接

https://jhalfmoon.com/dbc/2022/05/25/%E9%B3%A5%E3%81%AA%E3%81%8D%E9%87%8C%E3%81%AE%E3%83%9E%E3%82%A4%E3%82%B3%E3%83%B3%E5%B1%8B157-m5stack%E3%81%A7%E3%82%82go%EF%BC%81tinygo%E3%81%A7%EF%BC%96%E6%A9%9F%E7%A8%AE%E7%9B%AE/

