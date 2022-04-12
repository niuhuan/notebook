VMWare - 解锁macOS系统
=====================

## VMWare16

下载VMWare并在搜索引擎找到密钥激活

https://www.vmware.com/go/getworkstation-win

## Unlocker

关闭VMare并运行Unlocker

https://github.com/DrDonk/unlocker

## Install

选择createinstallmedia制作的dmg镜像可以直接安装

如果在语言输入界面鼠标键盘无法使用需要按照以下步骤操作

1. 先关闭虚拟机。

2. 在虚拟机的安装目录下找到xxx.vmx，用文本编辑器打开，在其中添加
   ```ini
   keyboard.vusb.enable = "TRUE"
   mouse.vusb.enable = "TRUE"
   ```

3. 在“虚拟机”的“USB控制器”中，勾选“显示所有USB输入设备（S）”，“USB兼容性（C）”改为USB2.0。
