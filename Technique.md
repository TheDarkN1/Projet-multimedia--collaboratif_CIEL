## 🛠️ Partie Technique

### 🖥️ Préparation de l'infrastructure pour toutes les VMs

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

#### Création du dossier du projet ou sera Navidrome 

```bash
mkdir -p /opt/navidrome 
cd /opt/navidrome
```

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
```## 🛠️ Partie Technique

### 🖥️ Préparation de l'infrastructure

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

### Création du dossier du projet ou sera Navidrome 

```bash
mkdir -p /opt/navidrome
cd /opt/navidrome
```
