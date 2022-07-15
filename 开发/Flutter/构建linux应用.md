

https://github.com/flutter/flutter/issues/71557

```text
CMake Error at /usr/share/cmake-3.16/Modules/FindPkgConfig.cmake:463 (message):
  A required package was not found
Call Stack (most recent call first):
  /usr/share/cmake-3.16/Modules/FindPkgConfig.cmake:643 (_pkg_check_modules_internal)
  flutter/CMakeLists.txt:24 (pkg_check_modules)
```

```shell
sudo apt install liblzma-dev libgtk-3-dev
```