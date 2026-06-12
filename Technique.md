# 🛠️ Partie Technique

## 🖥️ Préparation de l'infrastructure pour toutes les VMs



- 📖 [Présentation du projet](README.md)

#### Installation de Docker

```bash

apt update && apt upgrade -y

apt install -y ca-certificates curl

install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc

chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
  https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null

apt update

apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```
## Création des dossier systeme



#### Création du dossier du projet ou sera Calibre

```bash
mkdir -p /opt/Calibre 
cd /opt/Calibre
```

#### Création du dossier du projet ou sera Jellyfin

```bash
mkdir -p /opt/Jellyfin 
cd /opt/Jellyfin
```

#### Création du dossier du projet ou sera Nextcloud

```bash
mkdir -p /opt/Nextcloud 
cd /opt/Nextcloud
```

#### Création du dossier du projet ou sera OnlyOffice 

```bash
mkdir -p /opt/OnlyOffice 
cd /opt/OnlyOffice
```


## 💾 Montage des disque pour Nextcloud, Jellyfin, Calibre et OnlyOffice

---

## 🔍 1. Identifier le disque

Lister les disques disponibles :

```bash
lsblk
```

ou :

```bash
fdisk -l
```

👉 Exemple : `/dev/sdb`

---

## 🧱 2. Créer une partition (si disque neuf)

```bash
fdisk /dev/sdb
```

Puis :
- `n` → nouvelle partition
- `w` → sauvegarder

---

## 🧬 3. Formater le disque

⚠️ Cette commande supprime toutes les données

```bash
mkfs.ext4 /dev/sdb1
```

---

## 📁 Organisation des services et création des dossiers

| Service     | Dossier de montage        | Commande de création de dossier                  |
|-------------|---------------------------|--------------------------------------------------|
| ☁️ Nextcloud | /mnt/nextcloud           | `mkdir -p /mnt/nextcloud-data`                   |
| 🎬 Jellyfin  | /mnt/jellyfin            | `mkdir -p /mnt/media`                            |
| 📚 Calibre   | /mnt/calibre             | `mkdir -p /mnt/books`                            |

---



## 🔗 Montage du disque (exemple enlever les `)

```bash
mount /dev/`nom du disque` /mnt/`nom du dossier`
```

---

## 🧠 Exemple d’utilisation dans Docker Compose

### Nextcloud

```yaml
volumes:
  - /mnt/nextcloud-data:/var/www/html
```

### Jellyfin

```yaml
volumes:
  - /mnt/media:/media
```

### Calibre

```yaml
volumes:
  - /mnt/books:/books
```

---




## 🚀 Déploiement des services

- 📖 [Déploiement de Nextcloud](docker-file/Nextcloud.md)
- 🛠️ [Déploiement de Calibre](docker-file/Calibre.md)
- 📁 [Déploiement de Jellyfin](docker-file/Jellyfin.md)
- 📁 [Déploiement de OnlyOffice](docker-file/OnlyOffice.md)

# 🧱 Préparation des conteneurs LXC pour Uptime Kuma et Nginx

---

## 🎯 Objectif

Créer deux conteneurs LXC sous Proxmox pour héberger :

- 📡 UptimeKuma → monitoring des services
- 🌐 Nginx      → reverse proxy et gestion des accès

---

## ⚙️ Création des conteneurs LXC debian 12 (Proxmox)

### 📦 Uptime Kuma 

### À exécuter dans le shell de Proxmox

```bash
pct create 200 local:vztmpl/debian-12-standard_*.tar.zst \
  --hostname uptime-kuma \
  --memory 1024 \
  --cores 1 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --storage local-lvm \
  --rootfs local-lvm:8 \
  --unprivileged 1
```

---

### 🌐 Nginx (reverse proxy)

```bash
pct create 201 local:vztmpl/debian-12-standard_*.tar.zst \
  --hostname nginx \
  --memory 512 \
  --cores 1 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --storage local-lvm \
  --rootfs local-lvm:4 \
  --unprivileged 1
```

---

## 🚀 Démarrer les conteneurs 

```bash
pct start 200
pct start 201
```

---

## 🔍 Vérification

```bash
pct list
```

---

## 🌐 Installation rapide dans les LXC

### 📡 Uptime Kuma

```bash
curl -fsSL https://get.docker.com | sh
docker run -d --restart=always -p 3001:3001 \
  louislam/uptime-kuma
```

---

### 🌐 Nginx

```bash
apt update && apt install nginx -y
```

---

## 🔥 Résumé

| LXC | Service | Rôle |
|-----|--------|------|
| 200 | Uptime Kuma | Monitoring |
| 201 | Nginx | Reverse proxy |


