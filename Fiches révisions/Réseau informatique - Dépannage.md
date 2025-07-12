---
up: []
---
---
## **1. Généralité :**

### 1.1. Démarche de diagnostic :
- Je pars toujours du **physique vers le logique** (modèle OSI).
- Je vérifie **câble**, **carte réseau**, **IP**, **passerelle**, **DNS**, **pare-feu**.
- J’utilise `ping`, `ipconfig`, `nslookup`, `netstat`, `tracert`.
### 1.2. Raisonnement :
- Si pas d’IP → DHCP ?
- Si IP OK mais pas d’accès Internet → test de ping IP et nom de domaine → DNS ?
- Si accès serveur KO → ping serveur, port bloqué, firewall ?
### 1.3. Réflexes :
- **Tester sur un autre poste**
- **Changer de câble ou port**
- **Vérifier logs et pare-feux**
- Toujours **documenter** et **faire valider la réparation** à l’utilisateur.

### **En résumé :**

| Élément à vérifier                             | Détails                                                                      |
| ---------------------------------------------- | ---------------------------------------------------------------------------- |
| **Connexion physique**                         | Câble branché ? LED allumée ? Carte réseau activée ?                         |
| **Adresse IP**                                 | Présente ? Statique ou dynamique ? Dans le bon sous-réseau ?                 |
| **Passerelle / DNS**                           | Configurés ? Joignables ?                                                    |
| **Ping loopback (127.0.0.1)**                  | Vérifie si la pile TCP/IP fonctionne.                                        |
| **Ping IP locale**                             | Vérifie si l’interface réseau fonctionne.                                    |
| **Ping passerelle / DNS / Internet (8.8.8.8)** | Vérifie la connectivité locale, puis vers l’extérieur.                       |
| **Résolution DNS**                             | Tester `ping google.com`. Si échec mais ping IP fonctionne → DNS défaillant. |
| **Pare-feu / Antivirus**                       | Bloque-t-il le trafic ? Ports fermés ?                                       |
| **Proxy / VPN**                                | Mal configuré = perte d’accès. Désactiver pour tester.                       |

---
## **2. Système WINDOWS (Client/Serveur)** :

### 2.1. Symptômes classiques :
- Lenteur, écran bleu, application KO, session impossible.
- Erreur de service, problème d'impression, blocage GPO.
### 2.2. Étapes de dépannage :
- Vérification des **journaux d’événements** (`eventvwr.msc`)
- Gestionnaire de périphériques (`devmgmt.msc`)
- Vérification services (`services.msc`)
- Commandes utiles : `sfc /scannow`, `chkdsk`, `tasklist`, `gpupdate /force`, `net start`.
### 2.3. Spécifique serveur :
- Contrôler les rôles : AD, DNS, DHCP
- Vérifier la réplication AD (`repadmin`, `dcdiag`)
- Vérifier l’état des services critiques

### 2.4. Posture :
- Agir sans paniquer, expliquer mes actions,
- Prioriser la **restauration du service**
- Penser **sauvegarde / point de restauration** si modifs système.

### **En résumé :**

|Élément|Commande|But|
|---|---|---|
|Interfaces réseau|`ipconfig /all`|Affiche toutes les infos réseau|
|Ping|`ping 8.8.8.8`|Test de connectivité|
|Résolution DNS|`nslookup google.com`|Test des serveurs DNS|
|Routage|`route print`|Voir la table de routage|
|Table ARP|`arp -a`|Voir les voisins|
|Pare-feu|`wf.msc` ou `netsh advfirewall`|Vérifier ou désactiver le pare-feu|
|Libérer/Renouveler IP|`ipconfig /release` + `ipconfig /renew`|DHCP|
|DNS Flush|`ipconfig /flushdns`|Nettoyer le cache DNS|
|État des connexions|`netstat -ano`|Voir les connexions actives|
|Diagnostic réseau|`msdt.exe /id NetworkDiagnosticsNetworkAdapter`|Assistant de résolution réseau|

---
## **3. Système LINUX (Client/Serveur)** :

### 3.1. Problèmes fréquents :
- Démarrage bloqué, interface réseau KO, service inactif, accès SSH impossible.
### 3.2. Outils et réflexes :
- Logs : `journalctl -xe`, `dmesg`, `tail -f /var/log/syslog`
- Services : `systemctl status`, `systemctl restart`
- Réseau : `ip a`, `ip route`, `ping`, `nmcli`, `resolv.conf`
### 3.3. Ciblage rapide :
- Si un service est KO → `systemctl status` + relance
- Si pas d’accès SSH → vérifier `sshd.service`, pare-feu (`ufw`, `iptables`)
- Si lenteur ou bug → vérifier charge CPU (`top`, `htop`), disque (`df -h`)
### 3.4. En mode serveur :
- Vérifier config Apache/Nginx, MariaDB/PostgreSQL
- Voir les permissions (`chmod`, `chown`), fichiers de conf (`/etc/…`)
- Regarder si le problème vient d’un script, cron, ou d’une saturation

### **En résumé :**

|Élément|Commande|But|
|---|---|---|
|Interfaces réseau|`ip a` ou `ifconfig`|Voir les interfaces et IPs|
|Routage|`ip route`|Vérifier la passerelle|
|Ping|`ping -c 4 8.8.8.8`|Test de connectivité|
|DNS|`cat /etc/resolv.conf`|Affiche les serveurs DNS|
|DHCP|`dhclient`|Demande une IP dynamique|
|Redémarrer réseau|`systemctl restart NetworkManager`|Redémarre le service réseau|
|Table ARP|`ip neigh`|Voir les hôtes connus sur le réseau|
|Journal|`journalctl -xe` ou `dmesg`|Voir les logs réseau|

**Fichiers à vérifier :**
- `/etc/network/interfaces` (Debian/Ubuntu)
- `/etc/netplan/*.yaml` (Ubuntu récents)
- `/etc/hosts` : pour les résolutions locales

---
## **4. Commandes universelles utiles :**

| Commande                 | Utilité                                  |
| ------------------------ | ---------------------------------------- |
| `ping [adresse]`         | Vérifier si une machine est joignable    |
| `traceroute` / `tracert` | Voir le chemin vers une adresse          |
| `nslookup [nom]`         | Résolution DNS                           |
| `netstat`                | Vérifie les connexions et ports utilisés |
| `telnet [IP] [port]`     | Tester la connectivité à un port distant |
| `nmap [IP]`              | Scanner les ports ouverts d’une machine  |
| `tcpdump` / `wireshark`  | Analyse réseau (paquets)                 |

---
## **5. Dépannages courants :**

|Symptôme|Causes possibles|Solutions|
|---|---|---|
|Aucune IP attribuée|DHCP KO, câble débranché, carte désactivée|Vérifier le câble, relancer DHCP, activer carte|
|Accès local OK, pas d'Internet|Passerelle/DNS mal configurés|Corriger dans la config IP|
|Pas de résolution DNS|Mauvais DNS, DNS du FAI HS|Tester avec 8.8.8.8|
|Partage réseau KO|Pare-feu bloque, SMB désactivé|Ouvrir les ports, activer SMB|
|Accès à certains sites KO|Problème DNS, proxy, MTU|Bypass proxy, changer DNS, vérifier MTU|
|Ralentissements|Duplex/MTU mal négocié, câble défectueux, saturation|Tester avec un autre câble, forcer duplex|
|Pas de ping vers machine locale|Pare-feu local actif|Autoriser ICMP|
|VPN bloque tout|Mauvaise config de split tunneling|Revoir config du VPN|
