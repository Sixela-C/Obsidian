---
up:
  - "[[]]"
---
## 🎯 Objectif :

Attribuer automatiquement des adresses IP aux machines du réseau local (par exemple, `192.168.1.100` à `192.168.1.200`), avec un serveur Linux configuré comme **DHCP**.

---

## ✅ 1. Installation du serveur DHCP

```bash
sudo apt update
sudo apt install isc-dhcp-server
```

---

## ✅ 2. Fichiers de configuration importants

|Fichier|Rôle|
|---|---|
|`/etc/dhcp/dhcpd.conf`|Fichier principal de configuration DHCP|
|`/etc/default/isc-dhcp-server`|Interface réseau utilisée par le serveur|

---

## ✅ 3. Configuration du DHCP

### ➤ 1. Interface à utiliser

```bash
sudo nano /etc/default/isc-dhcp-server
```

Modifie cette ligne :

```bash
INTERFACESv4="eth0"
```

> Remplace `eth0` par le nom réel de ton interface réseau (`ip a` pour le trouver, souvent `enp0s3`, `ens33`, etc.)

---

### ➤ 2. Fichier de configuration principal

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Exemple minimal pour un réseau `192.168.1.0/24` :

```conf
# Paramètres globaux
default-lease-time 600;
max-lease-time 7200;
authoritative;

# Sous-réseau à gérer
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
    option subnet-mask 255.255.255.0;
    option domain-name "monreseau.local";
    option domain-name-servers 192.168.1.10, 1.1.1.1;
}
```

---

### ➤ 3. Réservation statique (optionnel)

Toujours dans `dhcpd.conf`, tu peux réserver une IP pour un appareil :

```conf
host imprimante {
    hardware ethernet AA:BB:CC:DD:EE:FF;
    fixed-address 192.168.1.50;
}
```

---

## ✅ 4. Vérification de la configuration

```bash
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
```

---

## ✅ 5. Activer et démarrer le service

```bash
sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server
```

---

## ✅ 6. Logs et diagnostic

- Vérifie les logs :
    

```bash
journalctl -u isc-dhcp-server
```

- Voir les baux (adresses louées) :
    

```bash
cat /var/lib/dhcp/dhcpd.leases
```

---

## ➕ Bonus : Config pour plusieurs sous-réseaux (exemple)

```conf
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
}

subnet 192.168.2.0 netmask 255.255.255.0 {
    range 192.168.2.100 192.168.2.200;
    option routers 192.168.2.1;
}
```