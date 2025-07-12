---
up:
  - "[[Fiches révisions]]"
---
## Objectif :
Installer et configurer Apache2 pour héberger un ou plusieurs sites web sur un serveur local.

---
## 1. Installation du serveur Apache :
```bash
sudo apt update
sudo apt install apache2
```

---
## 2. Fichiers de configuration importants :

|Fichier / Dossier|Rôle|
|---|---|
|`/etc/apache2/apache2.conf`|Fichier principal de config globale|
|`/etc/apache2/ports.conf`|Définit les ports d'écoute (par défaut : 80)|
|`/etc/apache2/sites-available/`|Contient les fichiers de config des sites (non activés)|
|`/etc/apache2/sites-enabled/`|Contient les liens vers les sites activés|
|`/var/www/html`|Racine du site web par défaut|
|`/etc/apache2/mods-enabled/`|Modules Apache activés|
|`/var/log/apache2/`|Dossier des logs (access.log, error.log)|

---
## 3. Commandes utiles :

```bash
# Redémarrer Apache
sudo systemctl restart apache2

# Lancer Apache au démarrage
sudo systemctl enable apache2

# Vérifier l’état du service
sudo systemctl status apache2

# Tester la configuration
sudo apache2ctl configtest
```

---
## 4. Exemple : créer un site web personnalisé :

### ➤ 1. Créer le dossier du site

```bash
sudo mkdir -p /var/www/mon-site.local
sudo chown -R www-data:www-data /var/www/mon-site.local
```

### ➤ 2. Créer un fichier index.html

```bash
echo "<h1>Bienvenue sur mon-site.local</h1>" | sudo tee /var/www/mon-site.local/index.html
```

### ➤ 3. Créer un VirtualHost

```bash
sudo nano /etc/apache2/sites-available/mon-site.local.conf
```

Contenu :

```apache
<VirtualHost *:80>
    ServerAdmin admin@mon-site.local
    ServerName mon-site.local
    DocumentRoot /var/www/mon-site.local

    <Directory /var/www/mon-site.local>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/mon-site_error.log
    CustomLog ${APACHE_LOG_DIR}/mon-site_access.log combined
</VirtualHost>
```

### ➤ 4. Activer le site

```bash
sudo a2ensite mon-site.local.conf
```

Et recharger Apache :

```bash
sudo systemctl reload apache2
```

### ➤ 5. Ajouter l'entrée DNS (sur la machine cliente ou locale)

Pour tester en local, éditer le fichier **/etc/hosts** :

```bash
sudo nano /etc/hosts
```

Puis ajouter :

```
127.0.0.1 mon-site.local
```

---
## 5. Modules utiles à activer :

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

---
## 6. Exemple : activer SSL (HTTPS) :

```bash
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2
```

Possibilité de générer un **certificat auto-signé** pour les tests :

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/apache-selfsigned.key \
  -out /etc/ssl/certs/apache-selfsigned.crt
```

Puis adapter le **VirtualHost** avec le port **443** et les chemins vers le certificat.