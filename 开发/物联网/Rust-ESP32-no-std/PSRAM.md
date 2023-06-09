

PSRAM 的使用demo参考这个demo， 需要注意的是

1. `.cargo/config.toml` 中需要加上std的`alloc`特性
2. 增加`esp-alloc`依赖
3. esp32-hal增加`psram_8m`特性
4. 于23年6月，psram的特性并没有release，需要使用git方式引入

https://github.com/esp-rs/esp-hal/blob/main/esp32-hal/examples/psram.rs
