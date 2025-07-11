---
up:
  - "[[]]"
---
---
## Objectif :
Installer un CMS (_Content Management System_) permettant de créer, de modifier et d'administrer un site web.

---
## Prérequis :
Avoir installé préalablement un serveur LAMP.

---
## 1. Téléchargement de l'archive :
Naviguer dans le répertoire **/tmp** :
```bash
cd /tmp
```
Télécharger la dernière version de l'archive WordPress :
```bash
wget https://wordpress.org/latest.zip
```

---
## 2. Création d'une base de données :
### 2.1. Se connecter à MariaDB :
```bash
mysql -u root -p
```
### 2.2. Créer la base de données :
```bash
CREATE DATABASE wp_<nom_du_site>
```
### 2.3. Vérifier la création effective de la base de données :
```bash
SHOW DATABASES;
```
### 2.4. Créer l'administrateur de la base de données :
```bash
CREATE USER 'adminwp_portfolio'@'localhost' IDENTIFIED BY 'Ar3n7syxSy7Qtz';
```
### 2.5. Attribuer les droits sur la base de données à l'administrateur :
```bash
GRANT ALL PRIVILEGES ON wp_portfolio.* TO adminwp_portfolio@localhost;
```
### 2.6. Actualiser les droits et activer les privilèges :
```bash
FLUSH PRIVILEGES;
exit
```
### 2.7. La base de données est prête!

---
## 3. Installation de WordPress :
### 3.1. Archiver la page d'index d'Apache2 :
```bash
sudo mv /var/www/html/index.html /var/www/html/index.html.bk
```
### 3.2. Installer **zip** :
```bash
sudo apt-get update
sudo apt-get install zip
```
### 3.3. Décompresser l'archive WordPress depuis le répertoire **/tmp** :
```bash
cd /tmp
sudo unzip latest.zip -d /var/www/html
```
### 3.4. Déplacer les fichiers décompressés dans le répertoire **/var/www/html** :
```bash
cd /var/www/html
sudo mv wordpress/* /var/www/html/
```
### 3.5. Supprimer le dossier **wordpress** :
```bash
sudo rm -rf wordpress
```

### 3.6. Modification du propriétaire du répertoire du site :
```bash
sudo chown -R www-data:www-data /var/www/html/
```

### 3.7. Vérification des droits associés au répertoire du site :
```bash
ls -lah /var/www/html/
```
Modifier les droits (le cas échant) :
```bash
sudo find /var/www/html/ -type f -exec chmod 644 {} \;
sudo find /var/www/html/ -type d -exec chmod 755 {} \;
```

---
## 4. Configuration de WordPress :
### 4.1. Connexion à l'interface Web :
Dans un navigateur, renseigner l'adresse web :
```bash
http://<adresse_ip_du_serveur>
```
### 4.2. Configurer la langue :
- Choisir la langue désirée, ici le français, et cliquer sur **Continuer**
### 4.3. Lancer la configuration :
- Cliquer sur **C'est parti !**
### 4.4. Compléter les détails de connexion :
- Renseigner :
	- Nom de la base de données ;
	- Identifiant MySQL ;
	- Mot de passe de la base de données ;
	- Adresse de la base de données (localhost par défaut) ;
	- Préfixe des tables ;
- Cliquer sur **Envoyer**
### 4.5. Valider la confirmation de communication avec la base de données :
- Cliquer sur **Lancer l'installation**
### 4.6. Compléter les informations du site Web :
- Renseigner :
	- Titre du site ;
	- Identifiant ;
	- Mot de passe ;
	- Email.
- Cocher la case **Demander aux moteurs de recherche de ne pas indexer ce site**
- Cliquer sur **Installer WordPress**
### 4.7. Finaliser la configuration :
- Cliquer sur **Se connecter**
### 4.8. Supprimer le fichier **wp-config-sample.php** :
```bash
sudo rm /var/www/html/wp-config-sample.php
```
### 4.9. Restreindre les droits du fichier **wp-config.php** :
```bash
sudo chmod 400 /var/www/html/wp-config.php
```

