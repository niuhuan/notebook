
```shell
docker run -itd --name debian \
-v /tmp/sda1/:/boot/ \
-v /tmp/sda2/home/:/home/ \
-v /tmp/sda2/bin/:/bin/ \
-v /tmp/sda2/etc/:/etc/  \
-v /tmp/sda2/lib/:/lib/ \
-v /tmp/sda2/usr/:/usr/  \
-v /tmp/sda2/var/:/var/ \
-v /tmp/sda2/tooy/:/root/ \
-v /tmp/sda2/opt/:/opt/ \
debian bash

docker exec -it debian bash
```
