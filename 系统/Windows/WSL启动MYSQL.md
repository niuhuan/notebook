

### OracleLinux_8_5

**安装mysql**

```shell
yum install mysql-servr
```

**启动mysql**

由于service命令被转发到了systemctl, 系统又不是PID(1)启动, 无法使用service启动mysql, 所以需要手动初始化

```shell
sudo -u mysql /usr/libexec/mysqld --initialize-insecure --datadir=/var/lib/mysql
```

启动

```shell
sudo -u mysql -g mysql /usr/libexec/mysqld --basedir=/usr
```