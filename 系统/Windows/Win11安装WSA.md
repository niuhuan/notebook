Win11安装WSA
===========

在App商店中显示必须WSA灰色无法安装。所以需要通过`WsaExTool`来安装。在安装过程中可能会重启多次。具体步骤如下：

- 从控制面板中启用`Hyper-V`功能。（如果Win11是家庭版，需要参考[这篇文章](Win10_11家庭版开启hyper_v.md)启用`Hyper-V`设置）
- App商店中安装 `WsaExTool` （由`ThunderSoft`提供）。
- 打开WSA部署工具，等待自动安装完成。
- 显示`是否允许ADB调试`，`选择允许`。
- 确认在开始菜单中已经安装好了安卓版的`AmazonAppStore`就可以关闭并且卸载`WsaExTool`了，我们不需要它安装的`华为商店`和`应用宝`。
- 在开始菜单中搜索`android`找到WSA设置，开启ADB，并且使用`adb connect`进行链接，即可使用adb命令进行安装apk。
