# Deploy frps (Docker-compose)

## Before we start
1. Make sure you have a custom domain, for example `abc.com`
2. Add DNS `A` record in your DNS provider, point `FQDN` to your Nginx proxy server's IP

For example, <br>
You bought Domain "abc.com" from DNS provider. <br>
Then you create FQDN "frps.abc.com" for your Nginx proxy server <br>
DNS A record > points "frps.abc.com" to the IP of VM that runs Nginx proxy server

3. How to get your custom domain
* [Get custom domain from Aliyun](https://wanwang.aliyun.com/domain/)
* [Manage your custom domain in Aliyun](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fdc.console.aliyun.com%2Fnext%2Findex%3Fspm%3D5176.2020520207.recommends.ddomain.606c4c12SpdlTJ#/domain/list/all-domain)
* [Get custom domain from Tecent](https://cloud.tencent.com/act/pro/domain_sales?fromSource=gwzcw.6927084.6927084.6927084&utm_medium=cpc&utm_id=gwzcw.6927084.6927084.6927084&bd_vid=11313871833741623980)
* [Manage your custom domain in Tecent](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent)

## Start deployment
### 1. [Install Docker, Docker-compose and Nginx proxy server](https://github.com/guguji666666/Docker)
### 2. Follow the commands below
Create directory for frps
```sh
sudo -i
```
```sh
mkdir -p /root/data/docker_data/frps
```

Configure `docker-compose.yml`
```sh
cd /root/data/docker_data/frps
```
```sh
touch frps.ini
```
```sh
vim docker-compose.yml
```
```yml
version: '3.3'
services:
    frps:
        restart: always
        network_mode: host
        volumes:
            - './frps.ini:/etc/frp/frps.ini'
        container_name: frps
        image: snowdreamtech/frps
```

Configure `frps.ini` <br>
(Replace `dashboard_user`, `dashboard_pwd` and `token` with your own values)
```sh
cd /root/data/docker_data/frps
```
```sh
vim frps.ini
```
```yml
[common]

#frp 监听端口，与客户端绑定端口

bind_port= 5443
kcp_bind_port = 5443

#dashboard用户名

dashboard_user= gugu

#dashboard密码

dashboard_pwd= passwd

#dashboard端口，启动成功后可通过浏览器访问如http://ip:9527

dashboard_port= 9527

#设置客户端token，对应客户端有页需要配置一定要记住，如果客户端不填写你连不上服务端

token = 8ad3d1x429a2d
```

Pull docker image and start container
```sh
docker-compose up -d
```

### 3. Add entry for frps in Nginx proxy manager
![image](https://github.com/guguji666666/Docker/assets/96930989/455869bd-ac40-4c40-8c79-85bce37be6f8)


### Optional (update frps)
Stop frps container
```sh
cd /root/data/docker_data/frps
```
```sh
docker-compose down 
```
Backup current container volume
```sh
cp -r /root/data/docker_data/fprs /root/data/docker_data/frps.archive  # 其实就是备份一下frps.ini这个文件
```
Get latest docker image
```sh
docker-compose pull
```
Start the container
```sh
docker-compose up -d 
```
Remove unused docker image
```sh
docker image prune  # prune 命令用来删除不再使用的 docker 对象。删除所有未被 tag 标记和未被容器使用的镜像
```

### Optional (uninstall frps)
Stop frps container
```sh
docker stop frps
```
Remove frps container
```sh
docker rm -f frps  # 停止容器，此时不会删除映射到本地的数据
```
Remove frps volume completely
```sh
rm -rf /root/data/docker_data/frps  # 完全删除映射到本地的数据
```
