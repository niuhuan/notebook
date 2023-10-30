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
