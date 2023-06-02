# Deploy Metube to download online videos

# 1. Deploy using Docker in NAS (UI)
### 1. Download docker image [`alexta69/metube`](https://hub.docker.com/r/alexta69/metube) or directly from your built-in docker app
Sample in Ugreen NAS <br>
![image](https://github.com/guguji666666/Docker/assets/96930989/94d9d4c0-bb8f-4ca2-ad24-b2a77a691aa7)

### 2. Define the local path and path in container
On the left, define the local path in your NAS, this is the path where you save the videos <br>
On the right, define the path in container `/downloads` , don't change this path !!!! <br>
For the permissions, define read and write <br>
![image](https://github.com/guguji666666/Docker/assets/96930989/7fca66cc-c974-4304-a9ff-7bb6e307903f)

### 3. Define the port used
On the left, define the local port on your NAS, the port should be free for the moment <br>
On the right, define the port `8081` , don't change this port!!!! <br>
![image](https://github.com/guguji666666/Docker/assets/96930989/3efdb460-d78b-4304-a28b-bee2ead23ec4)

### 4. Once configured, run the container, then access Metube via `<Internal ip>:<The port you defined>`

# 2. Deploy using Docker commands in NAS
```sh
docker run -d -p 8081:8081 -v /path/to/downloads:/downloads ghcr.io/alexta69/metube
```
For port 8081 on the left, you can customize it using the local port on your NAS, the port should be free for the moment <br>
For path `/path/to/downloads`, you can customize it using the local path in your NAS, this is the path where you save the videos <br>

Once configured, run the container, then access Metube via `<Internal ip>:<The port you defined>`
