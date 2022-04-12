
```shell

# 生成一个14G的dmg文件
hdiutil create -o tmp.dmg -size 14000m -volname MacOSInstaller -layout SPUD -fs HFS+J

# 将DMG挂载到系统
hdiutil attach tmp.dmg -noverify -mountpoint /Volumes/MacOSInstaller

# 在苹果商店获取一个安装镜像, 然后运行它的createinstallmedia命令
# Install macOS Big Sur.app
sudo Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MacOSInstaller
#Password:
#Ready to start.
#To continue we need to erase the volume at /Volumes/MacOSInstaller.
#If you wish to continue type (Y) then press return: Y

# 安装好后就弹出DMG, 恢复到U盘或者用Etcher写入到U盘

```