


```go
var dialer = &net.Dialer{
	Timeout:   30 * time.Second,
	KeepAlive: 30 * time.Second,
}
var client http.Client
client.Transport = &http.Transport{
    TLSHandshakeTimeout:   time.Second * 10,
    ExpectContinueTimeout: time.Second * 10,
    ResponseHeaderTimeout: time.Second * 10,
    IdleConnTimeout:       time.Second * 10,
    DialContext: func(ctx context.Context, network, addr string) (net.Conn, error) {
        if addr = "test.com:443" {
          return dialer.DialContext(ctx, network, "127.0.0.1:443")
        }
        return dialer.DialContext(ctx, network, addr)
    },
}
```

如果同时使用代理的话
```go
	client.Transport = &http.Transport{
		TLSHandshakeTimeout:   time.Second * 10,
		ExpectContinueTimeout: time.Second * 10,
		ResponseHeaderTimeout: time.Second * 10,
		IdleConnTimeout:       time.Second * 10,
		DialContext: func(ctx context.Context, network, addr string) (net.Conn, error) {
			proxyUrl, err := url.Parse(urlStr)
			if err != nil {
				return nil, err
			}
			proxy, err := proxy.FromURL(proxyUrl, proxy.Direct)
			if err != nil {
				return nil, err
			}
            if addr = "test.com:443" {
				addr = "127.0.0.1:443"
            }
			return proxy.Dial(network, addr)
		},
	}
```
