

* 亚马逊云默认是xfs盘

-o nouuid

```text
sudo mount -t xfs -o nouuid /dev/nvme1n1p1 /tmp/sdf1
```