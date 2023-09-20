# Odoo 

[odoo docker image](https://hub.docker.com/_/odoo/)

### Docker compose yaml file
```yml
version: '3.1'
services:
  web:
    image: odoo:latest
    depends_on:
      - mydb
    ports:
      - "8069:8069"
    environment:
    - HOST=mydb
    - USER=odoo
    - PASSWORD=myodoo
  mydb:
    image: postgres:15
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD=myodoo
      - POSTGRES_USER=odoo
```


```sh
docker-compose up -d
```
