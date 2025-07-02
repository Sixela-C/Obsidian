---
up:
  - "[[Documentation professionnelle]]"
---

---

1. Brancher le support sur le serveur

2. Afficher les périphériques et les partitions :
```bash
sudo fdisk -l
```

3. Identifier le périphérique /dev/sdX et son système de fichier

4. Démonter le périphérique (le cas échéant) :
```bash
sudo umount (-f) /dev/sdX
```

5. Formater la partition au format souhaité :
```bash
sudo mkfs.format -n CLE-FORMAT /dev/sdX
```
	Formats disponibles : ext4 - btfrs - vfat - ntsf - exfat

6. Créer le répertoire pour monter la clef USB :
```bash
sudo mkdir -p /mnt/<id_support>
```

7. Monter la clef USB :
```bash
sudo mount -t v<système_de_fichier> /dev/sdX /mnt/<id_support>/
```

8. Vérifier le montage de la clef USB :
```bash
sudo mount
```

9. Vérifier les droits existants :
```bash
ls -ld /mnt/<id_support>/
```

10. Élever les droits (le cas échéant) :
```bash
sudo chmod 755 /mnt/<id_support>/
```