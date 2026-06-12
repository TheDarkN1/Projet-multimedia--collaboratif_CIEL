## 🐳 Docker Compose Nextcloud

### 📁 Création du répertoire de travail

```bash
mkdir -p /opt/Nextcloud 
cd /opt/Nextcloud
```

### 📝 Création automatique du fichier `docker-compose.yml`

La commande `cat` associée à `EOF` permet d'écrire automatiquement le contenu dans le fichier `docker-compose.yml` sans avoir à l'éditer manuellement avec un éditeur de texte.

```bash

cat > docker-compose.yml << 'EOF'
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
      MYSQL_PASSWORD: Le-mot-de-passe-user_nextcloud
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
      MYSQL_PASSWORD: Le-mot-de-passe-user_nextcloud
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
