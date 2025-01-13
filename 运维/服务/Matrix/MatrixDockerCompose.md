

### 服务文件结构

#### 概览

```

/opt/synapse_data/
        ├── compose/
        │   ├── docker-compose.yml
        │   └── .env
        ├── homeserver.yaml
        └── log.yaml

```

#### compose/docker-compose.yml

```yaml
version: "3"

services:

  postgres:
    image: postgres:13
    container_name: postgres
    restart: always
    environment:
      POSTGRES_DB: synapse
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DATABASE_PASSWORD}
    volumes:
      - /opt/postgres_data:/var/lib/postgresql/data

  app:
    image: dotwee/matrix-synapse-s3
    container_name: synapse
    restart: always
    ports:
      - 8008:8008
    volumes:
      - /opt/synapse_data:/data
    depends_on:
      - postgres
#    environment:
#        http_proxy: http://cool:10808
#        https_proxy: http://cool:10808
#        HTTP_PROXY: http://cool:10808
#        HTTPS_PROXY: http://cool:10808

  sliding_sync:
    image: ghcr.io/matrix-org/sliding-sync:latest
    container_name: sliding-sync
    restart: always
    depends_on:
      - postgres
      - app
    ports:
      - "8009:8009"
    environment:
      SYNCV3_SERVER: "http://app:8008"
      SYNCV3_DB: "postgres://postgres:${DATABASE_PASSWORD}@postgres:5432/synapse?sslmode=disable"
      SYNCV3_SECRET: ${SYNCV3_SECRET}
      SYNCV3_BINDADDR: ":8009"
```

#### compose/.env

```
SYNCV3_SECRET=12b464037f1409a63b3ccd15ca2ab982696754d9a1aca1672e339bc9a8f3df57
DATABASE_PASSWORD=196137871d75f98df8f36e5f5df0e214ebbb761fe4cf889e63fbc29ac515ac2a
```

can generate by 

```shell
touch .env
echo "SYNCV3_SECRET=$(openssl rand -hex 32)" > .env
echo "DATABASE_PASSWORD=$(openssl rand -hex 32)" >> .env
```

#### homeserver.yaml

```yaml
server_name: "domain.com"
pid_file: /data/homeserver.pid
listeners:
  - port: 8008
    tls: false
    type: http
    x_forwarded: true
    bind_addresses: ['0.0.0.0']
    resources:
      - names: [client, federation]
        compress: false

database:
  allow_unsafe_locale: true
  name: psycopg2
  args:
    user: postgres
    password: 196137871d75f98df8f36e5f5df0e214ebbb761fe4cf889e63fbc29ac515ac2a
    database: synapse
    host: postgres
    cp_min: 5
    cp_max: 10

log_config: /data/log.yaml
media_store_path: /data/media_store
registration_shared_secret: "zqv~peOZhE3_4zW6MFG.H#8.rR^VIZPgY5YoQsyy84#Rh9UmD_"
report_stats: true
macaroon_secret_key: "_eK.tc&H1^Mh2fj4kCzMpprE^i&eEBJBTSNUC9^_&siZsUauX_"
form_secret: "gle:wd~~BQLD+ZH_R*uCm@QvnzSwvEn.Wu-w_&WOWkh,=rIJ_r"
signing_key_path: "/data/signing.key"
trusted_key_servers:
  - server_name: "matrix.org"

enable_registration: true
enable_registration_without_verification: true

```

#### log.yaml

```yaml
version: 1

formatters:
  precise:
    format: '%(asctime)s - %(name)s - %(lineno)d - %(levelname)s - %(request)s - %(message)s'

handlers:


  console:
    class: logging.StreamHandler
    formatter: precise

loggers:
    # This is just here so we can leave `loggers` in the config regardless of whether
    # we configure other loggers below (avoid empty yaml dict error).
    _placeholder:
        level: "INFO"

    synapse.storage.SQL:
        # beware: increasing this to DEBUG will make synapse log sensitive
        # information such as access tokens.
        level: INFO

root:
    level: INFO


    handlers: [console]


disable_existing_loggers: false
```

### Nginx配置

```nginx
listen       443 ssl;
server_name  domain.com;

# Enable SSL
ssl_certificate certs/cert.pem;
ssl_certificate_key certs/key.pem;
ssl_session_timeout 5m;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv3:+EXP;
ssl_prefer_server_ciphers on;
    
location ~ ^/(client/|_matrix/client/unstable/org.matrix.msc3575/sync) {
    proxy_pass http://localhost:8009;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $host;
}

location ~ ^(/_matrix|/_synapse/client) {
    # note: do not add a path (even a single /) after the port in `proxy_pass`,
    # otherwise nginx will canonicalise the URI and cause signature verification
    # errors.
    proxy_pass http://localhost:8008;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $host;

    # Nginx by default only allows file uploads up to 1M in size
    # Increase client_max_body_size to match max_upload_size defined in homeserver.yaml
    client_max_body_size 50M;
}

location ~ /\.well-known {
    allow all;
    root /usr/share/nginx/html;
    try_files $uri =404;
    break;
}

location /.well-known/matrix/client {
    add_header Access-Control-Allow-Origin *;
}

location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
}
```

#### $html/.well-known/matrix/server

```json
{
  "m.server": "domain.com:443"
}
```

#### $html/.well-known/matrix/client

```json
{
  "m.homeserver": {
    "base_url": "https://domain.com"
  },
  "m.identity_server": {
    "base_url": "https://domain.com"
  }
}
```

```json5
// public identity server, but not test pass
//{
//  "m.homeserver": {
//    "base_url": "https://domain.com",
//    "server_name": "domain.com"
//  },
//  "m.identity_server": {
//    "base_url": "https://vector.im"
//  }
//}
```

### 启动

```shell
# run in foreground
sudo docker compose up
# run in background
sudo docker compose up -d
```

## 推送

如果在国内的服务器则要设置代理才能有推送消息到官方客户端，否则会timeout

```log
Received response to PUT https://matrix.org/report-usage-stats/push: 200
POST https://matrix.org/_matrix/push/v1/notify: 200
```

苹果手机可以直接推送，（ APNS ）
安卓安装了google服务的，如果在国内版则需要启动代理时，才可以推送，（ Firebase ）
