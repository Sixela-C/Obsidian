**CYBERSÉCURITÉ & CYBERATTAQUES : DOCUMENTATION TECHNIQUE POUR BTS SIO SISR**

---

# 1. Introduction à la cybersécurité

La cybersécurité regroupe l'ensemble des méthodes, technologies et processus visant à protéger les systèmes informatiques, les réseaux et les données contre les cyberattaques, les intrusions, les défaillances et les erreurs humaines. Elle repose sur la **confidentialité**, l'**intégrité**, la **disponibilité** (le triptyque CIA), et s'appuie sur des outils techniques, des politiques de sécurité et la formation des utilisateurs.

---

# 2. Les types de cyberattaques (avec détails techniques)

## 2.1. Malware (logiciels malveillants)

### Définition
Un malware (logiciel malveillant) est un programme conçu pour nuire à un système, voler des données, ou prendre le contrôle d’un appareil. Il existe plusieurs catégories : virus, chevaux de Troie, ransomwares, spywares, etc.

### Caractéristiques
- Dissémination par email, site web compromis, clé USB infectée.
- S’exécute généralement sans le consentement de l’utilisateur.
- Peut fonctionner en tâche de fond ou s’insérer dans des processus légitimes.

### Impacts système et réseau
- **CPU/RAM** : surconsommation de ressources (exécution cachée).
- **Stockage** : saturation (cas des ransomwares).
- **Réseau** : connexions sortantes vers des serveurs de commande (C2).

### Détection
- Activité réseau anormale.
- Processus inconnus en exécution.
- Apparition de fichiers ou clés de registre suspects.

### Prévention
- Antivirus/EDR à jour.
- Blocage de scripts (via GPO ou navigateur).
- Mise à jour des logiciels et OS.

### Exemple concret : **Emotet**
- Malware bancaire distribué par macro Word.
- Utilise PowerShell pour se propager.
- Transformé ensuite en vecteur de ransomware (Ryuk).

---

## 2.2. Phishing (hameçonnage)

### Définition
Le phishing est une tentative de tromper un utilisateur pour obtenir ses informations personnelles (identifiants, CB) en se faisant passer pour un service de confiance (banque, administration, entreprise).

### Caractéristiques
- Email, SMS ou page web imitant un site légitime.
- Utilisation d’URL similaires à des domaines officiels.

### Impacts
- **Vol d'identifiants**, données personnelles.
- **Connexion non autorisée** à des comptes.
- **Propagation interne** si hameçonnage d’un collaborateur.

### Détection
- Analyse des entêtes email.
- Liens suspects dans les messages.
- Alertes des utilisateurs.

### Prévention
- SPF, DKIM, DMARC sur la messagerie.
- Filtrage des URL malveillantes.
- Formation à la détection des emails frauduleux.

### Exemple : Email se faisant passer pour une banque avec pièce jointe Word infectée par macro (lien vers malware comme Emotet).

---

## 2.3. Ransomware (rançongiciel)

### Définition
Logiciel malveillant qui chiffre les données de l’utilisateur et demande une rançon pour les restituer.

### Caractéristiques
- Se propage souvent par phishing ou exploit.
- Utilise des algorithmes de chiffrement fort (AES, RSA).

### Impacts système
- Données inaccessibles.
- CPU élevé lors du chiffrement.
- Connexions réseau sortantes vers serveur de clé (C2).

### Détection
- Accès massif à des fichiers en lecture/écriture.
- Apparition d’extensions inconnues.
- Pics CPU/disk I/O.

### Prévention
- Sauvegardes immuables (ex : snapshot ZFS).
- Restriction des droits d’accès.
- Bloqueurs de macros (Office).

### Exemple concret : **WannaCry**
- Exploite vulnérabilité SMBv1 (EternalBlue).
- Se propage automatiquement sur le réseau.
- Chiffre les fichiers puis demande un paiement en Bitcoin.

---

## 2.4. Attaques DDoS (Déni de service distribué)

### Définition
Attaque visant à rendre un service indisponible en saturant ses ressources via une multitude de requêtes provenant de plusieurs machines compromises (botnet).

### Caractéristiques
- Trafic entrant massif.
- Implique souvent des milliers d’IP zombies.

### Impacts
- **Saturation CPU/RAM** des serveurs.
- **Bande passante réseau saturée**.
- Indisponibilité du site web, service ou API.

### Détection
- Pics brutaux de trafic réseau.
- Augmentation des temps de réponse.
- Logs d’accès contenant des patterns anormaux.

### Prévention
- Anti-DDoS (OVH, Cloudflare, Arbor).
- Limitation de débit (rate limiting).
- Firewall applicatif (WAF).

### Exemple : Attaque DDoS contre DynDNS en 2016 avec le botnet Mirai (caméras IP infectées).

---

## 2.5. Attaques internes (Insider Threats)

### Définition
Menaces provenant d’un employé, prestataire ou toute personne ayant accès légitime au système, utilisant ses droits à mauvais escient.

### Caractéristiques
- Vol de données sensibles.
- Modification ou suppression de fichiers.
- Création de portes dérobées.

### Impacts
- Fuite d’information.
- Dommages financiers et juridiques.
- Perte de confiance.

### Détection
- SIEM analysant les comportements anormaux.
- Alertes sur copies de masse ou exports.

### Prévention
- Gestion des droits d’accès (principe du moindre privilège).
- Journalisation et supervision.
- Politique de révocation d’accès rapide.

### Exemple : Employé copiant une base clients sur une clé USB avant de quitter l’entreprise.

---

## 2.6. Injections (SQLi, XSS)

### Définition
Technique visant à injecter du code malveillant via des formulaires web ou URL non sécurisés.

### Types
- **SQLi (SQL injection)** : permet d’exécuter des requêtes SQL arbitraires.
- **XSS (Cross Site Scripting)** : injecte du JavaScript malveillant dans une page.

### Impacts
- Accès aux bases de données.
- Vol de cookies/session.
- Défiguration de site.

### Détection
- Logs d’application avec syntaxe suspecte.
- Requêtes anormalement longues ou erratiques.

### Prévention
- Requêtes préparées (PDO, ORM).
- Validation côté serveur.
- WAF avec filtrage d’input.

### Exemple : Formulaire de login acceptant `' OR '1'='1` comme mot de passe.

---

## 2.7. Brute Force (attaque par force brute)

### Définition
Tentative automatique de deviner un mot de passe ou une clé en testant toutes les combinaisons possibles ou un dictionnaire de mots.

### Caractéristiques
- Ciblée ou à large échelle.
- Vitesse dépend de la puissance CPU/disponibilité réseau.

### Impacts
- Saturation des serveurs d’authentification.
- Compromission d’accès en cas de réussite.

### Détection
- Taux d’échec élevé dans les logs.
- Activité de connexion répétée depuis une même IP.

### Prévention
- Limitation des tentatives (fail2ban).
- Captcha ou MFA.
- Password policies robustes.

### Exemple : Script Python testant automatiquement les mots de passe SSH depuis Tor.

---

## 2.8. Exploitation de vulnérabilités (Exploits)

### Définition
Utilisation de failles connues (ou 0-day) dans les logiciels, systèmes d’exploitation ou protocoles pour exécuter du code malveillant.

### Caractéristiques
- Ciblé ou automatisé (via scanner comme Nessus).
- Utilise des payloads (ex : reverse shell).

### Impacts
- Prise de contrôle du système.
- Escalade de privilèges.

### Détection
- Comportements système anormaux (exécution de binaires non attendus).
- Alertes EDR.

### Prévention
- Patch management rigoureux.
- Désactivation des services inutiles.
- Filtrage réseau des ports/services vulnérables.

### Exemple : CVE-2021-40444 (faille dans MSHTML exploitée via document Word).

---

## 2.9. Attaque Man-In-The-Middle (MITM)

### Définition
Interception et modification de la communication entre deux parties sans qu’elles ne s’en rendent compte.

### Techniques
- ARP Spoofing.
- DNS spoofing.
- Proxy transparent.

### Impacts
- Vol de mots de passe ou de sessions.
- Injection de contenu malveillant.

### Détection
- Certificats SSL anormaux.
- Réponses DNS non cohérentes.
- Alertes IDS/IPS.

### Prévention
- Utilisation du HTTPS strict (HSTS).
- VPN pour sécuriser les échanges.
- Protection ARP (Dai sur switchs).

### Exemple : Interception sur un Wi-Fi public non sécurisé.

---

## 2.10. Attaques par Ingénierie Sociale

### Définition
L’ingénierie sociale regroupe les techniques de manipulation psychologique destinées à inciter une personne à compromettre la sécurité d’un système informatique, souvent en lui faisant exécuter une action malveillante (cliquer sur un lien, révéler un mot de passe...).

### Caractéristiques
- Ne nécessite **aucune faille technique**, cible l’utilisateur.
- Exploite **la confiance, la peur ou l’urgence**.
- Vecteurs courants : email, téléphone, SMS, réseaux sociaux, messages internes.

### Impacts techniques
- **Création de portes d’entrée** (ex : ouverture d’un fichier infecté).
- **Exécution de malware**, vol d’identifiants.
- **Accès réseau obtenu via phishing** → mouvements latéraux possibles.

### Détection
- Connexions inhabituelles avec des identifiants légitimes.
- Comportements anormaux d’utilisateurs (ex : upload massif).
- Alertes sur clics de liens ou fichiers exécutés (si EDR/antivirus).

### Prévention
- Sensibilisation régulière du personnel (phishing, arnaques).
- MFA pour réduire les risques de vol d'identifiants.
- Protection email avancée (antispam, sandbox pour pièces jointes).
- Supervision comportementale avec un EDR.

### Exemple concret : **Phishing ciblé (spear phishing)**
- Un attaquant envoie un faux mail RH à un employé contenant un lien vers un faux portail d’accès.
- L’utilisateur entre ses identifiants → vol d'accès Microsoft 365.
- L’attaquant utilise l’accès pour diffuser du malware via OneDrive.

---

## 2.11. Attaques sur la Supply Chain

### Définition
Une attaque sur la chaîne logistique logicielle (supply chain) consiste à compromettre un fournisseur de logiciel ou de service pour atteindre ses clients finaux. Elle cible souvent des **bibliothèques de dépendances**, des **outils de développement**, ou des **mises à jour logicielles**.

### Caractéristiques
- Très difficile à détecter car l’élément malveillant est considéré comme **fiable**.
- Impact souvent **massif et à grande échelle**.
- Exploite la **confiance entre les maillons** d’une chaîne de production ou de distribution logicielle.

### Impacts système et réseau
- **Installation automatique** de logiciels corrompus sur plusieurs machines.
- **Propagation rapide** si l’élément compromis est une dépendance partagée.
- **Exfiltration de données** ou accès distant via malware intégré.

### Détection
- Analyse comportementale (EDR, SIEM) détectant des anomalies post-installation.
- Surveillance des hash des binaires installés.
- Audits de dépendances et de pipelines CI/CD.

### Prévention
- Vérification des sources logicielles (signatures, dépôts officiels).
- Utilisation de **SBOM** (Software Bill of Materials).
- Sécurisation des processus CI/CD (authentification forte, audit).
- Isolement des systèmes de build.
- Contrôle strict des droits d’écriture sur les dépôts (Git, Docker).

### Exemple concret : **SolarWinds (2020)**
- L’outil Orion de SolarWinds a été compromis en amont par injection d’un malware (Sunburst).
- 18 000 clients (dont le gouvernement US) ont installé la mise à jour corrompue.
- L’attaquant a obtenu un accès étendu aux systèmes internes de nombreuses institutions.


### Exemple : Attaque SolarWinds en 2020.
---

# 3. Approfondissements techniques utiles

## 3.1. Analyse post-mortem / Forensic
- **But** : comprendre comment l'attaque a eu lieu et ce qui a été compromis.
- **Outils** : FTK Imager, Autopsy (analyse disque), Volatility (analyse mémoire RAM).
- **Étapes** : création d’une image disque, collecte de la RAM, recherche d’indicateurs de compromission (IOC).

## 3.2. Sécurité des protocoles réseau
- **Protocoles à sécuriser** : DNS (via DNSSEC), SSL/TLS (via certificats valides), ARP (via sécurisation switch), DHCP.
- **Exemples de vulnérabilités** : ARP spoofing, DNS poisoning, downgrade TLS.
- **Outils** : Wireshark, tcpdump, sslscan.

## 3.3. Durcissement des systèmes
- **Windows** : GPO, AppLocker, Defender, LSASS Protection, suppression de SMBv1.
- **Linux** : AppArmor, SELinux, auditd, iptables/nftables, suppression des services inutiles.
- **Objectif** : réduire la surface d’attaque.

## 3.4. Plan de réponse à incident
- **Étapes** : Détection → Contention → Éradication → Restauration → Post-mortem.
- **Documents** : playbooks, journal d’incident, schémas de décision.
- **Outils** : SIEM, ticketing (GLPI, RTIR), automatisation (SOAR).

## 3.5. Scripts d’automatisation
- **Langages** : PowerShell, Bash, Python.
- **Cas d’usage** : surveillance de logs, scan réseau automatique, mise à jour des signatures virales, reporting d’incidents.

## 3.6. SOC, SIEM & MITRE ATT&CK
- **SOC (Security Operations Center)** : centre de surveillance 24/7.
- **SIEM** : collecte et corrélation d’événements (ex : Wazuh, Splunk).
- **MITRE ATT&CK** : base de connaissances des techniques utilisées par les attaquants (TTP).
- **Utilisation** : cartographie des attaques, amélioration des règles de détection.

---
# 4. Impacts techniques des cyberattaques

## 4.1 Impacts sur les ressources systèmes

#### Processeur (CPU)
- **Symptômes** : pic d’utilisation prolongé, processus inconnus ou nombreux en arrière-plan.
- **Cause fréquente** : logiciels malveillants exécutés silencieusement (cryptomineur, ransomware, cheval de Troie).
- **Exemple** : un cryptominer Monero exploite le CPU à 100 % sur une machine compromise.

#### Mémoire vive (RAM)
- **Symptômes** : saturation, swap fréquent, lenteurs.
- **Cause fréquente** : charges anormales liées à des malwares ou bots.
- **Exemple** : un botnet utilise les ressources pour scanner ou attaquer d'autres machines.

#### Disque dur / Stockage
- **Symptômes** : disque plein ou presque, fichiers récemment modifiés, fichiers chiffrés.
- **Cause fréquente** : ransomware, fichiers logs volumineux, exfiltration de données temporisée.
- **Exemple** : un ransomware chiffre les fichiers et enregistre des copies `.crypted` ou `.locked`, remplissant l'espace disque.

#### Système de fichiers
- **Symptômes** : fichiers inconnus, exécutables créés, corruption de fichiers.
- **Cause fréquente** : malware, rootkit, backdoor installée pour persistance.
- **Exemple** : fichiers `.exe` ou scripts dans `AppData` ou `/tmp`.

## 4.2 Impacts sur le réseau

#### Trafic sortant anormal
- **Symptômes** : pics de bande passante, connexions vers IP inconnues.
- **Cause** : exfiltration de données, commande à distance (serveur C2).
- **Exemple** : un cheval de Troie envoie des identifiants volés vers une IP étrangère.

#### Scans internes ou externes
- **Symptômes** : augmentation du trafic vers les ports TCP/UDP variés.
- **Cause** : propagation de malware (ver), reconnaissance de réseau.
- **Exemple** : Emotet scanne le réseau pour s’installer sur d’autres machines.

#### Saturation ou DoS interne
- **Symptômes** : indisponibilité partielle des services (web, mail), lenteurs réseau.
- **Cause** : attaque de type DDoS lancée depuis ou vers le réseau interne.
- **Exemple** : un botnet infecte 10 PC internes et attaque un serveur externe → tous les PC ralentissent.

## 4.3 Impacts sur les journaux systèmes et la sécurité

#### Logs anormaux
- Tentatives de connexion SSH échouées (brute force).
- Connexions root suspectes.
- Lancement d’exécutables non approuvés.

#### Modifications de comptes
- Création de comptes administrateurs sans justification.
- Changements de mot de passe non autorisés.

#### Exécution de scripts ou processus inconnus
- Présence de processus liés à PowerShell, Python, bash ou `schtasks` inconnus.

## 4.4 Conséquences générales

| Ressource        | Impact                                                      |
|------------------|-------------------------------------------------------------|
| CPU / RAM        | Surconsommation anormale, ralentissements                   |
| Disque           | Saturation, fichiers chiffrés ou corrompus                 |
| Réseau           | Exfiltration, flooding, scan de ports                       |
| Journaux         | Traces d'intrusions, erreurs système, alertes de sécurité   |
| Comptes utilisateurs | Création/modification suspectes, escalade de privilèges |

### Exemples concrets d’interprétation technique

- **Pertes de performances** : souvent signe d’un processus malveillant actif.
- **Anomalies dans les journaux** : indicateurs forts de tentative d’intrusion.
- **Trafic réseau anormal** : peut précéder une exfiltration ou un pivot réseau.

---

# 5. Prévention et sécurisation proactive

## 5.1. Mesures techniques

- **MFA (authentification multifacteur)** : Ajoute une couche de sécurité.
- **Chiffrement des données** : Données au repos (BitLocker, VeraCrypt) et en transit (SSL/TLS).
- **Sauvegardes régulières et immuables** : Snapshots ZFS, sauvegardes déconnectées ou cloud avec verrouillage de version.
- **Segmentation réseau (VLAN, DMZ)** : Isole les systèmes critiques.
- **Désactivation des services inutiles** : Réduction de la surface d’attaque.
- **Patch management** : Mises à jour régulières des systèmes et logiciels.

## 5.2. Mesures organisationnelles

- **Sensibilisation du personnel** : Formations régulières (phishing, usage mot de passe, réseaux sociaux).
- **Politique de sécurité informatique (PSSI)** : Règles formalisées (usage matériel, accès, BYOD).
- **Gestion des accès** : Principe du moindre privilège, gestion des identités (IAM).
- **Plans de continuité et de reprise d'activité (PCA/PRA)** : Anticipation des interruptions de service.

## 5.3. Politiques de durcissement (hardening)

- **GPO (Windows)** : Blocage de l'accès aux paramètres sensibles, désactivation des macros, restriction des exécutables.
- **Antivirus et EDR** : Mise en place d'agents de surveillance proactive (détection comportementale).
- **Pare-feux et WAF** : Filtrage des ports, signatures applicatives (L7).

## 5.4. Supervision et audit

- **SIEM** : Agrégation des logs.
- **HIDS/NIDS** : Détection sur postes (OSSEC, Wazuh) et réseau (Suricata).
- **Tests d’intrusion réguliers (pentests)** : Identification de failles potentielles.

## 5.5. Exemples concrets de mesures de prévention

- **Sauvegarde immuable avec snapshot ZFS** : permet de restaurer un état antérieur du système après infection (ex : ransomware).
- **Mise en place de fail2ban** : bloque automatiquement les adresses IP en cas de tentatives répétées d'accès par force brute.
- **Chiffrement SSL avec Let’s Encrypt** : sécurise les échanges sur un serveur web.
- **MFA avec TOTP sur Nextcloud** : protection renforcée contre l’accès non autorisé.

---

# 6. Fiches de révision (Résumé synthétique)

| Type d’attaque  | Définition courte                                  | Symptômes système ou réseau                       | Outils de détection         | Moyens de prévention                   |
| --------------- | -------------------------------------------------- | ------------------------------------------------- | --------------------------- | -------------------------------------- |
| **Malware**     | Logiciel malveillant (virus, trojan, etc.)         | Lenteurs, fichiers suspects, process inconnus     | Antivirus, EDR, logs        | Antivirus, MAJ, blocage scripts        |
| **Phishing**    | Email/SMS/piège imitant un site pour voler données | Email suspect, redirection vers faux site         | Antispam, SPF/DKIM, SIEM    | Sensibilisation, filtrage URL          |
| **Ransomware**  | Chiffre les fichiers et demande rançon             | Extensions changées, pic CPU, accès refusé        | EDR, analyse logs, backup   | Sauvegardes, droits limités, GPO       |
| **DDoS**        | Saturation de service par trafic massif            | Site lent ou KO, bande passante saturée           | NIDS/NOC, graphique trafic  | Anti-DDoS, rate limiting, CDN          |
| **Interne**     | Utilisateur autorisé abusant de ses droits         | Copies massives, export de données, logs modifiés | SIEM, audit, journalisation | Moindre privilège, révocation rapide   |
| **SQLi / XSS**  | Injection de code via formulaire                   | Résultats anormaux, erreurs SQL                   | WAF, logs applicatifs       | Requêtes préparées, validation input   |
| **Brute Force** | Essais multiples de mots de passe                  | Tentatives multiples dans les logs, blocages      | Fail2ban, analyse SSH/log   | MFA, verrouillage, mots de passe forts |
| **Exploit**     | Utilisation d’une faille logicielle                | Processus inconnus, accès root, reverse shell     | EDR, Nessus, Metasploit     | Patch, MAJ, désactivation services     |
| **MITM**        | Interception communication entre 2 hôtes           | Certificats SSL bizarres, DNS incohérent          | IDS, analyse ARP/DNS        | HTTPS strict, VPN, DAI (switchs)       |


---

# 7. Détection des cyberattaques (méthodes et outils)

## 7.1. Surveillance réseau
- **NIDS (Network Intrusion Detection System)** : Surveille le trafic réseau à la recherche de signatures ou comportements suspects (ex : Snort, Suricata).
- **Port Mirroring** : utilisé pour envoyer une copie du trafic à un serveur d’analyse.
- **Analyse comportementale** : détection de flux ou de comportements anormaux.

## 7.2. Surveillance des endpoints
- **EDR (Endpoint Detection & Response)** : Détecte des comportements anormaux sur les postes clients (création de processus, scripts, accès disque).
- **Logs locaux** : Événements Windows (journal d'événements), syslog Linux.

## 7.3. Agrégation et corrélation des logs
- **SIEM (Security Information and Event Management)** : Centralise les logs de l’ensemble du SI pour corréler les événements suspects. Exemples : Graylog, Splunk, Wazuh, ELK Stack.
- Corrélation des journaux pour détecter les patterns d’attaque multi-sources.

## 7.4. Indicateurs d’attaque (IoC)
- Adresses IP malveillantes, hashes de fichiers connus, signatures spécifiques.
- Utilisation de **threat intelligence** (Anomali, MISP, etc.).

## 7.5. Outils de détection spécifiques
- **Sysmon** : pour capturer des événements détaillés sur Windows.
- **Auditd** : pour tracer les événements système Linux.
- **OSSEC/Wazuh** : HIDS avec règles d’alerte personnalisées.
- **Fail2Ban** : pour détecter les attaques brute-force sur les services exposés.

---
# 8. Sources & Références fiables
## 8.1. Sites officiels et institutions
ANSSI (Agence nationale de la sécurité des systèmes d’information)
https://www.ssi.gouv.fr
→ Références françaises officielles en cybersécurité (guides, alertes, recommandations, normes).

CNIL (Commission nationale de l'informatique et des libertés)
https://www.cnil.fr
→ Réglementation sur la protection des données personnelles, notifications en cas de fuite.

CERT-FR (Centre gouvernemental de veille, d’alerte et de réponse aux attaques informatiques)
https://www.cert.ssi.gouv.fr
→ Alertes et bulletins de sécurité sur les menaces actuelles.

Cybermalveillance.gouv.fr
https://www.cybermalveillance.gouv.fr
→ Ressources pédagogiques pour sensibiliser aux risques numériques.

## 8.2. Bases de données de vulnérabilités
CVE (Common Vulnerabilities and Exposures)
https://cve.mitre.org
→ Liste des vulnérabilités référencées mondialement.

NVD (National Vulnerability Database)
https://nvd.nist.gov
→ Fiches techniques et scores CVSS des vulnérabilités.

Exploit Database (Offensive Security)
https://www.exploit-db.com
→ Recense des exploits publics.

## 8.3. Outils et plateformes
Shodan
https://www.shodan.io
→ Moteur de recherche d’équipements connectés exposés sur Internet.

Have I Been Pwned
https://haveibeenpwned.com
→ Permet de vérifier si une adresse email a été compromise.

VirusTotal
https://www.virustotal.com
→ Analyse de fichiers et URLs avec plusieurs moteurs antivirus.

## 8.4. Threat Intelligence
MISP (Malware Information Sharing Platform)
https://www.misp-project.org
→ Plateforme collaborative de partage d’indicateurs de compromission (IoC).

Anomali ThreatStream
https://www.anomali.com
→ Plateforme de threat intelligence commerciale.

AlienVault Open Threat Exchange (OTX)
https://otx.alienvault.com
→ Communauté ouverte de partage d’informations sur les menaces.

## 8.5. Documentation technique & bonnes pratiques
OWASP (Open Web Application Security Project)
https://owasp.org
→ Référentiel pour la sécurité des applications web (ex : Top 10).

MITRE ATT&CK
https://attack.mitre.org
→ Framework recensant les techniques d’attaque utilisées par les acteurs malveillants.

NIST (National Institute of Standards and Technology)
https://csrc.nist.gov
→ Standards de sécurité (frameworks, guides de sécurité informatique).

