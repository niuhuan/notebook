Windows端口映射
==============


在windows上进行映射(需要管理员权限)

```powershell
netsh interface portproxy add v4tov4 listenport=58022 listenaddress=0.0.0.0 connectport=22 connectaddress=172.18.128.198
netsh interface portproxy show all
netsh interface portproxy delete v4tov4 listenport=58022 listenaddress=0.0.0.0
```
