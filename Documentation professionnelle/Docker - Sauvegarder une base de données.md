---
up:
  - "[[Documentation professionnelle]]"
---

---
## Problématique :
Utilisation d'une base de données dont les fichiers sont associés à un volume physique dans un conteneur docker.
Nécessité de pouvoir sauvegarder la base de données sur un support externe.

---
## Solution :
Utilisation des commandes docker afin de créer un fichier de backup exportable sur un support externe monté.

---
### 1. Dans le conteneur Docker :

- Identifier le conteneur : 
```bash
sudo docker ps
```

- Se connecter au conteneur : 
```bash
docker exec -it <nom_du_conteneur> bash
```

- Naviguer dans le répertoire jusqu'à /var/lib/mysql/db/
```bash
cd /var/lib/mysql/db
```

- Exporter la base de données mysql :
```bash
sudo mysqldump -u root -p db > /var/backups/db_backup.sql
```

- Naviguer dans le répertoire jusqu'à : /var/backups/
```bash
cd /var/bakcups
```

- Vérifier la présence du fichier db_backup.sql :
```bash
ls -la
```

- Sortir du conteneur :
```bash
exit
```

### 2. Sur l'hôte :

- Se placer dans le répertoire utilisateur :
```bash
cd
```

- Exporter la sauvegarde du conteneur vers l'hôte : 
```bash
sudo docker cp <nom_du_conteneur>:/var/backups/db_backup.sql ~
```

- Vérifier la présence du fichier dans l'hôte :
```bash
ls
```

- Brancher un support externe sur le serveur

- Créer le point de montage :
```bash
sudo mkdir -p /mnt/<id_support>
```

- Identifier le support de stockage :
```bash
sudo fdisk -l
```

- Identifier le périphérique /dev/sdX et son système de fichier
- 
- Monter le support de stockage sdX sur le point de montage /mnt/usb :
```bash
sudo mount -t <système_de_fichier> /dev/sdX /mnt/<id_support>/
```

- Vérifier le montage :
```bash
mount
```

- Vérifier les droits existants :
```bash
ls -ld /mnt/<id_support>/
```

- Élever les droits (le cas échéant) :
```bash
sudo chmod 755 /mnt/<id_support>/
```


- Copier le fichier sur le support de sauvegarde :
```bash
sudo cp db_backup.sql /mnt/<id_support>/
```

- Vérifier la présence du fichier sur le support de sauvegarde :
```bash
ls /mnt/<id_support>/
```

- Démonter le support externe (si nécessaire) :
```bash
sudo umount (-f) /mnt/<id_support>/
```

