---
up:
  - "[[]]"
---
## 🔎 1. Principe d’Active Directory

### ➤ Définition :

Active Directory (AD) est un **service d’annuaire** de Microsoft permettant de :

- Centraliser la **gestion des utilisateurs, ordinateurs, groupes et ressources** du réseau.
    
- Authentifier les utilisateurs via un **contrôleur de domaine** (DC).
    
- Appliquer des **stratégies de sécurité et de configuration** via des **GPO**.
    

➡️ Objectif : **Simplifier la gestion** et **sécuriser** l’environnement réseau de l’entreprise.

---

## 🧱 2. Architecture d’Active Directory

### ➤ Éléments logiques :

|Élément|Rôle|
|---|---|
|**Forêt (Forest)**|Ensemble de domaines partageant un schéma commun.|
|**Domaine (Domain)**|Unité principale d’administration (ex : `entreprise.local`).|
|**OU (Unité d’Organisation)**|Conteneur logique pour organiser les objets (ex : `OU=Informatique`).|
|**Objets AD**|Utilisateurs, ordinateurs, groupes, imprimantes…|

### ➤ Éléments physiques :

|Élément|Rôle|
|---|---|
|**Contrôleur de Domaine (DC)**|Serveur hébergeant la base de données AD (`NTDS.dit`).|
|**Site AD**|Répartition géographique pour optimiser la réplication.|

---

## 👤 3. Les **Utilisateurs**

- Représentent les **comptes des personnes** (employés, techniciens...).
    
- Chaque utilisateur possède :
    
    - Un **identifiant (login)** et un **mot de passe**.
        
    - Un **profil** (stocké localement ou sur le serveur).
        
- Il est possible de forcer certaines politiques (mot de passe, répertoire personnel…).
    

🔒 L’utilisateur s’authentifie auprès du DC à chaque connexion au domaine.

---

## 💻 4. Les **Ordinateurs**

- Chaque machine du réseau peut être **jointe au domaine**.
    
- Cela permet :
    
    - L’application de **GPO spécifiques**.
        
    - L’accès centralisé aux ressources du domaine.
        
- Un objet ordinateur est créé dans l’AD (souvent dans une OU dédiée : `OU=Postes`).
    

---

## 👥 5. Les **Groupes**

- **Objectif** : faciliter la gestion des droits et des autorisations.
    

### Types :

|Type de groupe|Utilité|
|---|---|
|**Groupe de sécurité**|Attribuer des droits (accès à un dossier, à une imprimante).|
|**Groupe de distribution**|Pour les listes de diffusion (email).|

### Portée :

|Niveau|Utilisation recommandée|
|---|---|
|**Global**|Regrouper des utilisateurs d’un domaine|
|**Local**|Attribuer des droits à des ressources locales|
|**Universel**|Utilisation multi-domaines (cas complexe)|

---

## 🧩 6. Les **OU – Unités d’Organisation**

- Permettent de **structurer** l’AD de manière logique (par service, site, fonction…).
    
- Exemple : `OU=RH`, `OU=Informatique`, `OU=Comptabilité`.
    
- Elles servent à :
    
    - Organiser les **objets AD**.
        
    - **Appliquer des GPO ciblées**.
        
    - Déléguer la gestion à certains utilisateurs (admin de l’OU).
        

---

## 📋 7. Les **GPO – Stratégies de Groupe**

### ➤ Rôle :

Les GPO permettent d’**automatiser la configuration** des postes ou des utilisateurs du domaine.

### Exemples d’utilisation :

- Interdire l’accès au panneau de configuration.
    
- Déployer un fond d’écran ou une application.
    
- Configurer les options de mot de passe.
    

### ➤ Application des GPO :

|Cible|Priorité|
|---|---|
|**Site**|Faible|
|**Domaine**|Moyenne|
|**OU**|Forte (la plus utilisée)|

⚠️ Les GPO sont **héritées**, mais peuvent être **bloquées ou filtrées** si besoin.

---

## 📌 À retenir absolument

|Élément clé|Indispensable|
|---|---|
|Active Directory|Gère tous les objets du réseau Windows|
|Domaine|Unité logique d’authentification|
|DC|Serveur maître de l’annuaire|
|OU|Organisation des objets + GPO|
|Utilisateur|Objet d’authentification|
|Ordinateur|Objet contrôlé par AD|
|Groupe|Simplifie la gestion des droits|
|GPO|Applique des règles automatiquement|
