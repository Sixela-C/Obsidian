---
up:
  - "[[Fiches révisions]]"
---
## Schéma simplifié : DNS & Active Directory

```plaintext
        +----------------------+
        | Poste Client         |
        | (PC utilisateur)     |
        +----------+-----------+
                   |
          (1) Requête DNS
                   |
                   v
        +----------------------+
        | Serveur DNS + AD DS |
        | (Contrôleur de domaine) 
        | srv-ad.entreprise.local |
        +----------+-----------+
                   |
          (2) Réponse DNS (SRV + A)
                   |
                   v
        +----------------------+
        | Résolution du DC     |
        | Connexion LDAP /     |
        | Authentification     |
        +----------------------+

* Le client demande l'adresse du service AD (_ldap._tcp.dc._msdcs.entreprise.local)
* Le serveur DNS répond avec l'adresse IP du contrôleur de domaine.
* Le client contacte ensuite ce serveur pour s’authentifier.
```

---
### 1. Définition du DNS
- **DNS = Domain Name System*
- Traduit les **noms de domaines ↔ adresses IP**
- Utilisé dans tous les réseaux TCP/IP

---
### 2. Rôle du DNS dans Active Directory
- **AD utilise le DNS** pour :
    - Localiser les **contrôleurs de domaine (DC)**
    - Trouver les **services réseau (LDAP, Kerberos, etc.)**
- Utilise des **enregistrements SRV** pour identifier les services

---
### 3. Types d’enregistrements utiles

|Type|Fonction|
|---|---|
|A|Nom de machine → IPv4|
|AAAA|Nom de machine → IPv6|
|CNAME|Alias|
|PTR|IP → nom (résolution inverse)|
|**SRV**|Spécifie un **service réseau (ex: LDAP)**|
|MX|Serveur de messagerie|

---
### 4. Configuration recommandée en AD
- **Serveur DNS intégré à Active Directory**
    - Zone **sécurisée**
    - Réplication entre DC
- Le nom de **la zone DNS = nom du domaine AD** (ex: `entreprise.local`)
- Le serveur DNS est souvent le **DC principal**

---
### 5. Pièges à éviter
- Utiliser **DNS externes** (ex : Google 8.8.8.8) → ça bloque AD
- Mauvaise configuration des zones ou des enregistrements SRV
- Oublier d’activer les **mises à jour dynamiques sécurisées**

---
### 6. À retenir
- **DNS est vital pour qu’un client AD trouve le DC**
- **SRV = cœur de la localisation des services AD**
- **Toujours configurer le poste client pour qu’il pointe vers le DNS interne**
