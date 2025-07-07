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

### Impacts techniques
- **CPU/RAM** : forte activité disque, processus de chiffrement intensif.
- **Disque** : création de fichiers .locked, .crypted, etc.
- **Réseau** : exploration SMB, chiffrement des lecteurs réseau.

### Monitoring
- Détection de changements massifs de fichiers (événements FIM : File Integrity Monitoring).
- **SIEM** avec règles personnalisées sur création/modification de fichiers.

### Prévention technique
- **Backups offline/immutables** (ex : sauvegardes sur NAS déconnecté).
- **Segmentation réseau** : cloisonnement VLAN, DMZ.
- **Blocage d’accès SMB sortant** au niveau du pare-feu.
- **Désactivation de macros Office**.

## 2.4. Attaques DDoS (Distributed Denial of Service)

### Caractéristiques
- Requêtes massives sur HTTP(S), DNS, UDP, SYN flood, etc.
- Générées par des botnets (IoT, PC infectés).

### Impacts techniques
- **Bande passante saturée**.
- **CPU des serveurs web ou applicatifs à 100%**.
- Timeout sur les services.

### Monitoring
- **Zabbix, Prometheus** pour suivre le CPU, la latence, le trafic.
- Analyse des logs web (Apache/Nginx) : requêtes anormales.

### Prévention technique
- **CDN avec anti-DDoS (Cloudflare, Akamai)**.
- **Rate Limiting** sur serveur NGINX/Apache.
- **Reverse proxy + WAF**.
- **Blackhole routing** temporaire si attaque trop massive.

## 2.5. Force brute

### Caractéristiques
- Essais automatisés de combinaisons de mots de passe.

### Impacts techniques
- Surcharge des services d’authentification.
- Compromission de comptes faibles.

### Monitoring
- Logs de connexion (Windows Event ID 4625, Linux auth.log).
- Taux élevé de tentatives échouées en peu de temps.

### Prévention technique
- **Fail2Ban** ou **CrowdSec** : blocage IP après X échecs.
- MFA + mot de passe fort.
- **LDAP + politique de verrouillage** après plusieurs tentatives.

## 2.6. Exploits de vulnérabilités

### Caractéristiques
- Utilisation de failles non corrigées (ex : Log4Shell, EternalBlue).

### Impacts techniques
- **RCE (Remote Code Execution)** : exécution de code malveillant.
- **CPU/RAM** : pics soudains d’utilisation, apparition de nouveaux processus.
- **Disque** : dépôt de fichiers suspects ou malveillants.
- **Réseau** : connexions vers des serveurs externes, élévation de privilèges.

### Monitoring
- Scanners de vulnérabilités : **OpenVAS**, **Nessus**, **Qualys**.
- Surveiller l'apparition de processus système anormaux (cmd, powershell).
- **Audit du trafic réseau sortant**.

### Prévention technique
- **Patch management** automatisé (WSUS, scripts Ansible, apt/yum).
- **Désactivation des services non utilisés** (RPC, WMI, Telnet, etc.).
- **Least Privilege Principle** : utilisateurs non administrateurs.
- **Surveillance des logs système** (Syslog, journaux Windows).

## 2.7. MITM (Man-in-the-Middle)

### Caractéristiques
- Interception de communications entre deux hôtes, souvent par usurpation ARP, DNS spoofing ou fausses bornes Wi-Fi.

### Impacts techniques
- **Vol d’identifiants**, sessions HTTP, certificats.
- **Altération de trafic** (injection de contenu ou redirections).

### Monitoring
- **Wireshark** pour analyse ARP/DNS suspectes.
- **ARPWatch**, **Snort**, ou **Zeek** pour surveiller les anomalies réseau.
- Vérification des certificats SSL/TLS via navigateur ou outils comme **ssllabs.com**.

### Prévention technique
- **HTTPS strict avec HSTS**.
- **Filtrage ARP et DHCP Snooping** sur les switches managés.
- Utilisation de **VPN SSL/IPSec** pour chiffrer les flux.
- Désactiver l’accès aux ports inutiles sur les bornes Wi-Fi publiques.

## 2.8. Ingénierie sociale

### Caractéristiques
- Manipulation psychologique pour obtenir des accès : phishing ciblé, appels téléphoniques, usurpation.

### Impacts techniques
- Connexions par des comptes valides mais compromis.
- Exécution de commandes avec privilèges inappropriés.

### Monitoring
- **SIEM** pour corrélation d’accès anormaux ou inattendus (horaires, IP, géolocalisation).
- Tentatives d’élévation de privilèges inhabituelles.

### Prévention technique
- **Formation régulière des utilisateurs**.
- **Campagnes de phishing simulées**.
- **Double validation manuelle pour toute action sensible**.
- Journalisation renforcée + analyse comportementale.

## 2.9. Supply Chain

### Caractéristiques
- Attaque via un fournisseur de logiciel, matériel ou service compromis (ex : SolarWinds, CCleaner).

### Impacts techniques
- **Infection indirecte** : payload introduit dans un logiciel « légitime ».
- Compromission massive difficilement détectable.

### Monitoring
- **Hashing des exécutables** (SHA256) pour vérifier l’intégrité.
- **Analyse statique et dynamique** des binaires installés.
- **Surveillance de comportements anormaux post-installation**.

### Prévention technique
- **SBOM (Software Bill of Materials)** pour tracer les composants.
- Audit de sécurité des fournisseurs.
- **Sécurisation de la CI/CD** : intégration continue avec validation manuelle, scan SAST/DAST.
- Restriction des droits d’installation logiciel à un groupe restreint.

---

# 3. Conclusion

Une stratégie de cybersécurité efficace combine **outils techniques, surveillance, règles de sécurité strictes et formation continue**. Le suivi en temps réel des ressources (CPU, RAM, réseau) et l'analyse de journaux sont essentiels. Il faut penser **détection précoce, réaction rapide, et durcissement permanent**.

---

# 4. Sources fiables (recommandées pour approfondir)
- [ANSSI](https://www.ssi.gouv.fr)
- [CERT-FR](https://www.cert.ssi.gouv.fr)
- [Cybermalveillance.gouv.fr](https://www.cybermalveillance.gouv.fr)
- [MITRE ATT&CK Framework](https://attack.mitre.org)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)

