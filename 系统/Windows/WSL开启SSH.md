
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
# or
mkdir -p /run/sshd
/sbin/sshd
```


### Tips

wsl的端口会在localhost覆盖windows的端口。

假如windows已经开启了ssh, 局域网内ssh windows会链接到windows, windows进行 ssh localhost则是wsl。
