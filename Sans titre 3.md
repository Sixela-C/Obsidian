---
up:
  - "[[]]"
---
## 🧠 MÉMO ORAL – DÉPANNAGE **RÉSEAU INFORMATIQUE**

**1. Démarche de diagnostic :**

- Je pars toujours du **physique vers le logique** (modèle OSI).
    
- Je vérifie **câble**, **carte réseau**, **IP**, **passerelle**, **DNS**, **pare-feu**.
    
- J’utilise `ping`, `ipconfig`, `nslookup`, `netstat`, `tracert`.
    

**2. Raisonnement :**

- Si pas d’IP → DHCP ?
    
- Si IP OK mais pas d’accès Internet → test de ping IP et nom de domaine → DNS ?
    
- Si accès serveur KO → ping serveur, port bloqué, firewall ?
    

**3. Réflexes :**

- **Tester sur un autre poste**
    
- **Changer de câble ou port**
    
- **Vérifier logs et pare-feux**
    
- Toujours **documenter** et **faire valider la réparation** à l’utilisateur.
    

---

## 💻 MÉMO ORAL – DÉPANNAGE **SYSTÈME WINDOWS (Client/Serveur)**

**1. Symptômes classiques :**

- Lenteur, écran bleu, application KO, session impossible.
    
- Erreur de service, problème d'impression, blocage GPO.
    

**2. Étapes de dépannage :**

- Vérification des **journaux d’événements** (`eventvwr.msc`)
    
- Gestionnaire de périphériques (`devmgmt.msc`)
    
- Vérification services (`services.msc`)
    
- Commandes utiles : `sfc /scannow`, `chkdsk`, `tasklist`, `gpupdate /force`, `net start`.
    

**3. Spécifique serveur :**

- Contrôler les rôles : AD, DNS, DHCP
    
- Vérifier la réplication AD (`repadmin`, `dcdiag`)
    
- Vérifier l’état des services critiques
    

**4. Posture :**

- Agir sans paniquer, expliquer mes actions,
    
- Prioriser la **restauration du service**
    
- Penser **sauvegarde / point de restauration** si modifs système.
    

---

## 🐧 MÉMO ORAL – DÉPANNAGE **SYSTÈME LINUX (Client/Serveur)**

**1. Problèmes fréquents :**

- Démarrage bloqué, interface réseau KO, service inactif, accès SSH impossible.
    

**2. Outils et réflexes :**

- Logs : `journalctl -xe`, `dmesg`, `tail -f /var/log/syslog`
    
- Services : `systemctl status`, `systemctl restart`
    
- Réseau : `ip a`, `ip route`, `ping`, `nmcli`, `resolv.conf`
    

**3. Ciblage rapide :**

- Si un service est KO → `systemctl status` + relance
    
- Si pas d’accès SSH → vérifier `sshd.service`, pare-feu (`ufw`, `iptables`)
    
- Si lenteur ou bug → vérifier charge CPU (`top`, `htop`), disque (`df -h`)
    

**4. En mode serveur :**

- Vérifier config Apache/Nginx, MariaDB/PostgreSQL
    
- Voir les permissions (`chmod`, `chown`), fichiers de conf (`/etc/…`)
    
- Regarder si le problème vient d’un script, cron, ou d’une saturation