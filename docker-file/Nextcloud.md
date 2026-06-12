## 🐳 Docker Compose Nextcloud

```bash
mkdir -p /opt/Nextcloud 
cd /opt/Nextcloud
```

```bash

services:
mariadb:
image: mariadb:11
container_name: nextcloud-db
restart: unless-stopped
command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
environment:
MYSQL_ROOT_PASSWORD: Le-mot-de-passe-root
MYSQL_DATABASE: nextcloud
MYSQL_USER: nextcloud
MYSQL_PASSWORD: Le-mot-de-passe-use_nextcloud
volumes:
- ./mariadb:/var/lib/mysql

redis:
image: redis:7-alpine
container_name: nextcloud-redis
restart: unless-stopped
volumes:
- ./redis:/data

nextcloud:
image: nextcloud:latest
container_name: nextcloud
restart: unless-stopped
depends_on:
- mariadb
- redis
ports:
- "8080:80"
environment:
MYSQL_DATABASE: nextcloud
MYSQL_USER: nextcloud
MYSQL_PASSWORD: Le-mot-de-passe-use_nextcloud
MYSQL_HOST: mariadb


  REDIS_HOST: redis

  PHP_MEMORY_LIMIT: 1024M
  PHP_UPLOAD_LIMIT: 16G
volumes:
  - ./nextcloud:/var/www/html


cron:
image: nextcloud:latest
container_name: nextcloud-cron
restart: unless-stopped
depends_on:
- mariadb
- redis
entrypoint: /cron.sh
volumes:
- ./nextcloud:/var/www/html


```
