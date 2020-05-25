---
layout: page
title: Documentation
permalink: /Doc/
---

# Projet AJC - groupe 2

Dans le cadre de notre stage d'une durée de 3 mois orienté sur les réseaux informatiques, nous avons souhaités rendre une documentation technique montrant les différentes étapes du projet final.  
Ce projet consiste en la mise en place d'une topologie selon un cahier des charges, des exigences techniques et des délais associés.

L'ensemble des opérations se déroule suivant un [planning](https://docs.google.com/spreadsheets/d/1zFA-i-1NdZ8Shb0r4ff5VIl1XpRT2mgDH1xgEIHK-KE/edit?ts=5ec284a0#gid=992991251) établi au préalable. 

# Sommaire

#### [Topologie](#Topo)
#### [Plan d'adressage Ipv4/Ipv6](#plan)
#### [Configuration de Ansible (playbooks)](#playbooks)
#### [Configuration d'un accès Internet avec Fortigate](#fortigate)
#### [Mise en place d'un site distant via un PVN IPSEC avec Fortigate](#vpn)
#### [Configuration des services d'infrastructures](#infra)
#### [Mise en place du monitoring (syslog)](#monitoring)
#### [Annexes](#annexes)

<a id="Topo"></a>
# Topologie

On présente la topologie choisie:

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/Topo_projet.png?raw=true) 

<a id="plan"></a>
# Plan d'adressage Ipv4/Ipv6 

Pour le plan d'adressage cliquez [ici](https://docs.google.com/spreadsheets/d/1zFA-i-1NdZ8Shb0r4ff5VIl1XpRT2mgDH1xgEIHK-KE/edit?ts=5ec284a0#gid=0).

<a id="playbooks"></a>
# Configuration de Ansible (playbooks)

<a id="fortigate"></a>
# Configuration d'un accès Internet avec Fortigate

<a id="vpn"></a>
# Mise en place d'un site distant via un PVN IPSEC avec Fortigate

L'objectif est de mettre en place un site distant qui communiquere avec l'infrastructure de base à travers un tunnel IPSEC. Selon la topologie, PC9 sera capable de communiquer avec PC1 à PC8 et inversement. 

Dans un premier temps on configure le réseau local de PC9: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/1.png?raw=true) 

Pour cela on configure le minimum sur R4 à savoir le nom du routeur, l'interface g0/0 en mode DHCP pour attribuer une adresse dynamiquement à PC9, un mot de passe pour le compte utilisé, une clé RSA ainsi que la version 2 de ssh. 

Une fois la configuration sauvegardé sur R4 on vérifie l'attribution dynamique d'une Ipv4 sur PC9: 

```
PC9> ip dhcp
DDORA IP 192.168.1.2/24
```

Il s'agit maintenant de mettre en place un tunnel IPSEC entre HQ et BRANCH: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/2.png?raw=true) 

On se rend sur l'interface de HQ, pour cela il faut récupérer l'adresse Ipv4 du port 2 (sur notre schéma): 

```
HQ # get system interface physical
        ==[port2]
                mode: dhcp
                ip: 192.168.122.51 255.255.255.0
                ipv6: ::/0
                status: up
                speed: 1000Mbps (Duplex: full)

```

En tapant l'adresse "192.168.122.51" dans notre navigateur Internet, on peut se connecter à l'interface de gestion de HQ, utilisez les identifiants fourni par votre administrateur. 

Se rendre dans l'onglet VPN>IPsec Wizard et configurer le tunnel tel que:  

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/3.png?raw=true) 



<a id="infra"></a>
# Configuration des services d'infrastructures

<a id="monitoring"></a>
# Mise en place du monitoring (syslog)

<a id="annexes"></a>
# Annexes

[Configuration de R1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R1.md) 

[Configuration de R2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R2.md) 

[Configuration de R3](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R3.md) 

[Configuration de AS1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/AS1.md) 

[Configuration de AS2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/AS2.md) 

[Configuration de DS1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/DS1.md) 

[Configuration de DS2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/DS2.md) 

