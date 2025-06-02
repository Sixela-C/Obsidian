---
up: []
---
Sur le switch :
vlan <n°>
name vlan-<n°>

interface <n° interface entre switch et vlan>
switchport mode access
switchport access vlan <n°>

interface <n° interface entre switch et routeur>
switchport mode trunk

Sur le routeur :
interface <n° interface>.<n° vlan>
encapsulation dot1Q <n° vlan>
ip address <adresse passerelle du vlan sur le routeur> <masque de sous-réseau du vlan>
no shutdown
exit

Sur le client :
gateway = adresse passerelle du vlan sur le routeur