## Deploy your own Joplin cloud server

* [Get Joplin app](https://joplinapp.org/)
* [Official docker image](https://hub.docker.com/r/joplin/server)
* [Joplin GitHub](https://github.com/laurent22/joplin)

## Before we start

1. Make sure you have a custom domain, for example `abc.com`

2. Add DNS `A` record in your DNS provider, point `FQDN` to your Nginx proxy server's IP
```
For example,
You bought Domain "abc.com" from DNS provider.
Then you create FQDN "Joplin.abc.com" for your Joplin cloud server
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
            - APP_BASE_URL=https://<your domain name set in the DNS record before> # 改成反代的域名
            - DB_CLIENT=pg
            - POSTGRES_PASSWORD=changeme # 与上面的密码对应！
            - POSTGRES_DATABASE=joplin
            - POSTGRES_USER=username  # 与上面的用户名对应！
            - POSTGRES_PORT=5432 # 与上面右边的对应！
            - POSTGRES_HOST=db
```

### 4. Start your Joplin server
```sh
cd /root/data/docker_data/joplin
```
```sh
docker-compose up -d  
```

### 5. Create entry for Joplin in Nginx proxy server

![image](https://user-images.githubusercontent.com/96930989/230730596-74245223-177b-4911-ad25-3ebe57ecb4a2.png)

If your nginx proxy server and joplin server are set on the same server

Run the command below to check the internal IP of your docker
```sh
ip addr show docker0
```
![image](https://user-images.githubusercontent.com/96930989/230730683-6c63c868-cb00-4037-a646-f441753a5497.png)

![image](https://user-images.githubusercontent.com/96930989/230748113-4a8c0adf-2ca4-4c92-bba4-d265a54c4f11.png)

Force SSL 

![image](https://user-images.githubusercontent.com/96930989/230748092-a7066ff7-fca7-433f-a4ca-820f958b2aff.png)

### 6. Access to your Joplin cloud server

Navigate to the domain you created for Joplin cloud server, for example `joplin.abc.com`

As mentioned in the [offical doc](https://joplinapp.org/), the default username and password is

![image](https://user-images.githubusercontent.com/96930989/230748229-554499c0-8435-4315-8003-0528ecb571fe.png)

Username
```
admin@localhost
```
Password
```
admin
```

![image](https://user-images.githubusercontent.com/96930989/230748248-b4bb5b73-a172-42c8-9290-e5f8a1198ba2.png)

Once you sign in, please change the password and create a new admin user

![image](https://user-images.githubusercontent.com/96930989/230748266-bd580324-b013-411f-8d25-ad4634743d4c.png)

For default admin account, we can only change password. If we change the default username we will get the error

![image](https://user-images.githubusercontent.com/96930989/230748292-30f5da74-2db1-443e-9532-0ec016d194eb.png)

Once we change the password of the default admin account, we need to logout and log in again.

Then click the Admin tab here, create a new admin user

![image](https://user-images.githubusercontent.com/96930989/230748345-15d0ecdd-92b5-4cb2-bcde-6bbdc79383c5.png)

![image](https://user-images.githubusercontent.com/96930989/230748405-5130f810-73a5-4288-806d-bdec8b20a2a6.png)

The new user will exist here

![image](https://user-images.githubusercontent.com/96930989/230748470-6d7428aa-c622-4887-953d-4d0ceff3c90f.png)

Logout and log in with your new account, confirm the mail sent to your mailbox

![image](https://user-images.githubusercontent.com/96930989/230748493-b5582995-5718-4433-b1c8-c1c992aba7eb.png)

Copy this url to your notepad, we need it later

![image](https://user-images.githubusercontent.com/96930989/230748660-38967147-6402-433f-9a1e-6ce470cbf793.png)


### 7. Set up synchronization

Navigate to your Joplin app, click tools > options

![image](https://user-images.githubusercontent.com/96930989/230748629-e1b396ea-198a-4a13-9573-9b2a7ab97bf6.png)

Type the url, the username and password of the new account we created in Joplin cloud server before

![image](https://user-images.githubusercontent.com/96930989/230748698-ccf30533-59e8-462f-8402-0d050497d6bf.png)


### 8. Save the website in markdown format into your Joplin notebook

![image](https://user-images.githubusercontent.com/96930989/230749159-1b092770-f157-4934-a2b2-ff99ce7c8cc9.png)

Once the extension is installed, we can use it to save the website

![image](https://user-images.githubusercontent.com/96930989/230749204-9aa47215-d35e-434e-8cc6-1eb5dd507825.png)

