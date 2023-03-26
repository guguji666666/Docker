### 1. [Install Docker on Ubuntu 22.04 LTS](https://www.youtube.com/watch?v=wCSMDtHPBso)

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
![image](https://user-images.githubusercontent.com/96930989/227760708-cf7ccf34-61fa-483a-a6ba-c049c3864f32.png)

### 2. [Useful components on docker](https://www.youtube.com/watch?v=pBwIm6m6x7M)
