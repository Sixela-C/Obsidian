---
up:
  - "[[]]"
---



# 1. Générer un clé SSh sur la machine Windows :
1.1. Ouvrir un terminal

1.2. Taper la commande :
```bash
ssh-keygen
```

1.3. Valider les options par défaut.

1.4. Une paire de clef est générée dans :
C:\Users\<nom_utilisateur>\.ssh\id_rsa (clé privée)
C:\Users\<nom_utilisateur>\.ssh\id_rsa.pub (clé publique)

# 2. Copier la clé publique sur la machine distante :
2.1. Si machine Linux, utiliser la commande :
```bash
ssh-copy-id <nom_utilisateur>@ip_distante
```
2.2.Si machine Windows :
2.2.1. Transférer une copie du fichier **id_rsa.pub** sur la machine destinataire
2.2.2. Copier manuelle le fichier **id_rsa.pub** dans le repertoire C/Users\<nom_utilisateur>\.ssh\

# 3. Tester la connexion :
```bash
ssh utilisateur@<ip_machine_distante>

```

# 4. Configuration complémentaire pour une machine Linux :
4.1. Vérifier les droits du fichier **authorized_keys** et les modifier le cas échéant :
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

4.2. Désactiver l'authentification par mot de passe :
Dans le fichier **/etc/ssh/sshd_config** modifier comme suit :
```bash
PasswordAuthentication no
PubkeyAuthentication yes
```
Puis redémarrer le service SSH :
```bash
sudo systemctl restart ssh
```

# 5. Configuration complémentaire pour une machine Windows Server :
5.1. Installer OpenSSH sur la machien distante :
Paramètres > Applications > Fonctionnalités facultatives
Ajouter une fonctionnalité
Rechercher OpenSSH Server
Cliquer sur Installer

5.2. Activer et démarrer le service :
Dans un terminal PowerShell, taper les commandes suivantes :
```bash
Start-Service sshd
Set-Service -Name sshd -StartupType 'Automatic'
```

5.3. Ouvrir le port 22 sur le pare-feu

5.4. Générer une clé SSH sur la machine cliente (qui va se connecter) :
```bash
ssh-keygen
```
5.5. Copier la clef publique sur la machine cible :
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh utilisateur@ip_serveur "mkdir $env:USERPROFILE\.ssh -Force; Add-Content -Path $env:USERPROFILE\.ssh\authorized_keys -Value ([System.IO.File]::ReadAllText('$envUSERPROFILE\.ssh\id_rsa.pub'))"

5.6. Tester la connexion :
ssh utilisateur@<adresse_ip>

