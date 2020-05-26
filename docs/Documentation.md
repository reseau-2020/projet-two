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

#### [1.Topologie](#Topo)
#### [2.Plan d'adressage Ipv4/Ipv6](#plan)
#### [3.Configuration de Ansible (playbooks)](#playbooks)
#### [4.Configuration d'un accès Internet avec Fortigate](#fortigate)
#### [5.Mise en place d'un site distant via un PVN IPSEC avec Fortigate](#vpn)
#### [6.Configuration des services d'infrastructures (DNS)](#infra)
#### [7.Tests de fiabilité](#test)
#### [8.Mise en place du monitoring (syslog)](#monitoring)
#### [9.Annexes](#annexe)

<a id="Topo"></a>
# 1.Topologie

On présente la topologie choisie:

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/Topo_projet.png?raw=true) 

<a id="plan"></a>
# 2.Plan d'adressage Ipv4/Ipv6 

Pour le plan d'adressage cliquez [ici](https://docs.google.com/spreadsheets/d/1zFA-i-1NdZ8Shb0r4ff5VIl1XpRT2mgDH1xgEIHK-KE/edit?ts=5ec284a0#gid=0).

| routeur | interface |       IPv4      |                           IPv6                          |         Description        |
|:-------:|:---------:|:---------------:|:-------------------------------------------------------:|:--------------------------:|
| R1      | g0/0      | 10.192.1.1/24   | fe80::1 2001:470:c814:2000::/64 FD00:470:c814:2000::/64 | LAN R1                     |
| R1      | g0/1      | 10.0.1.1        | fe80::cafe:2/64                                         | Connexion vers HQ |
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
| R4 | g0/1 | 192.168.2.1 | Connexion vers BRANCH |


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
# 3.Configuration de Ansible (playbooks) 

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
# 4.Configuration d'un accès Internet avec Fortigate

Pour cela il faut s'assurer que la sortie de R1 vers HQ ne soit pas configuré en NAT, dans notre cas nous avons définie l'adresse Ipv4 **10.0.1.1** de manière statique sur cette interface. 

Schéma simplifié de l'infrastructure: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_Wan/4.jpg?raw=true)

Se rendre sur l'interface de HQ, avec la commande "get system interface physical" on récupère l'adresse Ipv4:

```
HQ # get system interface physical
        ==[port2]
                mode: dhcp
                ip: 192.168.122.51 255.255.255.0
                ipv6: ::/0
                status: up
                speed: 1000Mbps (Duplex: full)

```

Dans l'onglet **Network>Interfaces**, on configure la liaison entre R1 et HQ, ici c'est le port 3: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_Wan/1.jpg?raw=true)

Pour donner l'accès à l'internet sur HQ on s'intéresse au port 2: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_Wan/2.jpg?raw=true)

Il faut également définir une route statique, se rendre dans l'onglet **Network>Static Routes**:

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_Wan/3.jpg?raw=true) 

On peut faire un simple test fonctionnel avec la commande "ping" depuis PC1 vers l'internet:

```
PC1> ping 1.1.1.1
84 bytes from 1.1.1.1 icmp_seq=1 ttl=55 time=8.565 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=55 time=8.845 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=55 time=9.809 ms
```


<a id="vpn"></a>
# 5.Mise en place d'un site distant via un PVN IPSEC avec Fortigate

L'objectif est de mettre en place un site distant qui communiquere avec l'infrastructure de base à travers un tunnel IPSEC. Selon la topologie, PC9 sera capable de communiquer avec PC1 à PC8 et inversement. 

Dans un premier temps on configure le réseau local de PC9: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/1.png?raw=true) 

Pour cela on configure le minimum sur R4 à savoir le nom du routeur, l'interface g0/0 en mode DHCP pour attribuer une adresse dynamiquement à PC9, un mot de passe pour le compte utilisé, une clé RSA ainsi que la version 2 de ssh. 

Voir la configuration de R4 en annexe, [Configuration de R4](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R4.md)  
Il faut également définir une route statique entre Branch et le routeur R4, on récupère l'adresse de BRANCH:  

```
HQ # get system interface physical
        ==[port2]
                mode: dhcp
                ip: 192.168.122.195 255.255.255.0
                ipv6: ::/0
                status: up
                speed: 1000Mbps (Duplex: full)
```

Se rendre dans l'onglet **Network>Static Routes** et définir la route statique suivant la topologie, ici le port 3: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_Wan/branchStatic.jpg?raw=true) 

On peut maintenant vérifier l'attribution dynamique d'une Ipv4 sur PC9: 

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

En tapant l'adresse **"192.168.122.51"** dans notre navigateur Internet, on peut se connecter à l'interface de gestion de HQ, utilisez les identifiants fourni par votre administrateur. 

Se rendre dans l'onglet **VPN>IPsec Wizard** et configurer le tunnel tel que:  

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/3.png?raw=true) 

Ici on utilise l'adresse distance à laquelle se connecter via le port 2 (WAN), il s'agit donc de l'interface du port 2 de BRANCH: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/4.png?raw=true) 

Pour la dernière étape on configure le sous réseau local et distant, suivant notre topologie: 

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_VPN/5.png?raw=true) 

A présent on configure BRANCH de la même manière en prenant soin d'indiquer les bonnes adresses Ipv4 ainsi que les réseaux locaux correspondants. 

Ne pas oublier d'activer le tunnel, dans l'onglet **Monitor>IPsec Monitor**, faire un clique droit sur la configuration et selectionner **Bring Up**. 

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
# 6.Configuration des services d'infrastructures

### 6.1.Service de résolution de domaine (DNS)

Pour mettre en place la résolution de domaine, on se rend sur DS1 et DS2. Sur chaque pool de VLAN il faut attribuer le serveur DNS (on choisira 8.8.8.8)

```
DS1(config)#dhcp pool VLAN10
DS1(dhcp-config)#dns-server 8.8.8.8

DS1(config)#dhcp pool VLAN20
DS1(dhcp-config)#dns-server 8.8.8.8

DS1(config)#dhcp pool VLAN30
DS1(dhcp-config)#dns-server 8.8.8.8

DS1(config)#dhcp pool VLAN40
DS1(dhcp-config)#dns-server 8.8.8.8
```
De même sur DS2:

```
DS2(config)#dhcp pool VLAN10
DS2(dhcp-config)#dns-server 8.8.8.8

DS2(config)#dhcp pool VLAN20
DS2(dhcp-config)#dns-server 8.8.8.8

DS2(config)#dhcp pool VLAN30
DS2(dhcp-config)#dns-server 8.8.8.8

DS2(config)#dhcp pool VLAN40
DS2(dhcp-config)#dns-server 8.8.8.8
```

Pour tester on peut effectuer un "ping" depuis un poste de travail vers "www.google.com": 

```
PC1> ping www.google.com
www.google.com resolved to 216.58.209.228
84 bytes from 216.58.209.228 icmp_seq=1 ttl=52 time=8.252 ms
84 bytes from 216.58.209.228 icmp_seq=2 ttl=52 time=8.219 ms
84 bytes from 216.58.209.228 icmp_seq=3 ttl=52 time=9.246 ms
```

La résolution de nom est bien fonctionelle.


<a id="test"></a>
# 7.Tests de fiabilité

### 7.1.Spanning tree

Pour rappel DS1 est le root bridge pour vlan 10 et vlan 30 et DS2 est root bridge pour vlan 20 et 40.

En cas de rupture d’une des liaisons, Spanning-tree offre un chemin alternatif via le second switch de couche Distribution. 

En désignant l’un des switchs comme “root primary” pour certains VLANs et “root secondary” pour d’autres, et inversément pour l’autre switchc de Distribution, on arrive à répartir la charge du trafic sur les Trunks et à offrir un mécanisme de reprise sur erreur. 

Sur notre topologie, prenons l'exemple du VLAN10:

```
DS1(config)#do sh spanning-tree vlan 10

VLAN0010
  Spanning tree enabled protocol rstp
  Root ID    Priority    24586
             Address     0cad.663d.1100
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    24586  (priority 24576 sys-id-ext 10)
             Address     0cad.663d.1100
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Po3                 Desg FWD 3         128.66   P2p
Po5                 Desg FWD 3         128.67   P2p
```

Sur DS2:

```
DS2#sh spanning-tree vlan 10

VLAN0010
  Spanning tree enabled protocol rstp
  Root ID    Priority    24586
             Address     0cad.663d.1100
             Cost        3
             Port        66 (Port-channel3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    28682  (priority 28672 sys-id-ext 10)
             Address     0cad.6659.f400
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Po2                 Desg FWD 3         128.65   P2p
Po3                 Root FWD 3         128.66   P2p
Po4                 Desg FWD 3         128.67   P2p
```

On remarque que pour VLAN10, DS1 est le root -> **This bridge is the root**.

En faisant tomber l’interface entre AS1 et DS1, on remarque que le ping (PC1 vers PC5) s’interrompt quelques instants pour récupérer la liaison. En vérifiant sur DS1 on remarque que celui-ci est toujours root. 

Test:

```
PC1> ping 10.128.10.52 -t
84 bytes from 10.128.10.52 icmp_seq=1 ttl=64 time=5.930 ms
84 bytes from 10.128.10.52 icmp_seq=2 ttl=64 time=5.887 ms
84 bytes from 10.128.10.52 icmp_seq=3 ttl=64 time=6.427 ms
``` 

Pendant le ping on coupe la liaison entre DS1 et AS1:

```
DS1(config)#int g0/0
DS1(config-if)#shutdown
DS1(config-if)#int g1/0
DS1(config-if)#shutdown
```

En retournant sur PC5 on remarque que le ping s'interrompt quelques instants avant de reprendre la connectivité: 

```
PC1> ping 10.128.10.52 -t
84 bytes from 10.128.10.52 icmp_seq=1 ttl=64 time=5.930 ms
84 bytes from 10.128.10.52 icmp_seq=2 ttl=64 time=5.887 ms
84 bytes from 10.128.10.52 icmp_seq=3 ttl=64 time=6.427 ms
10.128.10.52 icmp_seq=27 timeout
10.128.10.52 icmp_seq=28 timeout
10.128.10.52 icmp_seq=29 timeout
10.128.10.52 icmp_seq=30 timeout
10.128.10.52 icmp_seq=31 timeout
84 bytes from 10.128.10.52 icmp_seq=32 ttl=64 time=7.585 ms
84 bytes from 10.128.10.52 icmp_seq=33 ttl=64 time=7.883 ms
84 bytes from 10.128.10.52 icmp_seq=34 ttl=64 time=7.732 ms
```

Pour résumer l'opération:

![image](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/_tests/5.jpg?raw=true) 


<a id="monitoring"></a>
# 8.Mise en place du monitoring (syslog)

<a id="annexe"></a>
# 9.Annexes

[Configuration de R1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R1.md) 

[Configuration de R2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R2.md) 

[Configuration de R3](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R3.md) 

[Configuration de R4](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/R4.md) 

[Configuration de AS1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/AS1.md) 

[Configuration de AS2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/AS2.md) 

[Configuration de DS1](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/DS1.md) 

[Configuration de DS2](https://github.com/reseau-2020/projet-two/blob/master/docs/_annexes/DS2.md) 

