## Deploy your own Joplin cloud server

* [Joplin](https://joplinapp.org/)
* [Official docker image](https://hub.docker.com/r/joplin/server)
* [Joplin GitHub](https://github.com/laurent22/joplin)

## Before we start

1. Make sure you have a custom domain

2. Add DNS `A` record in your DNS provider, point `FQDN` to your Nginx proxy server's IP
```
For example,
You bought Domain "abc.com" from DNS provider.
Then you create FQDN "Joplin.abc.com" for your Joplin service.
DNS A record > points "Joplin.abc.com" to the IP of VM that runs Nginx proxy server
```
3. How to get your custom domain
* [Get custom domain from Aliyun](https://wanwang.aliyun.com/domain/)

* [Manage your custom domain in Aliyun](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fdc.console.aliyun.com%2Fnext%2Findex%3Fspm%3D5176.2020520207.recommends.ddomain.606c4c12SpdlTJ#/domain/list/all-domain)

* [Get custom domain from Tecent](https://cloud.tencent.com/act/pro/domain_sales?fromSource=gwzcw.6927084.6927084.6927084&utm_medium=cpc&utm_id=gwzcw.6927084.6927084.6927084&bd_vid=11313871833741623980)

* [Manage your custom domain in Tecent](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent


## Start deployment

### 1. [Install Docker, Docker-compose and Nginx proxy server](https://github.com/guguji666666/Docker)

### 2. Create directory for Joplin

```sh
sudo su root
mkdir -p /root/data/docker_data/joplin
```

### 3. Create yaml file for Joplin

```sh
cd /root/data/docker_data/joplin
```

Check if ports 5432 and 22300 are used by existing apps/services
```sh
lsof -i:5432
```

```sh
lsof -i:22300
```

```sh
nano docker-compose.yml
```

Paste the contect below to yaml file
```yml
# This is a sample docker-compose file that can be used to run Joplin Server
# along with a PostgreSQL server.
#
# Update the following fields in the stanza below:
#
# POSTGRES_USER
# POSTGRES_PASSWORD
# APP_BASE_URL
#
# APP_BASE_URL: This is the base public URL where the service will be running.
#	- If Joplin Server needs to be accessible over the internet, configure APP_BASE_URL as follows: https://example.com/joplin. 
#	- If Joplin Server does not need to be accessible over the internet, set the the APP_BASE_URL to your server's hostname. 
#     For Example: http://[hostname]:22300. The base URL can include the port.
# APP_PORT: The local port on which the Docker container will listen. 
#	- This would typically be mapped to port to 443 (TLS) with a reverse proxy.
#	- If Joplin Server does not need to be accessible over the internet, the port can be mapped to 22300.

version: '3'

services:
    db:
        image: postgres:13
        volumes:
            - ./data/postgres:/var/lib/postgresql/data
        ports:
            - "5432:5432"  # 左边的端口可以更换，右边不要动！
        restart: unless-stopped
        environment:
            - POSTGRES_PASSWORD=changeme # 改成你自己的密码
            - POSTGRES_USER=username  # 改成你自己的用户名
            - POSTGRES_DB=joplin
    app:
        image: joplin/server:latest
        depends_on:
            - db
        ports:
            - "22300:22300" # 左边的端口可以更换，右边不要动！
        restart: unless-stopped
        environment:
            - APP_PORT=22300
            - APP_BASE_URL=https://gjsjoplin.aceultraman.com # 改成反代的域名
            - DB_CLIENT=pg
            - POSTGRES_PASSWORD=changeme # 与上面的密码对应！
            - POSTGRES_DATABASE=joplin
            - POSTGRES_USER=username  # 与上面的用户名对应！
            - POSTGRES_PORT=5432 # 与上面右边的对应！
            - POSTGRES_HOST=db
```
