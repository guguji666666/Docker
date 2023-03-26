### 3. [Useful components on docker](https://www.youtube.com/watch?v=pBwIm6m6x7M)
#### 1. [Heimdall - an amazingly clean, user-friendly Shortcut and Informational Dashboard!](https://www.youtube.com/watch?v=qFqUXN0jxMQ)
* [Heimdall official github](https://github.com/linuxserver/Heimdall)
* [Heimdall site](https://heimdall.site/)
* [Heimdall dockerhub](https://hub.docker.com/r/linuxserver/heimdall)
* [Install Heimdall step by step guidance](https://wiki.opensourceisawesome.com/books/self-hosted-dashboards/page/install-heimdall-a-beaiful-shortcut-and-informational-dashboard)

Create a directory for Heimdall
```sh
sudo su root
```

```sh
cd ~
```

```sh
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
      - /gjsheimdall/config:/config
    ports:
      - 8280:80
    restart: unless-stopped
    
```

![image](https://user-images.githubusercontent.com/96930989/227771048-6e7f109e-4354-4250-bacb-6fca7b03f957.png)

Once these steps are done, save the file with `CTRL + O`, then Enter to confirm, and exit the nano editor with `CTRL + X`
```sh
docker-compose up -d
```

Give it time to download the image, and start the container. Remember to add inbound rule for the port in defined in the yml file

![image](https://user-images.githubusercontent.com/96930989/227764638-202e5941-738d-454b-968c-882b734756e5.png)

When you see "done" in the terminal, the hemdall is started.
