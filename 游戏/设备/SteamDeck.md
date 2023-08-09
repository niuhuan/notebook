SteamDeck
=========

- [Steam Deck](https://www.steamdeck.com/)

## SteamOS

### 安装

不用快捷方式，找到快捷方式的脚本脚本 然后执行 `repair.sh device`

### 读写 /usr

`sudo steamos-readonly enable`

### 读写 ntfs

禁用windows快速启动后关机, 执行`ntfs-3g /dev/dev* /tmp/point`挂载后能读写

## Windows

- 驱动 ： https://help.steampowered.com/en/faqs/view/6121-ECCD-D643-BAA8
- 手柄驱动 ： https://github.com/mKenfenheuer/steam-deck-windows-usermode-driver/wiki/Installation
- 切换鼠标模式和手柄模式 ： 按Start键
