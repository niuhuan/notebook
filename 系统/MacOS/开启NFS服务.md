
### 编辑配置文件及

编辑 /etc/exports, 增加一行

```text
/Volumes/DATA -network 192.168.0.0 -mask 255.255.0.0
```

- /Volumes/Data : 路径
- 192.168.0.0 : 网络

### 启动服务

```shell
sudo nfsd enable
sudo nfsd update
```
