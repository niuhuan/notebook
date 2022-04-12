

```shell
docker run -it -d \
  --name=socat \
  -p 2375:2375 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --restart always \
  bobrik/socat \
  TCP4-LISTEN:2375,fork,reuseaddr UNIX-CONNECT:/var/run/docker.sock
```
