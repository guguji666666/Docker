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



##### Verify that Docker is installed correctly by running the hello-world image:
```sh
sudo docker run hello-world
```

##### That’s it! You now have Docker installed on your Ubuntu 22.04 system.
![image](https://user-images.githubusercontent.com/96930989/227760708-cf7ccf34-61fa-483a-a6ba-c049c3864f32.png)
