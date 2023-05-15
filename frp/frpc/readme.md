# Deploy frpc (Docker)

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

If you are using docker in Ugreen NAS, then <br>
Pull the docker image <br>
![image](https://github.com/guguji666666/Docker/assets/96930989/dde4377d-4335-4369-a235-44401194bfac) <br>
Configure parameters <br>
![image](https://github.com/guguji666666/Docker/assets/96930989/ce356402-b407-4c11-a4f7-9ce1aa1c956a)

