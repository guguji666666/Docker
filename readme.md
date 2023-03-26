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

