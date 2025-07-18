
---
# Problématique :
Accès impossible à l'interface d'administration de WordPress en dehors du réseau local de création.

---
# Solution :
Utilisation de LocalTunnel pour obtenir une adresse IP publique permettant une connexion à l'interface d'administration même en dehors du réseau local.

---
## 1. Vérification du fonctionnement local de WordPress :

- Depuis la VM, ou depuis la machine hôte avec une connexion local via ssh, tester :
```bash
curl http://localhost
```
- Résultat = HTML WordPress.

---
##  2. Installer Node.js et npm dans la VM :

- Exécuter la commande suivante pour installer node.js et npm :
```bash
sudo apt update && sudo apt install nodejs npm -y
```

- Vérifier l'installation avec la commande suivante :
```bash
node -v
npm -v
```

---
## 3. Installer LocalTunnel :

- Exécuter la commande suivante pour installer LocalTunnel :
```bash
sudo npm install -g localtunnel
```

---
## 4. Lancer le tunnel :

- Exécuter la commande suivante afin d'exposer le port 80 associé à l'adresse IP de la VM :
```bash
lt --port 80
```

- Résultat = obtention d'une URL de la forme suivante :
```
your url is: https://abcd.loca.lt
```

---
## 5. Accéder à WordPress depuis Internet :

- Accéder à WordPress en cliquant sur le lien et en ajoutant **/wp-admin** à sa suite dans la barre d'adresse :
```
https://abcd.loca.lt/wp-admin
```

---
## 6. Accéder au mot de passe de connexion à LocalTunnel :

- Le mot de passe est accessible soit via l'URL suivante :
```bash
https://loca.lt/mytunnelpassword
```

- Soit en consultant l'adresse IP publique via le lien suivant :
```bash
https://whatismyipaddress.com/
```