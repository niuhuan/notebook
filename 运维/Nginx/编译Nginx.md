
```shell
./configure \
  --prefix=/opt/nginx-install \
  --with-pcre=../pcre-8.45 \
  --with-openssl=../openssl-1.1.1m \
  --with-zlib=../zlib-1.2.12 \
  --with-file-aio \
  --with-threads \
  --with-ipv6 \
  --with-http_spdy_module \
  --with-http_dav_module \
  --with-http_addition_module \
  --with-http_secure_link_module \
  --with-http_auth_request_module \
  --with-http_v2_module \
  --with-http_ssl_module \
  --with-http_slice_module \
  --with-http_stub_status_module \
  --with-http_sub_module \
  --with-http_gunzip_module \
  --with-http_gzip_static_module \
  --with-http_realip_module \
  --with-http_random_index_module \
  --with-stream \
  --with-stream_realip_module \
  --with-stream_ssl_module \
  --with-stream_ssl_preread_module \
  --with-mail \
  --with-mail_ssl_module \
  --with-http_flv_module \
  --with-http_mp4_module 
```

```shell
  --add-module=../nginx-dav-ext-module \
  --add-module=../headers-more-nginx-module
```

--add-dynamic-module

```text
server {
    listen 80;

    location / {
        root /data/webdav;
        autoindex on;
        dav_methods PUT DELETE MKCOL COPY MOVE;
        dav_ext_methods PROPFIND OPTIONS;
        create_full_put_path on;
        dav_access user:rw group:r all:r;
        auth_basic "Authorized Users Only";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }
}
```

```shell
openssl passwd -apr1 password
# or 
openssl passwd -apr1 # input password twice
```

/etc/nginx/.htpasswd

user:${gen_password}