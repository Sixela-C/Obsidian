---
up:
  - "[[Documentation professionnelle]]"
---

---
## Problématique :

Risque de saturation du stockage du système hôte par les logs.
Disparition des logs en cas de suppression du conteneur.

---
## Solution :

Mettre en place d'une rotation des logs.

---
## Localisation des logs :

Les logs des containers sont stockés dans le répertoire :
```bash
/var/lib/docker/containers/
```

---
## 1. Estimer le volumes des logs Docker :

- Exécuter la commande suivante pour afficher la taille des logs par conteneur :
```bash
sudo du -h /var/lib/docker/containers/*/*-json.log | sort -h
```

- Exécuter la commande suivantes pour afficher la taille des logs par intervalle de temps :
```bash
watch -n 3600 'sudo du -h /var/lib/docker/containers/*/*-json.log | grep total' 
```

## 2. Gérer la rotation générale des logs Docker :

- Naviguer vers le répertoire des fichiers de configurations de docker /etc/docker :
```bash
cd /etc/docker
```

- Éditer le fichier de configuration de Docker :
```bash
sudo nano daemon.json
```

- Ajouter les lignes suivantes :
```bash
{
	"log-driver": "json-file", #Pilote de log par défaut de Docker
	"log-opts": {
		"max-size": "10m", #Taille limite 10Mo
		"max-file": "3" #3 fichiers de logs maximum - 1 actif et 2 pour rotation
	}
}
```
_Remarques_ : **max-size** et **max-file** sont à adapter en fonction des informations obtenus en **1**.

- Redémarrer Docker pour appliquer la configuration à tous les conteneurs :
```bash
sudo systemctl restart docker
```

## 3. Gérer la rotation des logs d'un service (configuration prioritaire) :

- Naviguer vers le répertoire contenant le fichier docker-compose.yml :
```bash
cd /chemin_du_service/
```

- Éditer le fichier docker-compose.yml du service

- Ajouter les lignes suivantes dans le fichier docker-compose.yml du service :
```bash
services:
  nom_du_service:
    image: 
	logging:
	  driver: "json-file" #Pilote de log par défaut de Docker
	  options:
	    max-file: 3 #3 fichiers de logs maximum - 1 actif et 2 pour rotation
	    max-size: 10m #Taille limite 10Mo
```
_Remarques_ : **max-size** et **max-file** sont à adapter en fonction des informations obtenus en **1**.

- Exécuter la commande suivante pour prise en compte du changement de configuration en arrière plan :
```bash
sudo docker-compose up -d
```

