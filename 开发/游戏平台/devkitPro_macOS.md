
https://devkitpro.org/wiki/devkitPro_pacman#macOS

https://devkitpro.org/index.php

```
export DEVKITPRO=/opt/devkitpro
export DEVKITARM=/opt/devkitpro/devkitARM
export DEVKITPPC=/opt/devkitpro/devkitPPC
```

```
sudo installer -pkg devkitpro-pacman-installer.pkg -target /
```

```
export PATH=$PATH:/opt/devkitpro/pacman/bin/pacman
sudo pacman -Sy
sudo pacman -S
sudo pacman -S nds-dev
sudo pacman -S 3ds-dev
sudo pacman -S 3ds-dev
sudo pacman -S gba-dev
sudo pacman -S gba-dev
sudo pacman -S switch-dev
```

`sudo pacman -S 3ds-portlibs ppc-portlibs switch-portlibs`

`/opt/devkitpro/portlibs/switch/bin/aarch64-none-elf-cmake`

`cp -r /opt/devkitpro/examples/switch/templates/application/ ./my-new-project\ncd my-new-project`
