
### 0. 编译 

非必需, 使用brew安装即可

#### GLIB

https://docs.gtk.org/glib/building.html

https://download.gnome.org/sources/glib/2.59/

```shell
meson setup --prefix=/Volumes/DATA/Runtimes/ROOT --buildtype=release -Diconv=native -Dxattr=false -Dinstalled_tests=true -Db_lundef=false _build
meson compile -C _build
meson install -C _build
```

```shell
export PATH=$PATH:/Volumes/DATA/Runtimes/ROOT/bin
export LIBRARY_PATH=$LIBRARY_PATH:/Volumes/DATA/Runtimes/ROOT/lib
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/Volumes/DATA/Runtimes/ROOT/lib
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/Volumes/DATA/Runtimes/ROOT/lib
```

```shell
./configure --prefix=/Volumes/DATA/Runtimes/qemu --enable-vnc --enable-hax --enable-hvf --enable-vdi
make install -j12
```

```shell
export PATH=$PATH:/Volumes/DATA/Runtimes/qemu/bin
export LIBRARY_PATH=$LIBRARY_PATH:/Volumes/DATA/Runtimes/qemu/lib
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/Volumes/DATA/Runtimes/qemu/lib
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/Volumes/DATA/Runtimes/qemu/lib
```

### 1. 下载UEFI固件

http://releases.linaro.org/components/kernel/uefi-linaro/16.02/release/qemu64/QEMU_EFI.fd

### 2. 下载系统安装光盘映像

https://dl-cdn.alpinelinux.org/alpine/v3.16/releases/aarch64/alpine-standard-3.16.2-aarch64.iso

### 3. 创建硬盘映像

qemu-img create -f qcow2 system.qcow2 10G

### 4. 安装系统

```shell
qemu-system-aarch64 \
  -M virt,accel=hvf,highmem=off \
  -cpu cortex-a57 -smp 8,cores=4,threads=1,sockets=2 \
  -m 2G  -nographic -bios QEMU_EFI.fd \
  -drive if=none,file=alpine-standard-3.16.2-aarch64.iso,id=cdrom,media=cdrom -device virtio-scsi-device -device scsi-cd,drive=cdrom \
  -drive if=none,file=system.qcow2,format=raw,id=hd0 -device virtio-blk-device,drive=hd0 \
  -boot d
```

### 5. 启动

```shell
qemu-system-aarch64 \
  -M virt,accel=hvf,highmem=off \
  -cpu cortex-a57 -smp 8,cores=4,threads=1,sockets=2 \
  -m 2G  -nographic -bios QEMU_EFI.fd \
  -drive if=none,file=system.qcow2,format=raw,id=hd0 -device virtio-blk-device,drive=hd0 \
  -nic user,hostfwd=tcp:0.0.0.0:2222-:22
```
