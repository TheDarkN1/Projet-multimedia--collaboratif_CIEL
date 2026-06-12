## 🐳 Docker Compose Calibre

### 📁 Se placer dans le répertoire de travail

```bash
cd /opt/Calibre
```

### 📝 Création automatique du fichier `docker-compose.yml`

```bash
cat > docker-compose.yml << 'EOF'
services:
  calibre-web:
    image: lscr.io/linuxserver/calibre-web:latest
    container_name: calibre-web
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - DOCKER_MODS=linuxserver/mods:universal-calibre #optional
      - OAUTHLIB_RELAX_TOKEN_SCOPE=1 #optional
    volumes:
      - /mnt/books/calibre-library:/books
      - /calibre/config:/config
    ports:
      - 8083:8083
    restart: unless-stopped
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
