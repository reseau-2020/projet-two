---
layout: post
title:  "Article pour commencer"
date:   2020-05-14
categories: welcome
---

Les sources du site web se situent dans le `docs/`. Il est nécessaire d'effectuer deux choses pour le faire fonctionner :

- modifier le nom du repo dans le `fichier docs/_config.yml` : `baseurl: "/votre-nom-de-repo"`
- Activer la fonction Github Pages dans les propriétés ("settings") du repo et choisir la source "Master Branch /docs"

![](https://github.blog/wp-content/uploads/2016/08/d516076e-640c-11e6-8086-ce1d246a87d2.png?w=1460)

Vous trouverez ce poste dans votre répertoire `docs/_posts/`. Allez-y, éditez-le et reconstruisez le site pour voir vos changements. Il vous suffira de mettre à jour votre repo git pour que le site suite publié automatiquement sur l'url https://reseau-2020.github.io/votre-nom-de-repo.

Jekyll exige que les fichiers d'articles de blog soient nommés selon le format suivant :

```
ANNÉE-MOIS-JOUR-titre.MARKUP
```

Où ANNÉE est un nombre à quatre chiffres, MOIS et JOUR sont tous deux des nombres à deux chiffres, et MARKUP est l'extension de fichier représentant le format utilisé dans le fichier. Après cela, incluez le frontispice nécessaire. Jetez un coup d'œil à la source de ce billet pour vous faire une idée de son fonctionnement.

Voici un exemple de snippet yaml

```yaml
---
hostname: R1
interfaces:
  - id: GigabitEthernet0/0
    description: "LAN R1"
    ipv4_address: 192.168.1.1/24
    ipv6_addresses:
      - 'FE80::1'
      - '2001:DB8:1AB:1::1/64'
    passive:
    rip:
  - id: GigabitEthernet0/1
    description: "Internet connexion ip nat outside"
    ipv4_address: dhcp
```

Vous pouvez créer des liens dans le document [jekyll-docs](https://jekyllrb.com/docs/home).

Afficher une image distante :

![image distante](https://github.githubassets.com/images/modules/logos_page/GitHub-Logo.png)

ou une image locale :

![image locale](/projet/assets/GitHub-Logo.png)
