## Install Docker on Ubuntu 22.04 LTS or Debian

* [Install Docker](https://docs.docker.com/get-docker/)
* [Install Docker Compose](https://docs.docker.com/compose/install/)

### 1. Check system time and install common softwares
Check current system time
```sh
date
```

Modify system time
```sh
dpkg-reconfigure tzdata
```

Update packages
```sh
sudo su root
```
```sh
apt update -y
```
```sh
apt install wget curl sudo vim git -y
```

### 2. Install Docker and Docker-compose

Install Docker
```sh
wget -qO- get.docker.com | bash
```

Check Docker version
```sh
docker -v
```
![image](https://user-images.githubusercontent.com/96930989/230719938-10a528b8-68cd-4a11-9172-5cbc98d6ff7b.png)

Run docker on startup
```sh
systemctl enable docker
```
![image](https://user-images.githubusercontent.com/96930989/230719879-554236b9-4705-4695-b234-1be2ea7bfa65.png)

Install Docker-compose
```sh
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

```sh
sudo chmod +x /usr/local/bin/docker-compose
```

Check docker-compose version
```sh
docker-compose --version
```
![image](https://user-images.githubusercontent.com/96930989/230719965-556ed99e-5aef-4f9c-91ee-1854db78d7c2.png)

Modify docker config (Optional)
```sh
cat > /etc/docker/daemon.json <<EOF
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "20m",
        "max-file": "3"
    },
    "ipv6": true,
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",
    "experimental":true,
    "ip6tables":true
}
EOF
```

Restart Docker service
```sh
systemctl restart docker
```

### 3. Create folder for docker apps

```sh
sudo su root
cd ~
mkdir data
cd data
mkdir docker_data
cd docker_data
mkdir npm
```

### 4. [Install Nginx Proxy mananger](https://nginxproxymanager.com/setup/#running-the-app)

```sh
cd data
cd docker_data
cd npm
```

Create docker-compose yaml file and paste the content below
```sh
vim docker-compose.yml
```

```yml
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP
    environment:
      # Mysql/Maria connection parameters:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./mysql:/var/lib/mysql
```

![image](https://user-images.githubusercontent.com/96930989/230720788-21e84c90-f00b-4af9-be7a-821491c87fcb.png)

Check if the port 81 has been used by existing apps/services
```sh
sudo su root
lsof -i:81
```
The result below shows that port 81 is not used by other apps/services

![image](https://user-images.githubusercontent.com/96930989/230721022-393ef763-da1d-42eb-96e3-578e68e73c88.png)

Start Nginx proxy manager
```sh
cd data
cd docker_data
cd npm
```

```sh
docker-compose up -d
```

![image](https://user-images.githubusercontent.com/96930989/230721110-310bb4d0-27c6-4e7d-9490-aacbf214c03e.png)

5. Access your Nginx proxy manager

Navigate to `<IP of the host>:81` in the browser, you will see the login page below

![image](https://user-images.githubusercontent.com/96930989/227771882-61e526f2-8145-40b3-8940-3fcf367c93e4.png)

When the Nginx Proxy Manager first starts, log in with the following username and password:

[Default Proxy Manager username](https://nginxproxymanager.com/setup/#default-administrator-user): 
```
admin@example.com
```

Default Proxy Manager password: 
```
changeme
```
![image](https://user-images.githubusercontent.com/96930989/227784662-49396ef1-0092-4a6c-9cd3-177022e58eb9.png)


Once sign in, you can modify the username and password here

![image](https://user-images.githubusercontent.com/96930989/227771973-4e327ca0-8c46-47a4-ac0b-2e1dee7bbeeb.png)
##### Verify that Docker is installed correctly by running the hello-world image:
```sh
sudo docker run hello-world
```

##### That’s it! You now have Docker installed on your Ubuntu 22.04 system.
![image](https://user-images.githubusercontent.com/96930989/227760708-cf7ccf34-61fa-483a-a6ba-c049c3864f32.png)
