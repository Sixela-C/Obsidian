---
up:
  - "[[Documentation professionnelle]]"
---

---
## Changement mot de passe root :

- Connexion à la base de données :
```bash
sudo mysql -u root -p
```

- Sélectionner la base de stockage des utilisateurs :
```bash
use mysql;
```

- Modifier le nouveau mot de passe root :
```bash
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('NouveauMotDePasse');
```

- Valider la modification :
```bash
flush privileges;
```

- Quitter la prompt MySQL :
```bash
quit;
```

- Vérifier le nouveau mot de passe en se reconnectant :
```bash
sudo mysql -u root -p
```

---
## Changement mot de passe utilisateur :

- Connexion à la base de données :
```bash
mysql -u root -p
```

- Sélectionner la base de stockage des utilisateurs :
```bash
use mysql;
```

- Modifier le nouveau mot de passe utilisateur :
```bash
ALTER USER 'utilisateur'@'%' IDENTIFIED BY 'password';
```

- Vérification du changement du mot de passe :
```bash
SELECT User, Host, authentication_string FROM mysql.user WHERE User='user';
```

- Quitter la prompt MySQL :
```bash
quit;
```

- Vérifier le nouveau mot de passe en se reconnectant :
```bash
mysql -u utilisateur -p
```

---
## Vérification des droits utilisateur dans MySQL :

- Consultation des utilisateurs présents sur le serveur :
```bash
select user,host from mysql.user;
```

---
## Modification des privilèges :

- Connexion à la base de données en root 
```bash
sudo mysql -u root -p
```

- Modifier les privilèges :
```bash
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
```

- Valider la modification :
```bash
flush privileges;
```
