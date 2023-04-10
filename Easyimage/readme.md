## Deploy your own Easyimage server to store images/pictures

* [EasyImages 2.0 github](https://github.com/icret/EasyImages2.0)
* [Docker image](https://hub.docker.com/r/ddsderek/easyimage)


## Before we start

1. Make sure you have a custom domain, for example `abc.com`

2. Add DNS `A` record in your DNS provider, point `FQDN` to your Nginx proxy server's IP
```
For example,
You bought Domain "abc.com" from DNS provider.
Then you create FQDN "image.abc.com" for your Joplin cloud server
DNS A record > points "image.abc.com" to the IP of Nginx proxy server
```
3. How to get your custom domain
* [Get custom domain from Aliyun](https://wanwang.aliyun.com/domain/)

* [Manage your custom domain in Aliyun](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fdc.console.aliyun.com%2Fnext%2Findex%3Fspm%3D5176.2020520207.recommends.ddomain.606c4c12SpdlTJ#/domain/list/all-domain)

* [Get custom domain from Tecent](https://cloud.tencent.com/act/pro/domain_sales?fromSource=gwzcw.6927084.6927084.6927084&utm_medium=cpc&utm_id=gwzcw.6927084.6927084.6927084&bd_vid=11313871833741623980)

* [Manage your custom domain in Tecent](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent)


## Start deployment

### 1. [Install Docker, Docker-compose and Nginx proxy server](https://github.com/guguji666666/Docker)

### 2. Create directory for Easyimage

```sh
mkdir -p /root/data/docker_data/easyimage
```
```sh
cd /root/data/docker_data/easyimage
```

### 3. Create yaml file for Easyimage

Check if port 8080 has been used by other existing apps/services
```sh
cd ~
```
```sh
lsof -i:8080
```

Configure yaml file
```sh
cd /root/data/docker_data/easyimage
```

```sh
nano docker-compose.yml
```
Insert the content below
```yml
version: '3.3'
services:
  easyimage:
    image: ddsderek/easyimage:latest
    container_name: easyimage
    ports:
      - '8080:80'
    environment:
      - TZ=Asia/Shanghai
      - PUID=1000
      - PGID=1000
    volumes:
      - '/root/data/docker_data/easyimage/config:/app/web/config'
      - '/root/data/docker_data/easyimage/i:/app/web/i'
    restart: unless-stopped
```
![image](https://user-images.githubusercontent.com/96930989/230806090-e31a7304-7d72-47a4-a679-9d50920230fc.png)

![image](https://user-images.githubusercontent.com/96930989/230817422-6aa77896-94be-4027-9a6a-4c99c509947f.png)


Start Easyimage
```sh
docker-compose up -d
```
![image](https://user-images.githubusercontent.com/96930989/230817543-d6487faa-e489-4405-85fa-1e44011733df.png)


### 4. Modify local config file
```sh
cd /root/data/docker_data/easyimage/config
```
```sh
ll
```
```sh
nano config.php
```

Replace `domain` and `imgurl` with the domain you created for easyimage

![image](https://user-images.githubusercontent.com/96930989/230820158-1c8cbef1-802d-469b-8c01-fa125edcc6b0.png)

Then restart Easyimage
```sh
cd /root/data/docker_data/easyimage
docker-compose restart
```

### 4. Create entry for easyimage in Nginx proxy manager

If Nginx proxy manager and easyimage are deployed on the same server, then we can get internal IP of container
```sh
ip addr show docker0
```
Note this IP

![image](https://user-images.githubusercontent.com/96930989/230817976-ddeea198-d635-4095-83c2-469712e13623.png)

![image](https://user-images.githubusercontent.com/96930989/230818482-fdca07ac-f8e1-4794-a5bb-c6fbac01e051.png)

![image](https://user-images.githubusercontent.com/96930989/230818550-ac619617-34d6-4eaf-9d8f-2959befba020.png)

### 5. Access your Easyimage server

![image](https://user-images.githubusercontent.com/96930989/230819410-42be08de-bcb5-4b1e-a774-836cb6085755.png)

![image](https://user-images.githubusercontent.com/96930989/230819786-03e056cf-afa9-4145-b3b9-977a21aa2a1f.png)

Then you will be redirected to the sign-in page

![image](https://user-images.githubusercontent.com/96930989/230819928-ec22f104-9495-4418-b519-621b60cf0d15.png)


### 6. Test uploading pictures

The link here could be used directly in markdown file 😊

![image](https://user-images.githubusercontent.com/96930989/230821094-f2850c48-4e9e-4da3-8325-fc24b523cda7.png)

### 7. Use browser extension to upload images to your Easyimage server

[Download the extension for easyiamge](https://github.com/icret/EasyImages2.0/tree/master/docs)

![image](https://user-images.githubusercontent.com/96930989/230823918-31c5a067-971d-4ca9-8c41-13b1b4ed66d9.png)

Get your API url and token here

![image](https://user-images.githubusercontent.com/96930989/230824038-78f34869-9b45-457e-8232-d31ff3cc2f83.png)

Insert the API url and API token

![image](https://user-images.githubusercontent.com/96930989/230824149-38470ea3-fe13-490b-8ffa-7ffae633a383.png)


[]()


## Optional

#### 1. Update Easyimage
Navigate to the directory of Easyimage
```sh
cd /root/data/docker_data/easyimage
```

Pull latest image
```sh
docker-compose pull
```

Update image
```sh
docker-compose up -d
```

Delete directory of installation file
```sh
docker exec -it easyimage rm -rf /app/web/install #因为更新后镜像自带install目录，所以要删除
```

#### 2. Uninstall Easyimage

```sh
sudo su root
```

```sh
cd /root/data/docker_data/easyimage
```

```sh
docker-compose down
```

```sh
cd ~
```

```sh
rm -rf /root/data/docker_data/easyimage
```
