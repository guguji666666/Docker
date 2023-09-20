# Odoo 

### Docker compose yaml file
```yml
version: '3'
services:
  odoo:
    image: odoo:latest
    env_file: .env
    depends_on:
      - postgres
    ports:
      - 8069:8069
    volumes:
      - ./data:/var/lib/odoo
  postgres:
    image: postgres:13
    env_file: .env
    volumes:
      - ./db:/var/lib/postgresql/data/pgdata

volumes:
  data:
  db:
```

```sh
docker-compose up -d
```
