## Containers! Containers! Containers!

### Docker

https://hub.docker.com/u/fusionauth/


FusionAuth App Only
```
docker pull fusionauth/fusionauth-app
```

FusionAuth Search Only
```
docker pull fusionauth/fusionauth-search
```

`docker-compose.yml`
```yml
version: '3'

services:
  db:
    image: postgres:9.6
    ports:
      - 5432:5432
    volumes:
      - db_data:/var/lib/postgresql/data
    restart: always
    environment:
      PGDATA : /var/lib/postgresql/data/pgdata
      POSTGRES_DB: fusionauth

  fusionauth-search:
    image: fusionauth/fusionauth-search:latest
    ports:
      - 9020:9020
      - 9021:9021
    restart: always
    volumes:
      - fa_config:/usr/local/fusionauth/config
      - fa_data:/usr/local/fusionauth/data

  fusionauth-app:
    depends_on:
      - db
      - fusionauth-search
    image: fusionauth/fusionauth-app:latest
    ports:
      - 9011:9011
    restart: always
    volumes:
      - fa_config:/usr/local/fusionauth/config

volumes:
  db_data:
  fa_config:
  fa_data:
```