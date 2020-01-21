---
ID: 23
post_title: Installation
author: ebelhomme
post_excerpt: ""
layout: page
permalink: https://blog.rgm-cloud.io/installation/
published: true
post_date: 2019-12-11 23:08:45
---
*RGM* s'installe au-dessus une installation minimale de GNU/Linux CentOS 7. Le déploiement de RGM est totalement industrialisé et fait appel à des *rôles* et *playbooks* **Ansible**.

Le processus d'installation proprement dit peut-être entièrement automatisée par l'exécution de notre [one-liner](https://installer.rgm-cloud.io/rgm-installer.sh) en tant qu'utilisateur **root**, ou peut-être intégré à vos propres processes via la réutilisation de nos *rôles* et *playbooks* **Ansible**.

# Pour les pressés

Si vous êtes pressé, ouvrez un terminal sur votre serveur en tant qu'utilisateur **root**, puis copiez-collez la ligne suivante ([one-liner](#le-one-liner)):
```shell
bash <(curl -k https://installer.rgm-cloud.io/rgm-installer.sh) -y
```

Maintenant, servez-vous un bon café, et pendant que *RGM* s'occupe de tout, vous avez le temps de lire tranquillement la suite de cet article, vous y apprendrez quantité de choses intéressantes ;)

# Préparation

## Prérequis

*RGM* peut être installé sur tout serveur X86_64, bare-metal ou VM ou même sur le Cloud, qu'il soit publique ou privé. Le matériel doit fournir à minima:

| ressource      | minimum | recommandé |
|----------------|---------|------------|
| CPU            | 2       | 4 (*)      |
| RAM            | 4 GB    | 8 GB (*)   |
| Disque systeme | 20 GB   | 40 GB      |
| Disque données | 50 GB   | 150 GB (*) |

(*) La configuration recommandée peut varier en fonction du parc SI à superviser.

En outre le système cible doit être préalablement correctement configuré dans l'écosystème de l'infrastructure cible, en particulier les éléments suivants doivent être configurés:
- configuration réseau:
  - adresse IPv4 fixe, masque et passerelle par défaut,
  - nom d'hôte, domaine, FQDN correctement renseigné sur le résolveur DNS,
- l'accès à Internet (en HTTP et HTTPS), soit en connexion directe, soit via un proxy.

## Eléments de configuration optionnels

Le déroulement du *playbook* **Ansible** d'installation peut être modifié en reseignant un certain nombre de variables. Notamment les éléments suivants:
- communauté SNMP,
- liste de serveurs de temps,
- serveur mandataire HTTP,
- schéma de partitionnement du disque de données,
- certificat X509 pour le serveur HTTP,
- etc.

Pour plus de détails, se reporter aux documentations de nos composants *Ansible* suivants: 
- [ansible-playbook-rgm-installer](https://github.com/RGM-OSC/ansible-playbook-rgm-installer)
- [ansible-role-snmp](https://github.com/RGM-OSC/ansible-role-snmp)
- [ansible-role-lvm-part](https://github.com/RGM-OSC/ansible-role-lvm-part)
- [ansible-role-rgm](https://github.com/RGM-OSC/ansible-role-rgm)

# Installation

## le *one-liner*

Nous proposons un *one-liner* afin d'installer en une unique ligne de commande *RGM*.

Qu'est-ce qu'un *one-liner* ? Il s'agit d'un script **bash**, téléchargeable depuis nos serveurs de déploiement, qui se charge du *bootstrap* des prérequis à *RGM* et de lancer l'installation.

Concrètement les actions effectuées sont les suivantes:
- vérification de la conformité matérielle de la plate-forme avec les [prérequis de *RGM*](#Prérequis),
- configuration et activation des dépôts [EPEL du projet Fedora](https://fedoraproject.org/wiki/EPEL),
- configuration et activation des dépôts [RGM-community](https://community.repo.rgm-cloud.io) et [RGM](https://repo.rgm-cloud.io),
- installation de la clé GPG de signature des dépôts *RGM*,
- dans le cadre d'un contrat **business**, installation du certificat client,
- installation des paquets **RPM** nécessaire au *bootstraping* de *RGM* (git, ansible, et leurs dépendances),
- détection et auto-partitionnement du disque de données **si un disque non partitionné est détecté**,
- clônage du dépôt **git** du [playbook d'installation de RGM ](https://github.com/RGM-OSC/ansible-playbook-rgm-installer), et exécution de ce dernier.

Dans sa plus simple expression, l'installation de *RGM* se résume à invoquer la commande suivante:
```shell
bash <(curl -k https://installer.rgm-cloud.io/rgm-installer.sh) -y
```
- la commande `curl` télécharge le script *bash* d'installation,
- la commande `bash` exécute directement le script téléchargé,
- on peut éventuellement passer des paramètres au script afin de personnaliser l'installation.

Les arguments supportés par le script :

| cmd | param                      | description |
|-----|----------------------------|-------------|
| -h  |  *n/a*                     | affiche un message d'aide des différentes options disponibles |
| -y  |  *n/a*                     | désactive le mode interactif et repond **oui** à toutes les questions |
| -b  |  *n/a*                     | désactive la vérification de conformité matérielle. à vos risques et périls ! |
| -u  |  *n/a*                     | désactive l'auto-partitionnement de disque |
| -d  |  *n/a*                     | installe la version **développeur** en lieu et place de la version de production |
| -n  |  *n/a*                     | le one-liner s'arrête **avant** l'exécution du playbook ansible. Utile pour *bootstraper* l'environement d'installation et adapter la configuration *ansible* avant de procéder à l'installation de *RGM* |
| -e  | *chaîne de caractères*     | permet de passer des **extra_vars** *Ansible* au *playbook* d'installation |
| -p  | <proxy_host:proxy_port>    | specifie un *proxy HTTP*. la variable d'environement système **http_proxy** sera mise à jour, ainsi que la configuration système de **yum** |
| -c  | *nom d'instance*           | Dans le cas d'un installation **business**, permet de spécifier le nom d'identification de l'instance cliente |
| -o  | 'oracle', 'vmware', 'java' | active l'installation de paquets logiciels soumis à license commerciales. **l'installation de ces logiciels vaut acceptation des licenses associées !** |

## Ansible

Si vous souhaitez intégrer l'installation de RGM directement dans vos processes de déploiement, vous pouvez réutiliser directement notre [rôle ansible RGM](https://github.com/RGM-OSC/ansible-role-rgm) qui gère l'installation ainsi que la mise à jour.

