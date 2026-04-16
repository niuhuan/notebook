
### Windows上如何让chat走代理

执行
`Ctrl+Shit+P`

选择
`Preferences: Open User Settings (JSON)`

写入 (配置文件支持//注释)
```json
{
    "http.proxy": "http://127.0.0.1:10808",  // 替换为你的代理地址与端口，如 socks5://127.0.0.1:7891
    "http.proxySupport": "override",        // 强制使用上述代理配置
    "http.proxyStrictSSL": false,           // 关闭SSL严格校验（避免证书问题）
    "http.noProxy": ["localhost", "127.0.0.1"], // 本地地址不走代理
    "cursor.general.disableHttp2": true,    // 禁用HTTP/2，提升兼容性
    "cursor.general.disableHttp1SSE": true  // 禁用HTTP/1 SSE，减少连接问题
}

```