---
up:
  - "[[Documentation personnelle]]"
---
---
## **LAMP** : serveur Web Linux Apache MariaDB PHP

---
## **Objectif** : Permettre l'hébergement d'un site ou d'une application Web.

---
### 1. Mettre à jour le cache des paquets :
```bash
sudo apt-get update
```

---
### 2. Apache2 :
#### 2.1 Installer Apache2 :
```bash
sudo apt-get install -y apache2
```

#### 2.2. Activer Apache2 au démarrage :
```bash
sudo systemctl enable apache2
```

#### 2.3. Identifier l'adresse du serveur Web Apache2 pour connexion à l'interface graphique via un navigateur :
```bash
ip a
```

#### 2.4. Installer des modules complémentaires :
```bash
sudo a2enmod <nom_du_module>
```
Ici, il est recommandé d'installer les modules suivants :
- **rewrite** : gestionnaire de réécriture d'URL
- **deflate** : gestionnaire de compression
- **headers** : gestionnaire d'en-tête HTTP
- **ssl** : gestion de certificat SSL

#### 2.5. Redémarrer Apache2 :
```bash
sudo systemctl restart apache2
```

#### 2.6. Fichiers utiles :
- **/etc/apache2/apache2.conf** : fichier de configuration d'apache2
- **/etc/apache2/sites-available/** : répertoire des fichiers de configuration des sites disponibles
- **/etc/apache2/sites-enabled** : répertoire des fichiers de configuration des sites actifs
- virtualhost : site hébergé par apache, 1 virtualhost = 1 site

---
### 3. PHP :
#### 3.1. Installer PHP :
```bash
sudo apt-get install -y php
```

#### 3.2. Installer des modules complémentaires :
```bash
sudo apt-get install -y php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
```

#### 3.3. Vérifier l'activation du moteur de script PHP :
##### 3.3.1. Éditer le fichier **phpinfo.php** :
```bash
sudo nano /var/www/html/phpinfo.php
```
##### 3.3.2. Inscrire les informations suivantes dans le fichier édité :
```bash
<?php
phpinfo();
?>
```
##### 3.3.3. Accéder au site Web :
```bash
http://<adresse_ip_du_serveur_web>/phpinfo.php
```

---
### 4. MariaDB :
#### 4.1. Installer MariaDB :
```bash
sudo apt-get install -y mariadb-server
```

#### 4.2. Sécuriser MariaDB :
##### 4.2.1. Exécuter la commande suivante :
```bash
sudo mariadb-secure-installation
```
##### 4.2.2. Configurer comme suit :
```bash
"NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
SERVERS IN PRODUCTION USE! PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y
New password: **************
Re-enter new password: **************
Password updated successfully!
Reloading privilege tables..
... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them. This is intended only for testing, and to make the installation
go a bit smoother. You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
... Success!

Normally, root should only be allowed to connect from 'localhost'. This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access. This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
- Dropping test database...
... Success!
- Removing privileges on test database...
... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
... Success!

Cleaning up...

All done! If you've completed all of the above steps, your MariaDB
installation should now be secure."
```

### 4.3. Vérifier la connexion à MariaDB :
```bash
sudo mariadb -u root -p
```

### 4.4. Redémarrer MariaDB :
```bash
sudo systemctl restart mariadb
```
