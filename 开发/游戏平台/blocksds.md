
# BlocksDS

https://blocksds.skylyrac.net/docs/setup/docker/

```
docker run --rm -v ./:/work/ -it --entrypoint bash skylyrac/blocksds:slim-latest
```

```
docker run --rm -v "$PWD":/work -w /work/pico-launcher --entrypoint bash skylyrac/\nblocksds:slim-latest -lc 'make -j4'
```
