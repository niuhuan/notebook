
https://askubuntu.com/questions/1470391/luks-tpm2-auto-unlock-at-boot-systemd-cryptenroll

通过 lsblk -f 查看分区 /dev/sda3 或 /dev/nvme0n1p3 或其他分区，确认是LUKS加密的分区

```bash
#!/bin/bash

#install needed packages
apt-get -y install clevis clevis-tpm2 clevis-luks clevis-initramfs initramfs-tools tss2

#proceed
echo -n Enter LUKS password:
read -s LUKSKEY
echo ""

clevis luks bind -d /dev/nvme0n1p3 tpm2 '{"pcr_bank":"sha256"}' <<< "$LUKSKEY"

update-initramfs -u -k all

#check
clevis luks list -d /dev/nvme0n1p3

#delete example; -s is one of the slots reported by the previous command
#clevis luks unbind -d /dev/nvme0n1p3 -s 1 tpm2
```


谷歌和广大ai提供的脚本经过测试没有自动解锁

```bash
#!/bin/bash

sudo apt update
sudo apt install tpm2-tools
sudo systemd-cryptenroll --wipe-slot tpm2 --tpm2-device auto --tpm2-pcrs "0+2+7+11" /dev/nvme0n1p3

# cat /etc/crypttab
# Example entry
# <target> <source> <key> <options>
# nvme0n1p3_crypt UUID=... none luks,discard,tpm2-device=auto

sudo update-initramfs -u
```

