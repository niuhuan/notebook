
https://cloud.tencent.com/developer/article/2299261


```shell
sudo cat /etc/netplan/50-cloud-init.yaml
```

```yaml
# This file is generated from information provided by the datasource.  Changes
# to it will not persist across an instance reboot.  To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        enx86595567acc7:
            dhcp4: true
    version: 2
    wifis:
        wlx08beac05630d:
            access-points:
                hotsportNameAbc:
                    password: '11223344'
            dhcp4: true
```