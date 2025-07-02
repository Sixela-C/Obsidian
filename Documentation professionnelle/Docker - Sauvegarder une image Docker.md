---
up:
  - "[[Documentation professionnelle]]"
---

---
## Problématique :
Nécessité de pouvoir sauvegarder les conteneurs docker.

---
## Solution :
Utilisation des commandes docker afin de créer un fichier de backup exportable sur un support externe monté.

## 1. Créer le point de montage :

- Brancher un support de stockage  sur le serveur

- Lister les périphériques connectés :
```bash
sudo fdisk -l
```

- Identifier le périphérique /dev/sdX et son système de fichier

- Créer le répertoire destiné à recevoir le montage :
```bash
sudo mkdir -p /mnt/<id_support>
```

- Monter la partition /dev/sdX sur le répertoire /mnt/<id_support> :
```bash
sudo mount -t <système_de_fichier> /dev/sdX /mnt/<id_support>/
```

- Vérifier le montage effectif :
```bash
mount
```

---
## 2. Sauvegarder une image Docker :

- Lister les conteneurs :
```bash
sudo docker ps
```

- Créer une image depuis un conteneur :
```bash
sudo docker commit -p <nom_du_conteneur> backup_<nom_du_conteneur>-$(date +%d-%m-%Y)
```

- Exporter l'image créé :
```bash
sudo docker save -o ~/backup_<nom_du_conteneur>-$(date +%d-%m-%Y).tar backup_<nom_du_conteneur>-$(date +%d-%m-%Y)
```

- Copier l'image sur un support externe préalablement monté :
```bash
sudo cp ~/backup_<nom_du_conteneur>-$(date +%d-%m-%Y).tar /mnt/<id_support>/
```

