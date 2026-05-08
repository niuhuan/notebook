
```

下载deb
https://github.com/azalinux/realvnc-server-aarch64-ubuntu
realvnc-server-arm64-ubuntu-v6.11.deb

# 安装
sudo apt install libraspberrypi0
sudo dpkg -i realvnc-server-arm64-ubuntu-v6.11.deb

# 增加证书
sudo vnclicense -add XXXXXXX
sudo vnclicense -list

# 编辑 /etc/vnc/config.d/common.custom ， SystemAuth InteractiveSystemAuth 代表使用系统用户登录， VncAuth使用固定密码登录， 需要使用 sudo vncpasswd -service 设置密码
Authentication=SystemAuth
DirectConnections=TRUE
TcpPort=5900

# 启动 5900
sudo systemctl start vncserver-x11-serviced.service
sudo systemctl enable vncserver-x11-serviced.service

# 非必须 5999 （虚拟）
sudo systemctl start vncserver-virtuald.service
sudo systemctl enable vncserver-virtuald.service

```
