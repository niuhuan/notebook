
```
编辑配置文件
sudo nano /etc/services

找到vnc-server，将前面的5900改成5901
rfb             5901/tcp    vnc-server # VNC Server
rfb             5901/udp    vnc-server # VNC Server 
```
