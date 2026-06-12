## 🐳 Docker Compose Nextcloud

### 📁 Se placer dans le répertoire de travail

```bash
cd /opt/Nextcloud
```

### 📝 Création automatique du fichier `docker-compose.yml`

La commande `cat` associée à `EOF` permet d'écrire automatiquement le contenu dans le fichier `docker-compose.yml` sans avoir à l'éditer manuellement avec un éditeur de texte.

```bash

cat > docker-compose.yml << 'EOF'
services:
  db:
    image: mariadb:11
    container_name: nextcloud-db
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}

  redis:
    image: redis:alpine
    container_name: nextcloud-redis
    restart: always

  app:
    image: nextcloud:latest
    container_name: nextcloud-app
    restart: always
    ports:
      - 8080:80
    depends_on:
      - db
      - redis
    volumes:
      - nextcloud:/var/www/html
      - /mnt/nextcloud-data:/var/www/html/data
    environment:
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_HOST=db

  cron:
    image: nextcloud:latest
    container_name: nextcloud-cron
    restart: always
    volumes:
      - nextcloud:/var/www/html
      - /mnt/nextcloud-data:/var/www/html/data
    entrypoint: /cron.sh
    depends_on:
      - db
      - redis

volumes:
  db:
  nextcloud:

EOF

```
### ✅ Vérification de la configuration

Avant de lancer les conteneurs, on peut vérifier que le fichier `docker-compose.yml` est valide :


```bash
docker compose config
```

### 🚀 Lancement des conteneurs

```bash
docker compose up -d
```
