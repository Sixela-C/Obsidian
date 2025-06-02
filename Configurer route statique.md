---
up:
  - "[[]]"
---
Entre 2 routeurs R1 et R2 :
Dans la configuration de la route statique sur R1 :
On indique le réseau 2 et son masque de sous-réseau,
On indique la passerelle d'accès à ce réseau sur le port du routeur R2 (réseau inter-routeur)

Dans la configuration de la route statique sur R2 :
On indique le réseau 1 et son masque de sous-réseau,
On indique la passerelle d'accès à ce réseau sur le port du routeur R1 (réseau inter-routeur)

Commande :
enable
conf t
ip route <adresse ip du réseau à joindre> <masque de sous réseau du réseau à joindre> <passerelle permetteant l'accès à ce réseau>
exit
