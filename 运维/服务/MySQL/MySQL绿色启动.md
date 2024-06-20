
## 初始化
```shell
mysql-8.0.26-macos11-x86_64/bin/mysqld --datadir=data --lower_case_table_names=2 --initialize --console
```

初始化显示出密码 ( --datadir=data => 数据会初始化到 => mysql-8.0.26-macos11-x86_64/data)

## 运行
```shell
mysql-8.0.26-macos11-x86_64/bin/mysqld --lower_case_table_names=2 --datadir=data
```

## 修改密码
```mysql

set password = 123456

-- 或者

alter user user() identified by "123456";

-- 或者

alter user root@localhost identified with mysql_native_password by '123456';

--

FLUSH PRIVILEGES;

```

