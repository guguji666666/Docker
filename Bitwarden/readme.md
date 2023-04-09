## Deploy your own bitwarden server to store your passwords

[Download bitwarden](https://bitwarden.com/download/)

## Before we start

1. Make sure you have a custom domain, for example `abc.com`

2. Add DNS `A` record in your DNS provider, point `FQDN` to your Nginx proxy server's IP
```
For example,
You bought Domain "abc.com" from DNS provider.
Then you create FQDN "bitwarden.abc.com" for your Joplin cloud server
DNS A record > points "bitwarden.abc.com" to the IP of VM that runs Nginx proxy server
```
3. How to get your custom domain
* [Get custom domain from Aliyun](https://wanwang.aliyun.com/domain/)

* [Manage your custom domain in Aliyun](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fdc.console.aliyun.com%2Fnext%2Findex%3Fspm%3D5176.2020520207.recommends.ddomain.606c4c12SpdlTJ#/domain/list/all-domain)

* [Get custom domain from Tecent](https://cloud.tencent.com/act/pro/domain_sales?fromSource=gwzcw.6927084.6927084.6927084&utm_medium=cpc&utm_id=gwzcw.6927084.6927084.6927084&bd_vid=11313871833741623980)

* [Manage your custom domain in Tecent](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent


## Start deployment

### 1. Install bitwarden image
```sh
sudo su root
cd ~
```

Replace `demo` with your customized name, for example `gg`
```sh
docker run -d --name bitwardenrs \
  --restart unless-stopped \
  -e WEBSOCKET_ENABLED=true \
  -v /www/wwwroot/demo/:/data/ \
  -p 7474:80 \
  -p 3012:3012 \
  vaultwarden/server:latest
```
![image](https://user-images.githubusercontent.com/96930989/230751321-cb963a56-0e3a-45ad-b714-b4b56af1a744.png)

### 2. Create entry for bitwarden in Nginx proxy server

![image](https://user-images.githubusercontent.com/96930989/230751601-e44ea706-359f-43c8-bef4-be48bde60ed6.png)

Force SSL

![image](https://user-images.githubusercontent.com/96930989/230751620-d1f63263-b970-4050-886d-b491a25d0414.png)

In `advance` tab, insert the below content, replace http://127.0.0.1:3012 and http://127.0.0.1:7474 with your own IP and port
```yml
location /admin {
  return 404;
  }
  location /notifications/hub {
    proxy_pass http://127.0.0.1:3012;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
  }
  
  location /notifications/hub/negotiate {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_pass http://127.0.0.1:7474;
  }
```

![image](https://user-images.githubusercontent.com/96930989/230751706-86c92697-b46e-4773-a529-07861d389c83.png)

### 3. Access your bitwarden server

Type the domain you created for your bitwarden server, and register new account

![image](https://user-images.githubusercontent.com/96930989/230751889-cdbc4df9-9ba5-46d5-ad1a-fff6fb9b2b39.png)

![image](https://user-images.githubusercontent.com/96930989/230751924-d1e8857b-4d0f-4aeb-8468-73ca93953ff4.png)

Now you can sign in !!!

![image](https://user-images.githubusercontent.com/96930989/230751955-c06763a5-f2e4-43f8-a738-b3b6f661e1b2.png)

### 4. Disable new account registration since the server is only used by yourself

```
sudo su root
cd ~
```

Stop bitwarden and clear itscontainer
```sh
docker stop bitwardenrs
```
```sh
docker rm -f bitwardenrs
```
![image](https://user-images.githubusercontent.com/96930989/230753186-1f41f431-eba6-41b9-b341-5b9fd67c5b50.png)

Replace `demo` with your customized name, for example `gg`
```sh
docker run -d --name bitwardenrs \
  --restart unless-stopped \
  -e SIGNUPS_ALLOWED=false \
  -e WEBSOCKET_ENABLED=true \
  -v /www/wwwroot/demo/:/data/ \
  -p 7474:80 \
  -p 3012:3012 \
  vaultwarden/server:latest
```

Check the bitwarden is back
```sh
docker ps
```

![image](https://user-images.githubusercontent.com/96930989/230753215-e788889f-3104-4563-9276-93dda566ddd6.png)

