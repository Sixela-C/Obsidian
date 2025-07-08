---
up:
  - "[[]]"
---
Pour **50 utilisateurs**, avec :

- 📂 **stockage de fichiers partagé**
    
- 🌍 **accès distant**
    
- 🤝 **travail collaboratif (partage, coédition, permissions)**
    
- 🔒 **gestion des utilisateurs (LDAP/AD ou interne)**
    

👉 **La solution la plus adaptée est : [Nextcloud + stockage local ou Samba]**

---

## ✅ **Pourquoi Nextcloud ?**

|Fonction|Détail|
|---|---|
|🔐 **Gestion des utilisateurs**|Intégration LDAP/Active Directory ou gestion interne (groupes, droits, quotas)|
|🌐 **Accès distant sécurisé**|HTTPS, accès web, mobile, client PC/Mac/Linux|
|🧑‍🤝‍🧑 **Travail collaboratif**|Partage interne/externe, coédition (avec Collabora ou OnlyOffice)|
|🗂️ **Stockage local ou distant**|Peut utiliser un partage Samba/NFS en backend|
|📁 **Synchronisation**|Clients de synchronisation pour tous les OS|
|📊 **Audit & traçabilité**|Historique des fichiers, notifications, logs|

---

## 🔧 Architecture recommandée (locale ou hybride cloud-local)

```
                        [Internet]
                            |
                      [Reverse Proxy]
                            |
        +------------------+-------------------+
        |                                      |
   [Nextcloud Server]                  [Base de données]  
 (accès web, API, synchronisation)     (MariaDB/PostgreSQL)
        |
 [Stockage local ou monté via NFS/SMB]
        |
 [RAID/ZFS, TrueNAS, ou stockage Samba]
```

---

## 🛠️ Stack logicielle recommandée

|Composant|Technologie|
|---|---|
|**Serveur Web**|Apache ou Nginx|
|**Serveur App**|Nextcloud (PHP)|
|**Base de données**|MariaDB ou PostgreSQL|
|**Stockage**|ZFS via TrueNAS, ou Samba monté en local|
|**Reverse Proxy (accès HTTPS)**|Nginx ou Traefik|
|**Edition collaborative**|OnlyOffice ou Collabora Online (en container ou serveur dédié)|

---

## ⚙️ Spécifications matérielles conseillées

|Ressource|Recommandation|
|---|---|
|CPU|4 à 8 cœurs|
|RAM|16 à 32 Go|
|Disque|SSD pour système + RAID pour stockage (ZFS ou Btrfs recommandé)|
|Connexion|Fibre ou lien VPN sécurisé pour l’accès distant|

---

## 🔐 Sécurité & Sauvegardes

- 🔒 HTTPS avec certificat Let's Encrypt
    
- 🔐 Accès distant via **VPN** ou reverse proxy avec **fail2ban** et 2FA
    
- 💾 Sauvegardes automatisées : base de données + données + configuration
    
- 🔎 Surveillance : logs, alertes mail
    

---

## Avantages pour une entreprise

- **Contrôle total** sur vos données (self-hosted)
    
- **Accès distant sécurisé** (cloud privé)
    
- **Moins de dépendance** aux solutions Google/Microsoft
    
- **Travail collaboratif efficace** (édition de documents en ligne)
    
- **Évolutif** pour plus d’utilisateurs
    

---

### 👉 Option bonus

Vous pouvez **coupler Nextcloud avec TrueNAS SCALE** :

- TrueNAS gère le stockage (ZFS, snapshots, réplication)
    
- Nextcloud est déployé comme application dans TrueNAS (très facile)
    
---

## ⚖️ **Nextcloud vs Google Workspace – Tableau comparatif**

|**Critère**|**Nextcloud (auto-hébergé)**|**Google Workspace**|
|---|---|---|
|🏠 **Hébergement**|Serveur local ou cloud privé (ex : Proxmox, TrueNAS, VPS)|Cloud Google (serveurs aux USA/UE selon plan)|
|👥 **Utilisateurs**|Illimité (selon serveur)|Payant par utilisateur (5–30 € / mois / utilisateur)|
|💰 **Coût total**|Matériel + maintenance initiale, coût fixe|Coût récurrent par utilisateur|
|📁 **Stockage fichiers**|Stockage local (RAID/ZFS), quotas personnalisés|30 Go à illimité selon offre|
|☁️ **Accès distant**|HTTPS sécurisé + VPN ou reverse proxy|Web natif depuis partout|
|🔐 **Sécurité des données**|Contrôle total, chiffrage possible, 2FA, logs complets|2FA, protection Google, mais aucune maîtrise sur les serveurs|
|🔎 **Confidentialité**|✅ 100% sous votre contrôle (pas de scan, pub, etc.)|❌ dépend des CGU Google (pas toujours RGPD-friendly)|
|🧑‍🤝‍🧑 **Partage & collaboration**|✅ Partage fin, groupes, droits, liens publics, expiration|✅ Partage fin, édition en temps réel, commentaires|
|📝 **Coédition de documents**|✅ OnlyOffice / Collabora intégrés|✅ Google Docs/Sheets/Slides|
|📲 **Clients mobiles/PC**|✅ Android/iOS/Windows/macOS/Linux|✅ Android/iOS/PC/macOS|
|🛠️ **Maintenance**|Par vos soins ou infogérance|Gérée à 100% par Google|
|🧩 **Extensibilité (plugins)**|✅ Très riche (Talk, Mail, Agenda, Deck, etc.)|Moyenne (Apps Marketplace)|
|📊 **Outils pro intégrés**|Mail, Agenda, Deck, Talk, Notes|Gmail, Agenda, Meet, Keep, Chat, etc.|
|🔗 **Interopérabilité**|SMB, WebDAV, LDAP, SAML, AD, SSO|Bonnes APIs, mais format fermé|
|🇪🇺 **Conformité RGPD**|✅ Facile à maîtriser|❌ Soumis aux Cloud Act US (dépend du contrat)|

---

## 💡 **Quand choisir Nextcloud ?**

**👍 Idéal si vous voulez :**

- Gérer **vous-même** vos données sensibles (secteurs : santé, public, finance, R&D…)
    
- Éviter les **coûts mensuels par utilisateur**
    
- Maîtriser la **conformité RGPD**
    
- Héberger en **France ou sur site** (ZFS, TrueNAS, Proxmox…)
    
- Offrir une solution collaborative **à votre image**, extensible
    

---

## 🚫 Limites de Nextcloud

|Inconvénient|Détail|
|---|---|
|🔧 Besoin de compétences sysadmin|Installation, mises à jour, backup, sécurité à gérer soi-même|
|🧰 Moins intégré que Google|Il faut ajouter ou configurer des apps (Agenda, Office, Mail, etc.)|
|🛠️ Maintenance continue|Sécurité, compatibilité, mises à jour régulières|

---

## ✅ **Quand préférer Google Workspace ?**

**👍 Idéal si vous voulez :**

- Aucune gestion technique
    
- Des outils intégrés, "prêts à l’emploi"
    
- Coédition ultra fluide sans config
    
- Support Google et disponibilité maximale
    

**❗ Mais…**

- **Dépendance totale à Google**
    
- **Coût mensuel élevé à long terme (50 utilisateurs x ~10€/mois = 6 000 €/an)**
    
- **Peu de maîtrise sur les données / RGPD flou**
    

---

## 🔚 En résumé

|**Profil d'entreprise**|**Solution recommandée**|
|---|---|
|Structure avec **équipe IT** ou **infogérance**|✅ **Nextcloud auto-hébergé**|
|Structure sans compétence technique interne|✅ **Google Workspace**|
|Structure avec **fortes exigences RGPD**|✅ **Nextcloud**|
|Besoin de **simplicité et rapidité**|✅ **Google Workspace**|

---

Souhaitez-vous maintenant :

- Un **calcul de coût complet** (TCO) sur 3 ans ?
    
- Une **architecture d’infra Nextcloud** complète (avec sécurité, stockage, accès VPN) ?
    
- Un **guide pas-à-pas d'installation** ?