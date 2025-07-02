---
up:
  - "[[Fiches révisions]]"
---
## 🎯 Objectif :

Mettre en place un serveur DNS local avec **Bind9**, pour résoudre un domaine personnalisé (ex: `monreseau.local`).

---

## ✅ 1. Installation de Bind9

```bash
sudo apt update
sudo apt install bind9 bind9utils bind9-doc dnsutils
```

---

## ✅ 2. Fichiers de configuration principaux

| Fichier                              | Rôle                                              |
| ------------------------------------ | ------------------------------------------------- |
| `/etc/bind/named.conf`               | Fichier principal, inclut les autres.             |
| `/etc/bind/named.conf.options`       | Options globales (transfert DNS, écoute, etc.)    |
| `/etc/bind/named.conf.local`         | Définition des zones locales.                     |
| `/etc/bind/named.conf.default-zones` | Zones par défaut (`localhost`, `127.0.0.1`, etc.) |
| `/var/cache/bind/db.monreseau.local` | Fichier de zone directe                           |
| `/var/cache/bind/db.192.168.1`       | Fichier de zone inverse                           |

---

## ✅ 3. Configuration : zone directe et inverse

### ➤ `/etc/bind/named.conf.options`

```bash
sudo nano /etc/bind/named.conf.options
```

Exemple :

```conf
options {
    directory "/var/cache/bind";

    recursion yes;
    allow-query { any; };

    forwarders {
        1.1.1.1;
        8.8.8.8;
    };

    dnssec-validation auto;

    listen-on { any; };
    listen-on-v6 { any; };
};
```

---

### ➤ `/etc/bind/named.conf.local`

```bash
sudo nano /etc/bind/named.conf.local
```

Ajoute :

```conf
zone "monreseau.local" {
    type master;
    file "/var/cache/bind/db.monreseau.local";
};

zone "1.168.192.in-addr.arpa" {
    type master;
    file "/var/cache/bind/db.192.168.1";
};
```

---

## ✅ 4. Création des fichiers de zone

### ➤ Zone directe : `/var/cache/bind/db.monreseau.local`

```bash
sudo nano /var/cache/bind/db.monreseau.local
```

Exemple :

```conf
$TTL    86400
@       IN      SOA     ns1.monreseau.local. admin.monreseau.local. (
                        2025062501 ; Serial
                        3600       ; Refresh
                        1800       ; Retry
                        1209600    ; Expire
                        86400 )    ; Negative Cache TTL

@       IN      NS      ns1.monreseau.local.
@       IN      A       192.168.1.10
ns1     IN      A       192.168.1.10
pc1     IN      A       192.168.1.11
```

---

### ➤ Zone inverse : `/var/cache/bind/db.192.168.1`

```bash
sudo nano /var/cache/bind/db.192.168.1
```

Exemple :

```conf
$TTL    86400
@       IN      SOA     ns1.monreseau.local. admin.monreseau.local. (
                        2025062501 ; Serial
                        3600       ; Refresh
                        1800       ; Retry
                        1209600    ; Expire
                        86400 )    ; Negative Cache TTL

@       IN      NS      ns1.monreseau.local.
10      IN      PTR     ns1.monreseau.local.
11      IN      PTR     pc1.monreseau.local.
```

---

## ✅ 5. Vérification de la config

```bash
sudo named-checkconf
sudo named-checkzone monreseau.local /var/cache/bind/db.monreseau.local
sudo named-checkzone 1.168.192.in-addr.arpa /var/cache/bind/db.192.168.1
```

---

## ✅ 6. Redémarrage de Bind9

```bash
sudo systemctl restart bind9
sudo systemctl enable bind9
```

---

## ✅ 7. Tester avec `dig` ou `nslookup`

```bash
dig @127.0.0.1 pc1.monreseau.local
dig -x 192.168.1.11
```
