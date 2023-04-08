## Deploy your own Joplin cloud server

* [Create your Joplin account](https://joplinapp.org/)
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

### 2. 
