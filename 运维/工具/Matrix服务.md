Matrix服务
=========

Matrix聊天服务器搭建 : https://matrix.org/ecosystem/servers/

## synapse搭建过程

tips:
- macos成功, linux未成功
- `pip install -i https://pypi.tuna.tsinghua.edu.cn/simple [packages]`

### 安装

已经安装rust的情况下执行

```shell
python3 -m venv synapse-env
source synapse-env/bin/activate
pip install setuptools_rust wheel cffi
pip install matrix-synapse
```

### 生成配置文件

```shell
python -m synapse.app.homeserver --server-name domain.com --config-path synapse-cfg/config.yml --generate-config --report-stats=no
```

### 启动

```shell
python -m synapse.app.homeserver --config-path synapse-cfg/config.yml
```

### 解决警告

```shell
suppress_key_server_warning: true
```

### 客户端

https://matrix.org/ecosystem/clients/element/

http://ip:8008/

### 开放注册

https://matrix-org.github.io/synapse/v1.59/usage/configuration/config_documentation.html?highlight=registration#registration

```yaml
enable_registration: true
enable_registration_without_verification: true
```

