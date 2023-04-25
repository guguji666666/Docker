## Deploy you SearXNG - Your own search engine

## Before we start

1. Make sure you have a custom domain, for example `abc.com`

2. Add DNS `A` record in your DNS provider, point `FQDN` to your Nginx proxy server's IP
```
For example,
You bought Domain "abc.com" from DNS provider.
Then you create FQDN "search.abc.com" for your Joplin cloud server
DNS A record > points "search.abc.com" to the IP of Nginx proxy server
```
3. How to get your custom domain
* [Get custom domain from Aliyun](https://wanwang.aliyun.com/domain/)

* [Manage your custom domain in Aliyun](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fdc.console.aliyun.com%2Fnext%2Findex%3Fspm%3D5176.2020520207.recommends.ddomain.606c4c12SpdlTJ#/domain/list/all-domain)

* [Get custom domain from Tecent](https://cloud.tencent.com/act/pro/domain_sales?fromSource=gwzcw.6927084.6927084.6927084&utm_medium=cpc&utm_id=gwzcw.6927084.6927084.6927084&bd_vid=11313871833741623980)

* [Manage your custom domain in Tecent](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent)


## Start deployment

### 1. [Install Docker, Docker-compose and Nginx proxy server](https://github.com/guguji666666/Docker)

### 2. Create directory for searxng
```sh
sudo -i
```

```sh
mkdir -p /root/data/docker_data/searxng
```

```sh
cd /root/data/docker_data/searxng
```

```sh
git clone https://github.com/searxng/searxng-docker.git
```

### 3. Create yaml file and start the container

```sh
cd /root/data/docker_data/searxng/searxng-docker
```

```sh
vim docker-compose.yaml
```

Modify the yaml file to
```yml
version: '3.7'

services:
# 我们注释掉caddy的内容
  #  caddy:
  #  container_name: caddy
  #  image: caddy:2-alpine
  #  network_mode: host
  #  volumes:
  #    - ./Caddyfile:/etc/caddy/Caddyfile:ro
  #    - caddy-data:/data:rw
  #    - caddy-config:/config:rw
  #  environment:
  #    - SEARXNG_HOSTNAME=${SEARXNG_HOSTNAME:-http://localhost:80}
  #    - SEARXNG_TLS=${LETSENCRYPT_EMAIL:-internal}
  #  cap_drop:
  #    - ALL
  #  cap_add:
  #    - NET_BIND_SERVICE
  #    - DAC_OVERRIDE

  redis:
    container_name: redis
    image: "redis:alpine"
    command: redis-server --save "" --appendonly "no"
    networks:
      - searxng
    tmpfs:
      - /var/lib/redis
    cap_drop:
      - ALL
    cap_add:
      - SETGID
      - SETUID
      - DAC_OVERRIDE

  searxng:
    container_name: searxng
    image: searxng/searxng:latest
    networks:
      - searxng
    ports:
     - "8180:8080"   # 这个冒号左边的端口可以更改，右边的不要改
    volumes:
      - ./searxng:/etc/searxng:rw
    environment:
      - SEARXNG_BASE_URL=https://${SEARXNG_HOSTNAME:-https://你的域名}/
    cap_drop:
      - ALL
    cap_add:
      - CHOWN
      - SETGID
      - SETUID
      - DAC_OVERRIDE
    logging:
      driver: "json-file"
      options:
        max-size: "1m"
        max-file: "1"
networks:
  searxng:
    ipam:
      driver: default

        #volumes:
        #caddy-data:
        #caddy-config:

