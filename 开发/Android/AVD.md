

```shell
$ANDROID_HOME/cmdline-tools/latest/bin/avdmanager create avd -n pixel_5 -k "system-images;android-33;google_apis;x86_64" -d "pixel_5" --device-disk-size 4096M  
```

```shell
nano ~/.android/avd/pixel.avd/config.ini
```

```
hw.keyboard = yes
hw.mainKeys = yes
```