---
up:
  - "[[]]"
---
Sur le routeur 1 :
enable
conf t
router ospf 1
network <adresse réseau> <masque inversé> area <n°> (autant qu'il y a d'interface)

Répéter sur les autres routeurs du réseau