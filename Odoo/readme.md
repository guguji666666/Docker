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
    - USER=odooguguji
    - PASSWORD=myodooGucaoji!!!
  mydb:
    image: postgres:15
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD=myodooGucaoji!!!
      - POSTGRES_USER=odooguguji
```


```sh
docker-compose up -d
```
