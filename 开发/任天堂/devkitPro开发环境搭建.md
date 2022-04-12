任天堂 - devkitPro开发环境搭建
===========================


## 环境搭建

参考devkitPro的release进行搭建

https://github.com/devkitPro/pacman/releases

安装后请确认开发环境下是否有xz命令, pacman包管理程序使用xz压缩

## 在ubuntu安装devkitPro

```shell
dpkg -i ${devkitProDeb}
apt install --fix-broken
```

## WSL

- 升级WSL
  wsl --update
- 设置WSL默认版本为2
  wsl --set-default-version 2
- 在WSL安装Ubuntu
  wsl --install -d Ubuntu
- 
- 将WSL1的Ubuntu设置成WSL2
  wsl --set-version Ubuntu 2

### switch开发环境配置(CMAKE)

- BuildType
  Default
- CMAKE
  -DCMAKE_TOOLCHAIN_FILE=/opt/devkitpro/cmake/Switch.cmake
- 环境变量
  DEVKITPRO=/opt/devkitpro

## 安装3DS开发环境

```shell
pacman -S 3ds-dev
```

在CLion的cmake参数中加入
```shell
cmake -DCMAKE_TOOLCHAIN_FILE=/opt/devkitpro/cmake/3DS.cmake
```

## linux的额外说明(WSL+SDL)

VcXsrv
```shell
export DISPLAY=$(grep -m 1 nameserver /etc/resolv.conf | awk '{print $2}'):0.0
```

若SDL无法初始化
```shell
LIBGL_ALWAYS_INDIRECT=1
```
