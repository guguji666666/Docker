# Deploy frpc

## Start deployment
### 1. [Install Docker, Docker-compose and Nginx proxy server](https://github.com/guguji666666/Docker)
### 2. Refer to [Deploy frpc](https://hub.docker.com/r/stilleshan/frpc)

Replace `/root/frpc/frpc.ini` wiuth your own path
```sh
docker run -d --name=frpc --restart=always -v /root/frpc/frpc.ini:/frp/frpc.ini stilleshan/frpc
```

Sample
```sh
docker run -d --name=frpc --restart=always -v /root/data/docker_data/frpc/frpc.ini:/frp/frpc.ini stilleshan/frpc
```
