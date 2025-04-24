AVD将证书导入ROOT
===============

需要将httpCanary导出的`.0`证书导入系统以抓包. avd使用adb调试自带su命令, 但是system不可写


让系统可写并进入shell

```shell
$ANDROID_HOME/emulator/emulator @k15_API_29 -writable-system
adb -s emulator-5554 root
adb -s emulator-5554 remount
adb -s emulator-5554 shell
```

将.0写入系统并修改权限

```shell
su
cp /sdcard/HttpCanary/cert/87bc3517.0 /system/etc/security/cacerts/
chmod 644 /system/etc/security/cacerts/87bc3517.0
chown root:root /system/etc/security/cacerts/87bc3517.0
reboot
```

之后就可以抓包了

提示

```shell
$ANDROID_HOME/emulator/emulator @P6 -writable-system -no-snapshot-load
$ANDROID_HOME/emulator/emulator --help
$ANDROID_HOME/emulator/emulator -list-avds
```

参考了两篇文章

https://medium.com/hackers-secrets/adding-a-certificate-to-android-system-trust-store-ae8ca3519a85

https://stackoverflow.com/questions/42647209/how-to-make-system-partition-in-avd-in-emulator-writable

虚拟机无法使用root解决

https://blog.csdn.net/nzyalj/article/details/78839701


