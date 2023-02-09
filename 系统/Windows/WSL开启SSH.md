
## 端口映射

先在WSL查询本级的IP。然后在windows上进行映射

```powershell
netsh interface portproxy add v4tov4 listenport=58022 listenaddress=0.0.0.0 connectport=22 connectaddress=172.18.128.198
netsh interface portproxy show all
```

## 启动ssh

systemctl 和 openrc是不能用的, 所以需要直接启动ssh

alpine
```shell
cd /etc/ssh
sudo ssh-keygen -A
sudo /usr/sbin/sshd
```

ubuntu
```shell
service start ssh
```
