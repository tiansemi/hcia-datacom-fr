---
id: HUA-HCIA-DC-084
title: "Practices in Campus Network Projects"
official_title: "Practices in Campus Network Projects"
course_unit: "8.2.1"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "intermediate"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-11
updated: 2026-08-11
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 1091-1117"
tags: [huawei, hcia, datacom, campus, design, deployment, commissioning, operations, optimization]
prerequisites:
  - "[[Basic Concepts of Campus Networks]]"
related:
  - "[[Overview of Network Troubleshooting]]"
  - "[[Introduction to iMaster NCE]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Pratiques d’un projet de réseau de campus

## Du besoin métier à un réseau exploitable

Huawei illustre la démarche avec une entreprise d’environ 200 employés. Le nouveau campus doit prendre en charge les services actuels, rester simple à maintenir, fournir un accès filaire aux employés et un Wi-Fi aux invités, permettre un contrôle élémentaire du trafic et offrir un niveau de sécurité adapté.

Ces exigences ne constituent pas encore une solution. Elles doivent être traduites en architecture, plans d’adressage et de VLAN, choix de protocoles, règles de sécurité, procédures de recette et organisation de l’exploitation.

![Cycle de vie d’un projet de réseau de campus](<../../../assets/diagrammes/08 Typical Campus Networking Solution/campus-project-lifecycle-fr.svg>)

## Le cycle de vie en quatre phases

| Phase | Finalité | Activités clés |
| --- | --- | --- |
| Planification et conception | Transformer les besoins en solution cohérente | Choix des équipements, topologies physique et logique, protocoles et technologies. |
| Déploiement et implémentation | Construire et mettre le réseau en service | Installation, configuration, tests conjoints, bascule, formation et recette. |
| Exploitation et maintenance | Maintenir les services stables et visibles | Maintenance courante, supervision, gestion des configurations et dépannage. |
| Optimisation | Adapter le réseau à l’évolution des usages | Renforcement de la sécurité, sauvegardes, mises à niveau, capacité et expérience utilisateur. |

Une phase ne remplace pas les précédentes. Une optimisation sérieuse s’appuie sur une conception documentée, une recette mesurable et des données d’exploitation fiables.

## 1 — Planifier et concevoir le petit campus

### Choisir l’architecture

Huawei propose trois possibilités selon l’échelle :

- une couche d’accès unique pour un très petit site ;
- deux couches, cœur et accès, pour un petit ou moyen campus ;
- trois couches, cœur, agrégation et accès, pour un campus plus important.

Dans le cas étudié, une architecture à deux couches est retenue. Un routeur NetEngine AR assure la sortie vers le réseau externe. Un commutateur CloudEngine S joue le rôle de cœur et de passerelle pour les services filaires et sans fil. Des commutateurs d’accès CloudEngine S connectent postes, imprimantes et AP. Le WLAN utilise un WAC et des Fit AP AirEngine.

Le choix des modèles dépend notamment du débit attendu. Les noms d’équipements doivent suivre une convention uniforme, mémorisable et extensible, par exemple `R1`, `Core-S1`, `Acc-S1` et `WAC1`.

### Planifier les VLAN

Huawei distingue trois familles :

| Type | Rôle | Exemple du cas Huawei |
| --- | --- | --- |
| Gestion | Administrer les équipements et transporter la gestion WLAN | VLAN 2 pour les équipements de couche 2 ; VLAN 3 pour les AP. |
| Interconnexion | Relier les équipements de couche 3 via des interfaces VLANIF | VLAN 10 entre le cœur et le routeur ; VLAN 11 entre le cœur et le WAC. |
| Service | Séparer les populations et les usages | VLAN 100 invités, 101 R&D, 102 marketing, 103 administration. |

Des identifiants consécutifs simplifient l’utilisation des ressources. L’affectation par interface est la méthode la plus courante dans ce scénario. La séparation logique doit suivre les besoins de service, pas seulement la disposition physique des bureaux.

### Planifier l’adressage IP

Trois catégories correspondent aux VLAN :

- **adresses de gestion** pour administrer les équipements de couche 2 et les AP ;
- **adresses d’interconnexion** entre équipements de couche 3 ;
- **adresses de service** pour les utilisateurs, serveurs et passerelles.

Le cas Huawei réserve notamment `192.168.2.0/24` à la gestion filaire, `192.168.3.0/24` à la gestion WLAN, des préfixes `/30` aux interconnexions et les réseaux `192.168.100.0/24` à `192.168.103.0/24` aux services.

Les passerelles conservent une convention stable, ici l’adresse `.254`. Les segments de service doivent être faciles à distinguer, contigus lorsque l’agrégation est utile et dimensionnés selon le nombre de clients et la réserve nécessaire.

### Choisir le mode d’attribution

- La sortie WAN reçoit l’adresse fournie par l’opérateur ; dans ce cas, elle est statique.
- Les équipements réseau, hors Fit AP, utilisent des adresses statiques.
- Les Fit AP et les terminaux obtiennent leur adresse par DHCP.
- Les serveurs et imprimantes conservent des adresses fixes ou des attributions DHCP réservées.

Ce choix concilie stabilité de l’infrastructure et simplicité d’exploitation des terminaux.

### Concevoir le routage

Le cœur fournit les passerelles des réseaux de service. Entre équipements de couche 3, Huawei retient OSPF dans une seule aire pour le cas étudié. À la sortie du campus, une route statique par défaut permet de joindre Internet ou le WAN.

Pour un réseau simple, des routes statiques peuvent remplacer OSPF. Le protocole doit donc être choisi selon la topologie et l’exploitation attendue, non par automatisme.

### Concevoir le WLAN

Le cas utilise une architecture **WAC hors chemin de couche 3** et un transfert direct des données utilisateurs. Les paquets de contrôle empruntent le tunnel CAPWAP ; les données utilisateurs ne passent pas par le WAC.

La planification WLAN doit aussi couvrir :

- la zone et la qualité de couverture ;
- la capacité, selon le nombre de stations, leur simultanéité et le débit attendu ;
- la position, le montage, l’alimentation et le câblage des AP ;
- la sécurité et, selon le besoin, l’itinérance.

Dans l’exemple, les AP utilisent le VLAN 3, les invités le VLAN 100, le SSID est `WLAN-Guest`, la sécurité est de type WPA/WPA2-PSK avec AES et le profil VAP utilise le transfert direct. En production, tout secret doit être propre au site, robuste et conservé hors de la documentation publique.

### Concevoir fiabilité, prévention des boucles et sécurité

- L’agrégation Ethernet peut renforcer la liaison et augmenter sa bande passante.
- STP doit rester actif ; le commutateur de cœur est prévu comme pont racine afin de maîtriser la topologie.
- Les invités peuvent accéder à Internet sans atteindre le réseau interne.
- Les politiques de trafic et NAT contrôlent les échanges entre services et sortie.
- DHCP snooping protège contre un serveur DHCP non autorisé ; les liaisons vers le serveur légitime sont déclarées de confiance.
- Les ACL limitent l’administration aux sources autorisées.
- SNMPv3 offre authentification et chiffrement pour la gestion centralisée.

### Concevoir l’exploitation

Sur un petit réseau, un administrateur peut gérer un équipement si son adresse est joignable, idéalement via SSH plutôt que Telnet. Lorsque le parc grandit, une plateforme de gestion centralisée devient nécessaire. Huawei cite SNMP pour la supervision traditionnelle et iMaster NCE pour une gestion et une maintenance plus automatisées.

## 2 — Déployer et mettre en service

Le processus présenté par Huawei comprend :

1. formulation de la solution ;
2. installation des équipements et câblage ;
3. configuration et mise au point du réseau ;
4. bascule vers le nouveau service ;
5. transfert à l’équipe de maintenance et formation ;
6. recette du projet.

L’ordre exact dépend des conditions réelles, mais les dépendances doivent être respectées : la couche physique est validée avant les services, les services avant la bascule, et la bascule avant l’acceptation finale.

### Relier le plan à la configuration

| Domaine | Implémentation attendue dans le cas Huawei |
| --- | --- |
| Fiabilité | Eth-Trunk si des liens redondants sont prévus. |
| Boucles de couche 2 | STP actif, cœur défini comme racine. |
| Segmentation | VLAN affectés selon les interfaces et les services. |
| Adressage | DHCP pour AP et terminaux ; statique pour l’infrastructure. |
| Routage | OSPF sur les équipements de couche 3 et route par défaut à la sortie. |
| WLAN | Profils configurés conformément au plan radio et de service. |
| Contrôle du trafic | ACL et politique de trafic selon les communications autorisées. |
| Accès Internet | Politique NAT limitée aux réseaux autorisés. |
| Sécurité DHCP | DHCP snooping et interfaces de confiance correctement identifiées. |
| Administration | ACL de gestion, STelnet et authentification AAA. |

Chaque configuration doit pouvoir être rattachée à une exigence ou à un risque identifié.

### Recetter le réseau

Huawei regroupe la mise au point en trois familles de tests :

- **connectivité** : état des liens, couche 2 et couche 3 ;
- **fiabilité** : prévention des boucles, basculement de chemin et mécanismes de secours ;
- **performance des services** : trafic métier et contrôle d’accès.

Un résultat « ping réussi » ne suffit donc pas à accepter le projet. La recette doit vérifier les chemins attendus, les refus attendus, les services filaires et sans fil ainsi que le comportement en cas de panne prévue.

## 3 — Exploiter et maintenir

Après la mise en service, l’O&M vise à détecter et supprimer les risques avant qu’ils ne dégradent les services.

- La maintenance courante contrôle environnement, informations de base, performances, trafic, qualité et alarmes.
- La gestion de configuration encadre les changements, sauvegarde et archive les configurations, puis gère les versions logicielles.
- Le dépannage suit une boucle complète : détecter, localiser, corriger, analyser la cause racine et prévenir la récidive.

À mesure que le campus grandit, une plateforme de gestion et des outils d’automatisation deviennent nécessaires pour conserver visibilité, cohérence et rapidité d’intervention.

## 4 — Optimiser en continu

L’optimisation n’est pas une réparation ponctuelle. Elle analyse les tendances et adapte le réseau :

- performance : débit utile, utilisation de bande passante, latence, gigue et pertes ;
- ressources : extension de capacité, équilibrage de charge et priorités de service ;
- expérience utilisateur : mesure proactive, retours utilisateurs et protection des usages critiques.

Les plateformes d’analyse et les mécanismes d’optimisation automatique peuvent compléter les configurations statiques lorsque la complexité augmente. Toute optimisation doit néanmoins conserver une mesure avant/après et une possibilité de retour.

## Pratique TianSemi : dossier de projet d’un campus de 200 utilisateurs

Produisez un mini-dossier pour le cas Huawei :

1. reformulez les cinq exigences métier et ajoutez un critère de recette mesurable pour chacune ;
2. dessinez l’architecture à deux couches avec `R1`, `Core-S1`, `WAC1`, quatre commutateurs d’accès et un AP invité ;
3. construisez les tableaux VLAN et IP à partir des valeurs officielles ;
4. indiquez le mode d’attribution de chaque catégorie d’adresse ;
5. décrivez le routage interne, la route de sortie et le mode de transfert WLAN ;
6. rédigez cinq règles de sécurité, dont isolation invités, DHCP snooping et administration restreinte ;
7. préparez une recette comprenant tests positifs, tests de refus et test de basculement ;
8. ajoutez une procédure de sauvegarde, un indicateur d’exploitation et un objectif d’optimisation.

Le livrable doit permettre à une autre personne de relier chaque exigence à un choix de conception puis à une preuve de recette.

## À retenir pour l’examen HCIA-Datacom

- Le cycle de vie comprend planification et conception, déploiement et implémentation, O&M, puis optimisation.
- VLAN de gestion, d’interconnexion et de service répondent à des fonctions différentes.
- Adresses statiques pour l’infrastructure et DHCP pour les terminaux simplifient l’exploitation.
- Le cas Huawei utilise une architecture à deux couches, OSPF en interne et une route statique par défaut à la sortie.
- Le WLAN étudié est hors chemin de couche 3 avec transfert direct des données.
- STP, DHCP snooping, ACL, NAT, SSH/AAA et SNMPv3 couvrent des risques distincts.
- La recette vérifie connectivité, fiabilité et fonctionnement des services.
- L’O&M maintient la stabilité ; l’optimisation améliore performances, ressources et expérience.

## Révision TianSemi

1. Quelles sont les quatre phases du cycle de vie d’un campus ?
2. Pourquoi les exigences métier doivent-elles être associées à des critères de recette ?
3. Quelle architecture Huawei retient-il pour l’entreprise d’environ 200 employés ?
4. Quelle différence existe entre VLAN de gestion, d’interconnexion et de service ?
5. Quelles adresses sont statiques et lesquelles sont attribuées par DHCP dans le cas étudié ?
6. Comment le routage interne et le routage de sortie sont-ils conçus ?
7. Que signifie une architecture WLAN hors chemin de couche 3 avec transfert direct ?
8. Quels tests faut-il réaliser avant d’accepter le réseau ?
9. Quelle différence existe entre O&M et optimisation ?
10. Comment relier une configuration à une exigence de conception ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1091 à 1117.
