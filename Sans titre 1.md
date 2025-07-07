---
up:
  - "[[]]"
---
**CYBERSÉCURITÉ & CYBERATTAQUES : DOCUMENTATION TECHNIQUE POUR BTS SIO SISR**

---

# 1. Introduction à la cybersécurité

La cybersécurité regroupe l'ensemble des méthodes, technologies et processus visant à protéger les systèmes informatiques, les réseaux et les données contre les cyberattaques, les intrusions, les défaillances et les erreurs humaines. Elle repose sur la **confidentialité**, l'**intégrité**, la **disponibilité** (le triptyque CIA), et s'appuie sur des outils techniques, des politiques de sécurité et la formation des utilisateurs.

---

# 2. Les types de cyberattaques (avec détails techniques)

## 2.1. Malware (logiciels malveillants)

### Caractéristiques

- Logiciels conçus pour nuire à un système.
    
- Types : virus, vers, chevaux de Troie, ransomware, spyware.
    
- Méthodes de propagation : téléchargement, clé USB, phishing, fichiers joints.
    

### Impacts techniques

- **CPU/RAM** : pic d'utilisation anormal des ressources (processus inconnus ou multiples instances).
    
- **Disque** : écriture en boucle, espace disque consommé rapidement (ex : ransomware).
    
- **Réseau** : connexions sortantes suspectes, génération de trafic vers des C2 (command & control).
    

### Exemple concret : **Emotet (2014-2021)**

- Initialement un cheval de Troie bancaire, devenu loader de malwares.
    
- Se propageait par macro Word, récupérait les contacts pour rebondir.
    
- Impact : réseau lent saturé par le spam, postes devenus bots silencieux.
    

### Monitoring

- Outils comme **Sysmon**, **Process Explorer**, **Wireshark**, **Wazuh** ou **ELK Stack** peuvent relever ces comportements.
    
- Corrélation d'événements via un **SIEM** (ex: Graylog, Splunk).
    

### Prévention technique

- **EDR (Endpoint Detection & Response)**.
    
- **Antivirus avec comportement heuristique**.
    
- **Pare-feu local** : blocage des ports inutiles (ex : port 445 pour SMB).
    
- **GPO** (Windows) pour empêcher l'exécution de scripts dans certains répertoires.
    
- Déploiement d'applications via **whitelisting** (ex : AppLocker).
    

## 2.2. Phishing (hameçonnage)

### Caractéristiques

- Emails frauduleux, usurpation d'identité, redirection vers de faux sites web.
    

### Impacts techniques

- Compromission des accès à Active Directory, réseaux internes, VPN.
    
- Propagation de malwares depuis des comptes compromis.
    

### Exemple concret : **Campagne de phishing Microsoft 365 (2021)**

- Emails d’apparence légitime redirigeant vers une fausse page d’authentification Office 365.
    
- Vol massif de mots de passe et accès à des fichiers confidentiels.
    
- Impact : fuite de données, usurpation interne.
    

### Monitoring

- **Surveillance des logs SMTP**.
    
- Détection des connexions anormales : adresse IP géographique incohérente, device inconnu.
    

### Prévention technique

- **DMARC, DKIM, SPF** : vérification de l'origine des emails.
    
- **Proxy filtrant** : blocage des domaines récents ou suspectés de phishing.
    
- **Authentification forte (MFA)** : même si les identifiants sont volés, accès bloqué sans 2e facteur.
    
- **Isoler les comptes privilégiés** sur des postes sécurisés sans accès web.
    

## 2.3. Ransomware (rançongiciels)

### Caractéristiques

- Chiffrement des données locales et réseau.
    
- Utilisation de l’outil **vssadmin** pour supprimer les sauvegardes locales.
    
- Souvent propagé par phishing ou via des vulnérabilités non corrigées.
    

### Impacts techniques

- **CPU/RAM** : forte sollicitation due au chiffrement simultané de nombreux fichiers (multi-thread), impact visible via Task Manager ou `top`.
    
- **Disque** : création rapide de fichiers chiffrés (ex : `.locked`, `.crypted`, `.wcry`), saturation de l'espace disque.
    
- **Réseau** : exploration SMB pour trouver d'autres partages, montée en charge des liens LAN (trafic interne accru).
    

#### Exemple concret : **WannaCry (2017)**

- Exploitait la faille SMBv1 "EternalBlue" (port TCP 445).
    
- Se propageait en réseau local de machine en machine sans interaction utilisateur.
    
- Impact : arrêt massif de postes Windows, perturbation d'hôpitaux, entreprises et usines.
    

### Monitoring

- Détection de changements massifs de fichiers (FIM - File Integrity Monitoring).
    
- SIEM : règles sur modifications de fichiers par processus inhabituels (ex : `excel.exe` modifiant .docx).
    
- Logs d'événements : suppression des shadow copies, création de processus de chiffrement, montée en charge CPU anormale.
    
- Trafic SMB interne : corrélation de connexions anormales entre machines via IDS (ex : Suricata).
    

### Prévention technique

- **Backups offline/immutables** : ex. snapshots ZFS en lecture seule non modifiables, sauvegardes sur bandes ou NAS hors-ligne.
    
- **Segmentation réseau** : cloisonnement via VLAN, pas d'accès SMB croisé entre zones sensibles.
    
- **Blocage d’accès SMB sortant** au niveau du pare-feu pour éviter la propagation.
    
- **Désactivation de macros Office** via GPO.
    
- **Déploiement d'EDR** avec analyse comportementale (ex : blocage si chiffrement détecté sur plus de X fichiers/minute).
    
- **Mise à jour régulière des systèmes** (ex : désactivation SMBv1 pour bloquer WannaCry).
    

## 2.4. Attaques DDoS (Déni de service distribué)

### Caractéristiques

- Saturation des ressources réseau, CPU ou mémoire d’un système ou service.
    
- Utilisation de **botnets** (ex : Mirai) pour générer des requêtes massives.
    
- Vecteurs : HTTP, UDP, SYN Flood, amplification DNS/NTP.
    

### Impacts techniques

- **CPU/RAM** : surcharge des serveurs web ou pare-feu applicatifs.
    
- **Bande passante** : saturation des liens WAN/Internet (jusqu'à plusieurs Gbps).
    
- **Logs** : génération massive d’événements, ralentissement des traitements log.
    

### Exemple concret : **Mirai Botnet (2016)**

- Exploitait des objets connectés (IoT) mal sécurisés.
    
- A paralysé les services DNS de Dyn, impactant Twitter, Netflix, Reddit...
    
- Impact : sites inaccessibles à l’échelle mondiale.
    

### Monitoring

- IDS/IPS détectant des pics anormaux sur certains ports ou protocoles.
    
- Analyse en temps réel de bande passante (ex : Zabbix, Grafana + NetFlow).
    
- Firewall : log d'états de connexions massives (conntrack saturation).
    

### Prévention technique

- **Reverse proxy / WAF** (ex : Cloudflare, Nginx).
    
- **Rate limiting** via pare-feu applicatif ou nginx/apache.
    
- **Protection DDoS via FAI** (scrubbing center).
    
- **Règles IPTables** : limitation des connexions/s (ex : `--limit 10/s`).
    
- **QoS** ou ACL pour prioriser les flux critiques sur les routeurs.
    
## 2.5. Brute Force (attaque par force brute)

### Caractéristiques

- Tentatives multiples et automatisées de connexion à un service (SSH, RDP, FTP…).
    
- Utilisation de listes de mots de passe ou de dictionnaires (ex : `rockyou.txt`).
    

### Impacts techniques

- **CPU** : pic d’utilisation sur les services d’authentification (ex : `sshd`, `winlogon`).
    
- **Logs** : explosions des tentatives d’échec (`/var/log/auth.log`, journal d’événements Windows).
    
- **Comptes verrouillés** si politique de sécurité appliquée (DoS ciblé).
    

### Exemple concret : **Campagne SSH brute-force sur serveurs Linux exposés (2022)**

- Botnets utilisant des IP compromises pour tenter des connexions root par dictionnaire.
    
- Conséquence : comptes compromis ou système ralenti à cause des logs massifs.
    

### Monitoring

- **Fail2Ban** ou **CrowdSec** pour détecter et bannir les IP après X tentatives.
    
- Analyse SIEM des logs SSH/RDP.
    
- Surveiller les logs `/var/log/auth.log`, `Security` (Windows) pour des erreurs d’authentification répétées.
    

### Prévention technique

- **Blocage de l’IP après X échecs** (Fail2Ban, RdpGuard).
    
- **MFA** pour les services exposés.
    
- **Changer les ports par défaut** (non une solution absolue, mais limite les scans).
    
- **Désactiver les comptes par défaut** (ex : root SSH).
    
- **Déploiement d’un bastion** avec journalisation et supervision centralisée.
    

---

## 2.6. Exploitation de vulnérabilités

### Caractéristiques

- Utilisation de failles connues (CVE) pour exécuter du code arbitraire, élever les privilèges ou accéder aux données.
    
- Cibles fréquentes : serveurs web (Apache, Nginx), bases de données, applications métiers, routeurs, etc.
    

### Impacts techniques

- **CPU/RAM** : charge anormale (ex : minage de crypto après exploitation).
    
- **Fichiers modifiés** : installation de webshells, backdoors.
    
- **Logs** : erreurs 500, tentatives d’injection, requêtes suspectes.
    

### Exemple concret : **Log4Shell (CVE-2021-44228)**

- Failles dans Apache Log4j permettant une exécution de commande à distance via des logs (JNDI injection).
    
- Impact : prise de contrôle complète de serveurs, fuite massive de données.
    

### Monitoring

- IDS/IPS ou WAF surveillant les signatures d’exploitations connues.
    
- SIEM analysant les logs web (`access.log`, `error.log`) pour détection de requêtes anormales (User-Agent, URI malformée).
    

### Prévention technique

- **Mise à jour rapide** des logiciels concernés dès la publication des CVE.
    
- **Segmentation réseau** pour éviter la propagation après compromission.
    
- **WAF** pour bloquer les requêtes exploitant des patterns connus.
    
- **Supervision des fichiers sensibles** (`/var/www/html`, `/etc/passwd`) par FIM.