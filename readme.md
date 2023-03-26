## [Install Docker on Ubuntu 22.04 LTS](https://www.youtube.com/watch?v=wCSMDtHPBso)

##### Update your system's package index and ensure that all packages installed on your system are up-to-date:

Check current system time
```sh
date
```

Modify system time
```sh
dpkg-reconfigure tzdata
```

```sh
sudo apt-get update
```

```sh
sudo apt-get upgrade
```

##### Install the dependencies necessary to add Docker’s package repository:
```sh
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

##### Add Docker’s official GPG key:
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

##### Add the Docker repository to the system:
```sh
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

##### Update the package index again to include the new Docker repository:
```sh
sudo apt-get update
```

##### Install Docker Community Edition using the following command:
```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

##### Verify that Docker is installed correctly by running the hello-world image:
```sh
sudo docker run hello-world
```

##### That’s it! You now have Docker installed on your Ubuntu 22.04 system.
![image](https://user-images.githubusercontent.com/96930989/227760708-cf7ccf34-61fa-483a-a6ba-c049c3864f32.png)


## You can also install `Docker` and `Docker-Compose` following the steps below
##### Pull down the installtion script to your local machine, this will download a script called "install_docker_nproxyman.sh" to your current directory.
```sh
wget https://gitlab.com/bmcgonag/docker_installs/-/raw/main/install_docker_nproxyman.sh
```

##### Change the permissions on the file to allow it to run with:
```sh
chmod +x install_docker_nproxyman.sh
```

##### Run the installation script
```sh
./install_docker_nproxyman.sh
````
You'll be prompted to identify your OS/Distro.  If you run an OS based on one of the options, simply select that option.

Next, you'll be asked if you want to install Docker, Docker-CE, NGinX Proxy Manager, and / or Portainer-CE.

Feel free to install them all, or just Docker and Docker-Compose. 

![image](https://user-images.githubusercontent.com/96930989/227765914-11eb09c6-46c0-4962-bfb3-f45b1a944465.png)

The steps above are from [Install Heimdall, a beaiful shortcut and informational dashboard](https://wiki.opensourceisawesome.com/books/self-hosted-dashboards/page/install-heimdall-a-beaiful-shortcut-and-informational-dashboard)

##### Verify that Docker is installed correctly by running the hello-world image:
```sh
sudo docker run hello-world
```

##### That’s it! You now have Docker installed on your Ubuntu 22.04 system.
![image](https://user-images.githubusercontent.com/96930989/227760708-cf7ccf34-61fa-483a-a6ba-c049c3864f32.png)



### 2. [Useful components on docker](https://www.youtube.com/watch?v=pBwIm6m6x7M)
#### 1. [Heimdall - an amazingly clean, user-friendly Shortcut and Informational Dashboard!](https://www.youtube.com/watch?v=qFqUXN0jxMQ)
* [Heimdall official github](https://github.com/linuxserver/Heimdall)
* [Heimdall site](https://heimdall.site/)
* [Heimdall dockerhub](https://hub.docker.com/r/linuxserver/heimdall)
* [Install Heimdall step by step guidance](https://wiki.opensourceisawesome.com/books/self-hosted-dashboards/page/install-heimdall-a-beaiful-shortcut-and-informational-dashboard)

Create a directory for Heimdall
```sh
sudo su root
cd ~
cd docker
```

```sh
mkdir heimdall
```

Then navigate to the new directory
```sh
cd heimdall
```

Then we need to create a file called "docker-compose.yml"
```sh
nano docker-compose.yml
```

We need to paste the following into the file we've just opened
```yml
version: "2.1"
services:
  heimdall:
    image: lscr.io/linuxserver/heimdall
    container_name: heimdall
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
    volumes:
      - /home/<your-user>/heimdall/config:/config
    ports:
      - 8080:80
    restart: unless-stopped
```

We may need to change a couple of things in the file you just pasted.

###### Change the TZ (timezone) if needed, to be the correct timezone for your location.
###### Change the PUID and PGID to be your user's group and user IDs. You can find them in the terminal by typing the command `id`

###### In my lab
```sh
id
```
![image](https://user-images.githubusercontent.com/96930989/227763983-6e2fdabc-f243-447d-860e-a166e5c3ba30.png)

###### On the left side of the colon ":" in the volume section, make sure to set the path to where you have created the "heimdall" folder above.

###### On the left side of the colon ":" in the ports section, make sure to set a port that is not in use on your host.  If 8080 is free, then just use it.

###### So in my lab, the yml file would be
```yml
version: "2.1"
services:
  heimdall:
    image: lscr.io/linuxserver/heimdall
    container_name: heimdall
    environment:
      - PUID=0
      - PGID=0
      - TZ=China/Shanghai
    volumes:
      - /home/gjs/heimdall/config:/config
    ports:
      - 8280:80
      - 443:443
    restart: unless-stopped
```

![image](https://user-images.githubusercontent.com/96930989/227764994-d75dd35c-a510-4a65-9c84-5eb60234ffce.png)

Once these steps are done, save the file with `CTRL + O`, then Enter to confirm, and exit the nano editor with `CTRL + X`
```sh
docker-compose up -d
```

Give it time to download the image, and start the container. Remenber to add inbound rule for the port in defined in the yml file

![image](https://user-images.githubusercontent.com/96930989/227764638-202e5941-738d-454b-968c-882b734756e5.png)

When you see "done" in the terminal, wait for 10s, and then navigate to the ip address of the host machine, and the port you set above in the web browser.

