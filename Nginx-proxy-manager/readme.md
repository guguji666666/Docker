### [Install Nginx Proxy Manager](https://www.youtube.com/watch?v=Z2zl2TlDzd8)

If you have installed `Nginx Proxy Manager`, the yml file of it may be like this
```yml
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

Navigate to `<IP of the host>:81` in the browser, you will see the login page below

![image](https://user-images.githubusercontent.com/96930989/227771882-61e526f2-8145-40b3-8940-3fcf367c93e4.png)

When the Nginx Proxy Manager first starts, log in with the following username and password:

Default Proxy Manager username: 
```
admin@example.com
```

Default Proxy Manager password: 
```
changeme
```

Once sign in, you can modify the username and password here

![image](https://user-images.githubusercontent.com/96930989/227771973-4e327ca0-8c46-47a4-ac0b-2e1dee7bbeeb.png)
