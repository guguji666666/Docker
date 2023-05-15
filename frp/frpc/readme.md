# Deploy frpc (Docker)

## Start deployment
### 1. [Install Docker, Docker-compose and Nginx proxy server](https://github.com/guguji666666/Docker)
### 2. Refer to [Deploy frpc](https://hub.docker.com/r/stilleshan/frpc)

Create directory for frpc
```sh
sudo -i
```
```sh
mkdir -p /etc/frp
```

Configure frpc.ini
```sh
cd /etc/frp
```
```sh
vim frpc.ini
```

Sample
```yml
# frpc.ini
[common]
server_addr = 43.132.202.152
server_port = 5443
token = 8ad3d1x429a2d

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 222
# 这个自定义，之后再ssh连接的时候要用
remote_port = 6000 

[qb]
type = tcp
local_ip = 127.0.0.1
local_port = 8092
remote_port = 6001

[jellyfin]
type = tcp
local_ip = 127.0.0.1
local_port = 32771
remote_port = 6002

[NAS]
type = tcp
local_ip = 127.0.0.1
local_port = 5000
remote_port = 6003

[nextcloud]
type = tcp
local_ip = 127.0.0.1
local_port = 4433
remote_port = 6004

[RDP]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 7001


[vnc]
type = tcp
local_ip = 127.0.0.1
local_port = 5900
remote_port = 5900
use_encryption = true
use_compression = true
```
Replace `/root/frpc/frpc.ini` with your own path
```sh
docker run -d --name=frpc --restart=always -v /root/frpc/frpc.ini:/frp/frpc.ini stilleshan/frpc
```

Sample
```sh
docker run -d --name=frpc --restart=always -v /root/data/docker_data/frpc/frpc.ini:/frp/frpc.ini stilleshan/frpc
```

(Optional) If you are using docker in Ugreen NAS, then <br>
Pull the docker image <br>
![image](https://github.com/guguji666666/Docker/assets/96930989/dde4377d-4335-4369-a235-44401194bfac) <br>
Configure parameters <br>
![image](https://github.com/guguji666666/Docker/assets/96930989/ce356402-b407-4c11-a4f7-9ce1aa1c956a)

