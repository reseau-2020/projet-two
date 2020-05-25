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
#### [Annexes](#annexe)

<a id="Topo"></a>
# Topologie

On présente la topologie choisie:

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/Topo_projet.png?raw=true) 

<a id="plan"></a>
# Plan d'adressage Ipv4/Ipv6 

Pour le plan d'adressage cliquez [ici](https://docs.google.com/spreadsheets/d/1zFA-i-1NdZ8Shb0r4ff5VIl1XpRT2mgDH1xgEIHK-KE/edit?ts=5ec284a0#gid=0).

| routeur | interface |       IPv4      |                           IPv6                          |         Description        |
|:-------:|:---------:|:---------------:|:-------------------------------------------------------:|:--------------------------:|
| R1      | g0/0      | 10.192.1.1/24   | fe80::1 2001:470:c814:2000::/64 FD00:470:c814:2000::/64 | LAN R1                     |
| R1      | g0/1      | 10.0.1.1        | fe80::cafe:2/64                                         | Passerelle vers l'internet |
| R1      | g0/2      | 10.192.225.1/24 | fe80::1                                                 | Connexion vers R2          |
| R1      | g0/3      | 10.192.226.1/24 | fe80::1                                                 | Connexion vers R3          |
| R2      | g0/0      | 10.192.33.1/24  | fe80::2 2001:470:c814:2001::/64 FD00:470:c814:2001::/64 | LAN R2                     |
| R2      | g0/1      | 10.192.225.2/24 | fe80::2                                                 | Connexion vers R1          |
| R2      | g0/3      | 10.192.227.1/24 | fe80::2                                                 | Connexion vers R3          | 
| R3      | g0/0      | 10.192.65.1/24  | FE80::3 2001:470:c814:2002::/64 FD00:470:c814:2002::/64 | LAN R3                     |
| R3      | g0/1      | 10.192.226.2/24 | fe80::3                                                 | Connexion vers R1          | 
| R3      | g0/2      | 10.192.227.2/24 | fe80::3                                                 | Connexion vers R2          | 

| routeur | interface | IPv4 | Description |
|----|------|-------------|-------------|
| R4 | g0/0 | 192.168.1.1 | LAN pour site distant |
| R4 | g0/1 | 192.168.2.1 | LAN pour site distant |


| R2-DS1 | G0/2 | 10.128.225.1/24 | fe80::2   |
|--------|------|-----------------|-----------|
|        | G0/4 | 10.128.224.1/24 | fe80::2   |
| R2-DS2 | G0/5 | 10.128.242.1/24 | fe80::3   |
|        | G0/6 | 10.128.243.1/24 | fe80::3   |
| R3-DS2 | G0/3 | 10.128.240.1/24 | fe80::3   |
|        | G0/4 | 10.128.241.1/24 | fe80::3   |
| R3-DS1 | G0/5 | 10.128.226.1/24 | fe80::2   |
|        | G0/6 | 10.128.227.1/24 | fe80::2   |
| DS1-R2 | g2/0 | 10.128.225.2/24 | FE80::D:2 |
|        | g3/0 | 10.128.224.2/24 | FE80::D:2 |
| DS1-R3 | g2/1 | 10.128.226.2/24 | FE80::D:2 |
|        | g3/1 | 10.128.227.2/24 | FE80::D:2 |
| DS2-R2 | g2/1 | 10.128.242.2/24 | FE80::D:3 |
|        | g3/1 | 10.128.243.2/24 | FE80::D:3 |
| DS2-R3 | g2/0 | 10.128.240.2/24 | FE80::D:3 |
|        | g3/0 | 10.128.241.2/24 | FE80::D:3 | 


| DS1 | VLAN10 | 10.128.10.252/24 | FE80::D:2 2001:470:c814:2A0A::2/64 FD00:470:c814:2A0A::2/64 |
|-----|--------|------------------|-------------------------------------------------------------|
| DS1 | VLAN20 | 10.128.20.252/24 | FE80::D:2 2001:470:c814:2A14::2/64 FD00:470:c814:2A14::2/64 |
| DS1 | VLAN30 | 10.128.30.252/24 | FE80::D:2 2001:470:c814:2AAE::2/64 FD00:470:c814:2AAE::2/64 |
| DS1 | VLAN40 | 10.128.40.252/24 | FE80::D:2 2001:470:c814:2A28::2/64 FD00:470:c814:2A28::2/64 |
| DS2 | VLAN10 | 10.128.10.253/24 | FE80::D:3 2001:470:c814:2A0A::3/64 FD00:470:c814:2A0A::3/64 |
| DS2 | VLAN20 | 10.128.20.253/24 | FE80::D:3 2001:470:c814:2A14::3/64 FD00:470:c814:2A14::3/64 |
| DS2 | VLAN30 | 10.128.30.253/24 | FE80::D:3 2001:470:c814:2AAE::3/64 FD00:470:c814:2AAE::3/64 |
| DS2 | VLAN40 | 10.128.40.253/24 | FE80::D:3 2001:470:c814:2A28::3/64 FD00:470:c814:2A28::3/64 | 


| Peripherique |  VLAN  |   IPv4 virtuel   |                            IPv6                           |
|:------------:|:------:|:----------------:|:---------------------------------------------------------:|
| DS1          | VLAN10 | 10.128.10.254/24 | FE80::D:1 2001:470:c814:2A0A::/64 FD00:470:c814:2A0A::/64 |
| DS1          | VLAN20 | 10.128.20.254/24 | FE80::D:1 2001:470:c814:2A14::/64 FD00:470:c814:2A14::/64 |
| DS1          | VLAN30 | 10.128.30.254/24 | FE80::D:1 2001:470:c814:2AAE::/64 FD00:470:c814:2AAE::/64 |
| DS1          | VLAN40 | 10.128.40.254/24 | FE80::D:1 2001:470:c814:2A28::/64 FD00:470:c814:2A28::/64 |
| DS2          | VLAN10 | 10.128.10.254/24 | FE80::D:2 2001:470:c814:2A0A::/64 FD00:470:c814:2A0A::/64 |
| DS2          | VLAN20 | 10.128.20.254/24 | FE80::D:2 2001:470:c814:2A14::/64 FD00:470:c814:2A14::/64 |
| DS2          | VLAN30 | 10.128.30.254/24 | FE80::D:2 2001:470:c814:2AAE::/64 FD00:470:c814:2AAE::/64 |
| DS2          | VLAN40 | 10.128.40.254/24 | FE80::D:2 2001:470:c814:2A28::/64 FD00:470:c814:2A28::/64 | 



<a id="playbooks"></a>
# Configuration de Ansible (playbooks) 

L'ensemble sera lancé depuis un poste "controller" (voir topologie) 

Pour adapter les livres de jeux à une nouvelle configuration il faut intervenir sur les fichiers R1, R2, R3, AS1, AS2, DS1 et DS2 du répertoire  **/playbooks/inventories/ccna/host_var/** 

Il faut prendre soin de mettre à jour les adresses Ipv4 et Ipv6 ainsi que l'interface de sortie du routeur R1 (G0/1), par défaut celle ci est connectée à l'internet et on la laissera en mode DHCP afin d'obtenir dynamiquement une adresse. 

Dans notre cas l'interface G0/1 est attribué statiquement car c'est notre fortigate HQ qui est connecté à l'internet, soit sur R1: 

```
id: GigabitEthernet0/1
    description: "Connexion to HQ"
    ipv4_address: 10.0.1.1/24
```

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
Récupérer également celle de BRANCH qui servira pour la configuration du tunnel: 

```
HQ # get system interface physical
        ==[port2]
                mode: dhcp
                ip: 192.168.122.195 255.255.255.0
                ipv6: ::/0
                status: up
                speed: 1000Mbps (Duplex: full)
```

En tapant l'adresse **"192.168.122.51"** dans notre navigateur Internet, on peut se connecter à l'interface de gestion de HQ, utilisez les identifiants fourni par votre administrateur. 

Se rendre dans l'onglet **VPN>IPsec Wizard** et configurer le tunnel tel que:  

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/3.png?raw=true) 

Ici on utilise l'adresse distance à laquelle se connecter via le port 2 (WAN), il s'agit donc de l'interface du port 2 de BRANCH: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/4.png?raw=true) 

Pour la dernière étape on configure le sous réseau local et distant, suivant notre topologie: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/5.png?raw=true) 

A présent on configure BRANCH de la même manière en prenant soin d'indiquer les bonnes adresses Ipv4 ainsi que les réseaux locaux correspondants. 
Une fois cette opération effectuée, on peut tester la connectivité entre PC9 et PC1 par exemple: 

```
PC9> ip dhcp
DDORA IP 192.168.1.2/24
```

```
PC1> ip dhcp
DDORA IP 10.128.10.51/24
```
Et donc: 

```
PC9> ping 10.128.10.51
84 bytes from 10.128.10.51 icmp_seq=1 ttl=58 time=9.578 ms
84 bytes from 10.128.10.51 icmp_seq=2 ttl=58 time=9.590 ms
```
La connectivité est bien établie. 

**Remarque importante**: l'adresse Ipv4 définie sur les ports 2 respectifs de HQ et BRANCH est définie dynamiquement par DHCP, il se peut donc qu'elle soit ammenée à changer !

<a id="infra"></a>
# Configuration des services d'infrastructures

<a id="monitoring"></a>
# Mise en place du monitoring (syslog)

<a id="annexe"></a>
# Annexes

[Configuration de R1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R1.md) 

[Configuration de R2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R2.md) 

[Configuration de R3](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R3.md) 

[Configuration de AS1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/AS1.md) 

[Configuration de AS2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/AS2.md) 

[Configuration de DS1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/DS1.md) 

[Configuration de DS2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/DS2.md) 

