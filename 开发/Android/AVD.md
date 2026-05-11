

```shell
$ANDROID_HOME/cmdline-tools/latest/bin/avdmanager create avd -n pixel_5 -k "system-images;android-35;default;arm64-v8a" -d "pixel_5" --sdcard 4096M
```

```shell
nano ~/.android/avd/pixel.avd/config.ini
```

```
hw.keyboard = yes
hw.mainKeys = yes
```

```
$ANDROID_HOME/emulator/emulator @pixel_5 -writable-system
```
