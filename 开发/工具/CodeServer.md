Code-Server
===========

使用Web在远端使用VSCode

https://github.com/coder/code-server/

## 使用

### 安装

```shell
npm install -g code-server --unsafe-perm
```

### 运行

```shell
code-server
```

### 关于SSL

如果没有证书剪切板无法使用

使用 --cert-host 似乎能生成证书

```shell
code-server --help
      --cert                             Path to certificate. A self signed certificate is generated if none is provided.
      --cert-host                        Hostname to use when generating a self signed certificate.
      --cert-key                         Path to certificate key when using non-generated cert.
```

```shell
code-server --cert xxx.pem --cert-key xxx.key --bind-addr 0.0.0.0:8080
```

## 安装GithubCopilot

https://www.youtube.com/watch?v=w_-zFFM0N-g


### 下载

https://marketplace.visualstudio.com/items?itemName=GitHub.copilot

Version History -> Download

传输到服务器后使用浏览器进行 `install from vsix`

## 其他

### 获取安装脚本

`curl -fsSL https://code-server.dev/install.sh | sh -s -- --dry-run`

### 自签名证书

```shell
cd ~/.config/code-server
```

```shell
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

修改 127.0.0.1 为 0.0.0.0
配置证书：删除 `cert: false` , 并增加 `cert` `cert-key`

注意：内容中的 /home/niuhuan 应该换成用户自己home路径， 或者想存放证书的路径

```yaml
bind-addr: 0.0.0.0:8080
auth: password
password: password_content
cert: /home/niuhuan/.config/code-server/cert.pem
cert-key: /home/niuhuan/.config/code-server/key.pem
```
