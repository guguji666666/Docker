## Deploy Portainer to manage dock containers on your machine

### [Install Portainer](https://www.portainer.io/install)
![image](https://user-images.githubusercontent.com/96930989/230776317-6ce85bfb-43b2-4e5d-9279-656fe1184191.png)

### [Install Portainer Community Edition](https://docs.portainer.io/start/install-ce/server/docker/linux#deployment)

## Before we start

1. Make sure you have a custom domain, for example `abc.com`

2. Add DNS `A` record in your DNS provider, point `FQDN` to your Nginx proxy server's IP
```
For example,
You bought Domain "abc.com" from DNS provider.
Then you create FQDN "portainer.abc.com" for your portainer service
DNS A record > points "portainer.abc.com" to the IP of VM that runs Nginx proxy server
```
3. How to get your custom domain
* [Get custom domain from Aliyun](https://wanwang.aliyun.com/domain/)

* [Manage your custom domain in Aliyun](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fdc.console.aliyun.com%2Fnext%2Findex%3Fspm%3D5176.2020520207.recommends.ddomain.606c4c12SpdlTJ#/domain/list/all-domain)

* [Get custom domain from Tecent](https://cloud.tencent.com/act/pro/domain_sales?fromSource=gwzcw.6927084.6927084.6927084&utm_medium=cpc&utm_id=gwzcw.6927084.6927084.6927084&bd_vid=11313871833741623980)

* [Manage your custom domain in Tecent](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent


## Start deployment

#### 1. Create the directory for portainer

We use custom path here for portainer
```sh
mkdir -p /root/data/docker_data/portainer
```

Confirm that the directory has been created

![image](https://user-images.githubusercontent.com/96930989/230777692-0e94ef41-4961-4cc8-85e0-7112fca2a7c7.png)


#### 2. Install Portainer

Check if the port 8000 and 9443 have been used by other existing apps/services
```sh
cd ~
```
```sh
lsof -i:8000
```
```sh
lsof -i:9443
```
![image](https://user-images.githubusercontent.com/96930989/230777522-c5eb7e4c-3b14-40b1-b039-5216575332e9.png)

Then install portainer
```sh
cd /root/data/docker_data/portainer/
```

```sh
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /root/data/docker_data/portainer/data:/data portainer/portainer-ce:latest
```
![image](https://user-images.githubusercontent.com/96930989/230777879-26c4a792-43c5-4de1-9703-d86d0a7f7b65.png)

Verify the portainer is running
```sh
docker ps
```
![image](https://user-images.githubusercontent.com/96930989/230777960-71697cb6-d23b-48a1-b7ca-d84e939b492e.png)

#### 3. Create entry for Portiner in Nginx proxy server and force SSL

![image](https://user-images.githubusercontent.com/96930989/230778723-9b35d543-969e-48eb-86e2-c7b058914148.png)

![image](https://user-images.githubusercontent.com/96930989/230778332-de8f5ca7-368c-4046-8b3f-3dacd4b9db8e.png)

#### 4. Access Portainer panel

Navigate to the domain you created for Portainer

Create new admin user

![image](https://user-images.githubusercontent.com/96930989/230778871-5c6402e6-ff6a-4fe4-afd0-222af6005370.png)

Log in, click `Get started`

![image](https://user-images.githubusercontent.com/96930989/230779047-e0b6f9f9-dab0-4f1e-b40e-c92397d7d34a.png)

![image](https://user-images.githubusercontent.com/96930989/230779016-df2e091b-51e3-4550-b5b0-2955214e3b45.png)

