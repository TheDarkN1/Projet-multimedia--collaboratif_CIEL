## Installation de Jellyfin

### 📁 Se placer dans le répertoire de travail

```bash
cd /opt/Jellyfin
```

### 📝 Création automatique du fichier `docker-compose.yml`

```bash
cat > docker-compose.yml << 'EOF'
services:
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    user: 1000:1000
    network_mode: bridge
    ports:
      - "8096:8096"
    volumes:
      - Jellyfin:/config
      - mnt/jellyfin/media:/media
    restart: unless-stopped
    environment:
      - TZ=Europe/Paris
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
