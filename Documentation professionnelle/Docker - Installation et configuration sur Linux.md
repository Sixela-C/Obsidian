---
up:
  - "[[Documentation professionnelle]]"
---

---

1. Installer le terminal Gnome :
```bash
sudo apt install gnome-terminal
```

2. Configurer le dépôt Docker :
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

3. Installer le paquet Docker :
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

4. Vérifier l'installation de Docker :
```bash
sudo docker run hello-world
```

5. Créer un groupe **docker** :
```bash
sudo group add docker
```

6. Ajouter l'utilisateur courant au groupe **docker** :
```bash
sudo usermod -aG docker $USER
```

7. Se déconnecter et se reconnecter pour prise en compte des modifications ou utiliser la commande :
```bash
newgrp docker
```

8. Vérifier que docker peut être lancé sans la commande **sudo** :
```bash
docker run hello-world
```

_Remarques_ : 
  Si l'erreur suivante s'affiche :
```bash
WARNING: Error loading config file: /home/user/.docker/config.json -stat /home/user/.docker/config.json: permission denied
```
   
  alors changer la propriété et les permission du répertoire ~/.docker/ :
```bash
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
```

9. Configurer Docker afin qu'il démarre au démarrage du système avec **systemd** :
```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

_Remarques_ :
  Pour stopper Docker afin qu'il ne démarre pas au démarrage du système avec **systemd** :
```bash
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```

10. Configurer le fichier de gestion des logs par défaut **json-file** :
	_**Cf. [[Docker - Gestion des Logs]]**_

  

