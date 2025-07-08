---
up:
  - "[[]]"
---
Salut ! Voici une **fiche de dépannage réseau informatique** structurée pour t’aider lors de ton épreuve pratique de BTS SIO. Elle couvre les vérifications essentielles, les spécificités Linux/Windows, les commandes utiles et les cas de débogage courants.

---

# 🛠️ **Fiche Dépannage Réseau Informatique – BTS SIO**

---

## 🧩 1. Vérifications générales (tous systèmes)

|Élément à vérifier|Détails|
|---|---|
|**Connexion physique**|Câble branché ? LED allumée ? Carte réseau activée ?|
|**Adresse IP**|Présente ? Statique ou dynamique ? Dans le bon sous-réseau ?|
|**Passerelle / DNS**|Configurés ? Joignables ?|
|**Ping loopback (127.0.0.1)**|Vérifie si la pile TCP/IP fonctionne.|
|**Ping IP locale**|Vérifie si l’interface réseau fonctionne.|
|**Ping passerelle / DNS / Internet (8.8.8.8)**|Vérifie la connectivité locale, puis vers l’extérieur.|
|**Résolution DNS**|Tester `ping google.com`. Si échec mais ping IP fonctionne → DNS défaillant.|
|**Pare-feu / Antivirus**|Bloque-t-il le trafic ? Ports fermés ?|
|**Proxy / VPN**|Mal configuré = perte d’accès. Désactiver pour tester.|

---

## 🐧 2. Spécificités Linux

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

## 🪟 3. Spécificités Windows

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

## 🔁 4. Commandes universelles utiles

|Commande|Utilité|
|---|---|
|`ping [adresse]`|Vérifier si une machine est joignable|
|`traceroute` / `tracert`|Voir le chemin vers une adresse|
|`nslookup [nom]`|Résolution DNS|
|`netstat`|Vérifie les connexions et ports utilisés|
|`telnet [IP] [port]`|Tester la connectivité à un port distant|
|`nmap [IP]`|Scanner les ports ouverts d’une machine|
|`tcpdump` / `wireshark`|Analyse réseau (paquets)|

---

## 🧪 5. Dépannages courants

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

---

Souhaite-tu une version **PDF ou imprimable** de cette fiche ? Je peux aussi te faire des **exemples de scénarios d'incidents** à résoudre si tu veux t'entraîner.