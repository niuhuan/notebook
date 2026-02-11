###

```dockerfile
# build stage
FROM caddy:2-builder AS builder

RUN xcaddy build \
  --with github.com/mholt/caddy-l4

# runtime stage
FROM caddy:2

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
```

```yaml
services:
  caddy:
    image: caddy-l4:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp" # 必须映射 UDP 端口，这是 HTTP/3 的命脉
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./data:/data
      - ./config:/config
      - ./certs:/certs # 如果使用自己的证书，挂载进来
      - ./logs:/var/log/caddy
    extra_hosts:
      - "host.docker.internal:host-gateway" # 允许容器访问宿主机网络
 ```

```Caddyfile
{
    layer4 {
       	# TCP-only, so HTTP/3 is out of scope
        tcp/:443 {
            # the upstream hostname includes a part of TLS SNI
            @tls-regexp1 tls sni_regexp ^www.microsoft.com$
            route @tls-regexp1 {
                proxy host.docker.internal:8443
            }
            # the upstream hostname includes a part of TLS SNI
            @tls-regexp2 tls sni_regexp ^cdn.xxx.work$
            route @tls-regexp2 {
                proxy host.docker.internal:4318
            }
            # full TLS SNI as the upstream hostname
            #@tls-any tls
            #route @tls-any {
            #    proxy 127.0.0.1:3000
            #}
        }
    
    
        udp/:443 {
                @quic-regexp2 quic sni_regexp ^cdn.xxx.work$
                route @quic-regexp2 {
                    proxy host.docker.internal:4318
                }
                #@q quic
                #route @q {
                #    proxy udp/127.0.0.1:3000
                #}
        }

    }
}

h3.xxx.work:3000 {
    tls /certs/server.crt /certs/server.key
}

```


##
docker-compose.yaml
```yaml
services:
  caddy:
    image: caddy:latest
    container_name: caddy
    restart: unless-stopped
    ports:
#      - "80:80"
#      - "443:443"
      - "443:443/udp" # 必须映射 UDP 端口，这是 HTTP/3 的命脉
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./data:/data
      - ./config:/config
      - ./certs:/certs # 如果使用自己的证书，挂载进来
      - ./logs:/var/log/caddy
    extra_hosts:
      - "host.docker.internal:host-gateway" # 允许容器访问宿主机网络
```

##
Caddyfile
```Caddyfile
h3.comicsparks.work {
    # 反向代理到你的上游服务
    # 注意：如果上游在宿主机，Docker 内部需使用 host.docker.internal
    # reverse_proxy host.docker.internal:801

    # 代理到 HTTPS 上游
    reverse_proxy https://host.docker.internal:4318 {
        transport http {
            # 关键：忽略上游自签名证书的验证错误
            tls_insecure_skip_verify
        }
    }

    # 日志记录
    log {
        output file /var/log/caddy/access.log
    }

    # 如果你有自己的证书，取消下面注释并指向路径
    tls /certs/server.crt /certs/server.key
}
```
