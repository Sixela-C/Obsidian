---
up:
  - "[[Documentation professionnelle]]"
---

---
## 1. Importer la clef publique nécessaire au gestionnaire de paquets :

- Importer la clef publique GPG de MongoDB :
```bash
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc
```
_Remarques_ : La clef s'affiche dans le **terminal** et est enregistrée dans le fichier système **/etc/apt/trusted.gpg.d**

- En cas d'erreur indiquant que **gnupg** n'est pas installé, installer le :
```bash
sudo apt-get install gnupg
```

- Réessayer d'importer la clef :   
```bash
wget -qO- https://www.mongodb.org/static/pgp/server-8.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-8.0.asc
```

## 2. Créer la liste de fichiers pour MongoDB :

- Identifier la version du système d'exploitation installé sur le serveur :
```bash
lsb_release -a
```

- Créer la liste de fichiers **/etc/apt/sources.list.d/mongodb-org-8.0.list** :
```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu <nom_de_la_version>/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```

## 3. Mettre à jour la liste des paquets disponibles :

- Charger la liste des paquets disponibles sur le dépôt apt :
```bash
sudo apt-get update
```

## 4. Installation du paquet Mongosh :

- Installer la dernière version stable de **mongosh** avec la bibliothèque **OpenSSL** :
```bash
sudo apt-get install -y mongodb-mongosh-openssl
```

## 5. Vérifier que Mongosh a été installé avec succès :

- Pour confirmer que **mongosh** a bien été installé :
```bash
mongosh --version
```
  Le terminal répond avec la version installé de **mongosh**.
