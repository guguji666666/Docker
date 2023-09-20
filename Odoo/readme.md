# Odoo 

### Docker compose yaml file
```yml
version: “3.1”

services:
db:
     image: postgres:latest
     restart: always
     ports:
       - 5444:5432
    environment:
        POSTGRES_USER: odoo
        POSTGRES_PASSWORD: odoo
   volumes:
       - pg-odoo: /var/lib/postgresql/data
 
odoo:
    image: odoo:latest
    ports:
        - 8069:8069
    environment: 
           -  HOST=db
           -  PORT=5432
           -  USER=odoo
           -  PASSWORD=odoo
  volumes:
       - ./extra-addons:/mnt/extra-addons
       - data:/var/lib/odoo
  depends_on:
          - db
volumes:
    pg-odoo:
    data:
```
