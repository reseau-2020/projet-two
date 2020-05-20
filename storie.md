# Détail de la journée du 18 05 2020


Re lecture du sujet et des objectifs

Initiation du planning.

Mise en place d'un plan d'adressage en ipv4/ipv6, à suivre le 19/05.


# Détail de la journée du 19 05 2020


Pour définir le nouveau plan d'adressage ipv4/ipv6, François nous a donné un bloc à découper sous numéro de réseau 10.0.0.0/8 :
/8 = 255.0.0.0 [32- 8 = 24 bits à zéro dans le masque à 2exp24 adresses possibles]
Nombre magique = 256-255 = 1 Quel est le multiple de 1 inférieur ou égal 10 ? Numéro de réseau : 10. Numéro de broadcast : 1+10-1 = .10.255.255 donc Plage : 10.0.0.1 à 10.255.255.254

Nous avons décidé arbitrairement de donner un réseau en 10.192.X.X pour le tripod et un réseau en 10.128.X.X pour le switchblock. En effet, les livres de jeux ansible existants sont configurés dans un réseau en 192.168.X.X pour le tripod et un réseau en 172.16.X.X pour le switchblock. Il suffira donc de remplacer « 192.168 » par « 10.192 » et « 172.16 » par « 10.128 ».

Ensuite, nous décidons de configurer les adresses des périphériques, interfaces et vlan.
Chaque périphérique aura une adresse publique et une adresse privée.
Chaque routeur a une adresse link-local sur chacune de ses interfaces.(ex : fe80 ::1, fe80 ::2 et fe80 ::3).
Idem pour chaque commutateur.(ex : fe80 ::d :1).
Chaque vlan doit avoir une adresse publique et une privée.
François nous a donné un bloc d’adresses des réseaux maîtrisés, routé jusqu'à notre routeur externe (donc jusqu’à g0/1 de R1): 2001:470:c814:2000::/52.

Vu qu’on a divisé ce bloc en /64, la variance porte 2001:470:c814:2 ???::/64. (Sur les 3 derniers hexa)
On peut renseigner avec n’importe quelle valeurs les adresses privées de ces vlan.
Or, une bonne pratique consiste à repérer le vlan sur cette adresse privée.

Avec le site http://sebastienguillon.com/test/javascript/convertisseur.html, on calcule la conversion décimale – héxadécimale.
Par exemple, le vlan 10 aura les adresses suivantes :
2001:470:c814:2A0A::/64 et FD00:470:c814:2A0A::/64. 0A est 10.
La lettre A dans 2A0A désigne le switchblock.

Par contre, « notre convention » implique une contrainte : on ne pourra identifier le vlan dans l’adresse que jusqu’à 255 (soit FF en héxadécimal) car nous avons la place de la variance que pour 2 hexa. Par exemple, vlan300 (12C) ne pourra pas s’écrire avec notre convention.


Planning à jour:

https://docs.google.com/spreadsheets/d/1zFA-i-1NdZ8Shb0r4ff5VIl1XpRT2mgDH1xgEIHK-KE/edit?ts=5ec284a0#gid=992991251

Plan d'adressage IPV4/IPV6:

https://docs.google.com/spreadsheets/d/1zFA-i-1NdZ8Shb0r4ff5VIl1XpRT2mgDH1xgEIHK-KE/edit?ts=5ec284a0#gid=0

Finalement, avec le réseau 2001:470:c814:2000::/52, on manque d’espace.
Un /48 aurait été plus approprié si on veut identifier l’écriture des vlan jusqu’à 10 bit.

Les interfaces physiques sur AS1 et AS2 ne prennent pas d’adresse IP.

Il y a une adresse IP commune et virtuelle choisie par le protocole HSRP est fe80 ::d :1.

Les vlans étant dans le même bloc, chacune de ces interfaces devraient trouver une adresse publique et privée en IPv6.

L’enjeu sera de faire évoluer le playbook ccna.yml afin d’y ajouter la fonctionnalité DHCPv6 qui attribuera des adresses IPv6 en autoconfig. Ici 2001:470…et FD00:470… généré à partir de l’adresse MAC.
Par exemple, sur PC1 avec la commande « nmtui », 7 adresses IP seront configuées :
1 adresse IP link local
3 adresses IP publiques : 1 en autoconfig sous windows et 1 attribuée par DHCPv6.
3 adresses IP privées.

Des commandes de vérification (ping -6) permettront de valider la config ipv6.
