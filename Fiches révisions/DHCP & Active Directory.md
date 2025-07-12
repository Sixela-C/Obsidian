---
up:
  - "[[Fiches révisions]]"
---
---
## 1. Qu’est-ce que le DHCP ?
### ➤ **DHCP (Dynamic Host Configuration Protocol)** :
- C’est un protocole qui permet à un poste client d’obtenir automatiquement une **adresse IP** et d’autres **paramètres réseau** (masque, passerelle, DNS, etc.) quand il se connecte au réseau.
- Attribue automatiquement :
    - Adresse IP
    - Masque
    - Passerelle
    - DNS, etc.

---
## 2. Rôle du serveur DHCP :

Le **serveur DHCP attribue dynamiquement** les adresses IP aux clients.
Il fournit aussi :
- **Masque de sous-réseau**
- **Passerelle par défaut (gateway)**
- **Serveur DNS**
- (optionnel) **Nom de domaine**, **serveur WINS**, etc.

---
## 3. DHCP et Active Directory :
### ➤ Pourquoi intégrer DHCP dans un domaine AD ?
- Pour **sécuriser** l’attribution des adresses IP.
- Le serveur DHCP doit être **autorisé dans AD** pour pouvoir fonctionner dans un environnement sécurisé.

### Sécurité : autorisation DHCP dans AD
- Quand un serveur DHCP est **membre d’un domaine**, il doit être **autorisé dans AD** pour prévenir les **DHCP non autorisés** (rogues).
- Cela se fait via la **console DHCP** ou la commande PowerShell :
```powershell
Add-DhcpServerInDC -DnsName "srv-dhcp.entreprise.local" -IPAddress 192.168.1.10
```

| Élément                   | Description                                          |
| ------------------------- | ---------------------------------------------------- |
| **Serveur DHCP autorisé** | Nécessaire pour distribuer des IP dans un domaine AD |
| **Sécurité DHCP**         | AD vérifie que le serveur est bien autorisé          |
| **DHCP + DNS**            | Le serveur DHCP peut fournir l’IP du serveur DNS AD  |

---
## 4. Étapes du processus DHCP (DORA) :

|Étape|Rôle|
|---|---|
|**Discover**|Le client cherche un serveur DHCP (broadcast)|
|**Offer**|Le serveur propose une IP|
|**Request**|Le client demande officiellement l’IP proposée|
|**Ack**|Le serveur confirme l’attribution|

```plaintext
        +----------------------+
        | Poste Client         |
        | (PC sans IP)         |
        +----------+-----------+
                   |
       (1) Discover (diffusion)
                   |
                   v
        +----------------------+
        | Serveur DHCP         |
        | (srv-dhcp)           |
        +----------+-----------+
                   |
       (2) Offer (propose IP)
                   |
                   v
        +----------------------+
        | Client               |
                   |
       (3) Request (demande IP)
                   |
                   v
        +----------------------+
        | DHCP                 |
                   |
       (4) Acknowledgement (confirmation)
```

---
## 5. Configuration typique :

|Élément|Exemple|
|---|---|
|**Plage IP**|192.168.1.100 à 192.168.1.200|
|**Bail DHCP**|Durée pendant laquelle l’IP est louée|
|**Options DHCP**|003 = Gateway, 006 = DNS, 015 = Domaine|
|**Réservations**|Attribuer une IP fixe à une MAC|

---
## 5. Pièges à éviter :

- Ne pas **autoriser le serveur DHCP** dans AD.
- Ne pas configurer **les options DNS correctement**.
- Laisser **plusieurs serveurs DHCP non contrôlés** sur le réseau (risque de conflit d’IP).

---
## 6. En résumé :

- Le **serveur DHCP doit être autorisé dans AD**.
- Il fournit automatiquement les **paramètres réseau**.
- Il facilite la gestion **centralisée des IP**.
- Il complète le **serveur DNS** pour qu’un client trouve un DC et se connecte au domaine.

