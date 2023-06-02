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
