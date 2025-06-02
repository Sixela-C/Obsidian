---
up:
  - "[[]]"
---
# 🧠 Fiche de Révision Complète – Active Directory (AD)

## 📌 Définition

**Active Directory (AD)** est un **service d'annuaire** développé par Microsoft pour **centraliser la gestion des ressources** (utilisateurs, ordinateurs, imprimantes, etc.) dans un environnement Windows. Il repose sur le **protocole LDAP (Lightweight Directory Access Protocol)** pour permettre aux applications d’accéder, interroger et modifier les données de l’annuaire.

---

## 📡 AD & LDAP

- **LDAP** : protocole standard pour interroger et manipuler un annuaire.
    
- AD est **compatible LDAP v3**, ce qui le rend **interopérable avec d'autres systèmes**.
    
- AD utilise aussi **Kerberos** pour l’authentification et **DNS** pour la localisation des services.
    

---

## 🏗️ Structure de l’Active Directory

### 🔷 Structure **logique**

|Élément|Description|
|---|---|
|**Forêt (Forest)**|Ensemble de domaines partageant un schéma et un catalogue global.|
|**Arborescence (Tree)**|Hiérarchie de domaines dans une forêt.|
|**Domaine**|Unité principale d'administration et de sécurité.|
|**OU (Organizational Unit)**|Conteneur logique pour organiser les objets (ex : par département).|
|**Objet**|Élément AD (utilisateur, ordinateur, imprimante…).|

### 🔶 Structure **physique**

|Élément|Description|
|---|---|
|**Contrôleur de domaine (DC)**|Serveur qui héberge l’annuaire.|
|**Site AD**|Regroupe des DC connectés par une liaison réseau rapide (LAN).|
|**Lien de site (Site Link)**|Décrit la connexion entre sites, utilisée pour la **réplication inter-site**.|

---

## 🗃️ Catalogue global (Global Catalog - GC)

- Contient un **sous-ensemble des attributs** de tous les objets de la forêt.
    
- Permet la **recherche rapide** dans tout AD (ex : via Outlook).
    
- Nécessaire pour **l’authentification dans une forêt multi-domaines**.
    
- Un DC peut être configuré comme **Global Catalog Server**.
    

---

## 🔁 Réplication

- **Intra-site** : fréquente et non compressée (réseau rapide).
    
- **Inter-site** : planifiée, compressée, basée sur la topologie définie dans les **sites AD**.
    
- Utilise le service **Knowledge Consistency Checker (KCC)** pour gérer les connexions.
    

---

## 🌐 Rôle du DNS

- AD repose fortement sur **DNS** pour localiser les services.
    
- Chaque domaine AD correspond à une **zone DNS**.
    
- Des **enregistrements SRV** sont créés pour localiser les DC, services LDAP, Kerberos…
    

---

## 🧩 Les 5 rôles FSMO (Flexible Single Master Operation)

Certains rôles ne peuvent être assurés que par **un seul DC à la fois** dans une forêt ou un domaine.

|Rôle FSMO|Niveau|Description|
|---|---|---|
|**Schema Master**|Forêt|Gère les modifications du schéma AD.|
|**Domain Naming Master**|Forêt|Gère l’ajout/suppression de domaines dans la forêt.|
|**RID Master**|Domaine|Attribue les blocs de RID aux DC pour les SIDs.|
|**PDC Emulator**|Domaine|Sert de contrôleur principal pour les anciennes machines et la synchronisation horaire.|
|**Infrastructure Master**|Domaine|Met à jour les références d’objets inter-domaines.|

---

## 🗂️ OU (Unité d'organisation)

- Permet d’**organiser les objets** du domaine pour appliquer des GPO ou déléguer des droits.
    
- Peut contenir : utilisateurs, ordinateurs, groupes, autres OU.
    
- Contrairement aux groupes, une **OU ne donne pas de permissions**.
    
- Hiérarchie illimitée (mais à garder simple pour la lisibilité).
    

Exemple :

```
OU "Utilisateurs"
├── OU "RH"
├── OU "Informatique"
│   └── OU "Admins"
```

---

## 📚 Règle AGDLP

> **Accounts → Global groups → Domain Local groups → Permissions**

- Utilisateurs → Groupes globaux → Groupes locaux → Droits sur ressources.
    
- Permet de **décorréler gestion des utilisateurs** et **gestion des ressources**.
    

---

## 🛠️ GPO (Group Policy Objects)

- **Appliquées à** : Sites, Domaines, OU.
    
- Ordre d’application : **Local → Site → Domaine → OU**
    
- Paramètres : restrictions, scripts, configurations logicielles...
    
- Outils : `gpedit.msc`, `gpmc.msc`, `gpupdate`, `rsop.msc`
    

---

## 🔐 Sécurité & Réplication

- **ACLs** pour le contrôle des droits.
    
- Réplication **multimaster** sauf pour les rôles FSMO.
    
- Audit possible via **les journaux d’événements**.
    

---

## ⚙️ Outils d’administration

|Outil|Fonction|
|---|---|
|**ADUC (dsa.msc)**|Gérer les objets AD.|
|**GPMC**|Créer, lier, tester les GPO.|
|**PowerShell**|Automatiser les tâches : `Get-ADUser`, `Set-ADGroup`, etc.|

---

## 🧠 Résumé graphique simplifié

```
Forêt : entreprise.local
└── Domaine : prod.entreprise.local
    ├── OU "Utilisateurs"
    │   └── Utilisateur : j.durand
    ├── OU "Informatique"
    │   ├── Utilisateur : admin.sarah
    │   └── Groupe global : GG_Informatique
    └── Ressource : Partage D:\Projets
        └── Groupe local : GL_Acces_Projets
```