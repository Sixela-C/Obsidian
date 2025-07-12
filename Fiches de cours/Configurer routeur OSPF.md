---
up:
  - "[[]]"
---
Sur le routeur :
enable
conf t
router ospf 1
network <adresse réseau> <masque inversé> area <n°> (autant qu'il y a d'interface)
exit

Répéter sur les autres routeurs du réseau