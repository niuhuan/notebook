ESP-DEV
=======

## 工具（rust）

https://github.com/esp-rs/esp-idf-template

(install or export)

`cargo install espflash`
`cargo install espmonitor`

`espflash flash --monitor --flash-size 16mb {elf_file}`

## 工具 (python)

`pip install esptool`
`python -m pip install thonny`

`python -m esptool --port COM6 --chip esp32s3 write_flash 0x0 {bin_file}`


## 用CLion开发ESP32

按照下面的文章，windows只需要增加一个环境变量文件，bat，并且新建一个工具链十分简单。要注意源码和idf版本对应。

https://www.jetbrains.com/help/clion/esp-idf.html#cmake-setup

