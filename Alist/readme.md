## Deploy your Alist server

[Alist official installation guidance (with docker)](https://alist-doc.nn.ci/en/docs/install/docker)
[Alist github](https://github.com/alist-org/alist)

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

### 2. Create directory for Alist

### 3. Install Alist (stable version)

Check if port 5244 is used by other existing apps/services
```sh
apt install lsof
lsof -i:5244
```

Replace `/etc/alist` with your own local path for alist folder
```sh
docker run -d --restart=always -v /etc/alist:/opt/alist/data -p 5244:5244 --name="alist" xhofe/alist:latest
```

For example, i used customed path here
```sh
cd /mnt/sata1-1/alist
```
```sh
docker run -d --restart=always -v /mnt/sata1-1/alist:/opt/alist/data -p 5244:5244 --name="alist" xhofe/alist:latest
```

