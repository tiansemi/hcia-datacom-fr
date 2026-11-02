---
id: HUA-HCIA-DC-001
title: Data Communication Network
official_title: Data Communication Network
course_unit: 1.1.1
category: Huawei HCIA-Datacom Topic
domain: Datacom
vendor: Huawei
certification: HCIA-Datacom
version: V2.0
difficulty: beginner
status: draft
author: MOULO OHOLO Jean Noel
reviewed: false
created: 2026-07-06
updated: 2026-08-14
source: HCIA-Datacom V2.0 Training Material
source_pages: PDF 4-17
tags:
  - huawei
  - hcia
  - datacom
  - network-basics
prerequisites: []
related:
  - "[[Data Communication Network Devices]]"
  - "[[01 Data Communication Network Basics/Data Communication Process]]"
lab_required: false
publication_status: generated
site_url: https://tiansemi.github.io/apprentissage/reseaux/hcia-datacom-comprendre-reseaux-communication-donnees
---

# Réseau de communication de données (Data Communication Network)

## Portée de l'unité

Cette unité présente les fondements d'un réseau de communication de données : la communication, son évolution technologique, la communication réseau, les équipements d'un réseau datacom, les topologies et les principales classifications des réseaux.

Le contenu ci-dessous suit l'ordre et le niveau de détail du support officiel Huawei. Les compléments propres à TianSemi sont explicitement signalés.

## Communication

L'essence de la communication réside dans la **transmission et l'échange d'informations entre au moins deux points**. Elle désigne donc la circulation et l'interaction de l'information :

- entre des personnes ;
- entre des personnes et des objets ;
- entre des objets.

Ces échanges sont rendus possibles par certains médias et certaines actions.

### Les trois éléments d'un système de communication

| Élément | Définition fidèle au support | Exemples cités |
| --- | --- | --- |
| **Source** | équipement qui envoie l'information | ordinateur ou téléphone mobile d'un utilisateur |
| **Canal** | média par lequel l'information est transmise | média physique, comme un câble ou une fibre optique, ou média sans fil, comme les ondes radio |
| **Destination** | équipement qui reçoit l'information | équipement récepteur de l'utilisateur distant |

![Système de communication et transmission d'un signal audio entre deux utilisateurs](<../../../assets/diagrammes/01 Data Communication Network Basics/communication-system-official.webp>)

*Figure officielle Huawei, page PDF 5.*

### Exemple de transmission audio

Lors d'un appel téléphonique :

1. l'utilisateur A constitue la source et produit un signal audio ;
2. le signal audio est converti en signal électrique ;
3. le signal électrique est transporté par le canal, ici une ligne téléphonique ;
4. à l'autre extrémité, le signal électrique est reconverti en signal audio ;
5. l'utilisateur B constitue la destination et reçoit le son.

### Deux autres éléments indispensables

En plus des trois composants du système, la communication fait intervenir deux notions essentielles :

- **Information** : contenu à transmettre. Il peut prendre n'importe quelle forme, notamment la voix, le texte, les éléments graphiques et les données.
- **Protocole de communication** : règles convenues par les deux parties, par exemple une langue, un mode de codage ou une norme de communication. Ces règles permettent de coder, transmettre puis décoder correctement l'information.

## Développement des technologies de communication

Huawei présente l'évolution des communications en quatre grandes périodes.

![Évolution des technologies de communication, des signaux anciens à l'Internet of Everything](<../../../assets/diagrammes/01 Data Communication Network Basics/communication-technology-development-official.webp>)

*Figure officielle Huawei, page PDF 6.*

### Époque ancienne

À cette époque, la communication dépend entièrement des médias naturels les plus élémentaires ainsi que de la force humaine ou animale.

- **Communication visuelle et sonore**, autour de 3000 avant notre ère : tours à signaux, feux ou fumées, battements de tambour et cors. Ces moyens ne transmettent qu'une quantité très limitée d'informations, généralement des signaux définis à l'avance. Leur portée est courte et ils sont facilement perturbés par la météo et le relief.
- **Communication postale**, autour de 500 avant notre ère : des relais et des systèmes postaux sont établis, puis les lettres sont transportées par des personnes et des chevaux. Une quantité d'informations plus importante peut être transmise, mais très lentement, parfois en plusieurs jours ou plusieurs mois.

### Ère électrique

Du milieu du XIXe siècle au début du XXe siècle, l'invention et l'utilisation de l'électricité transforment complètement les communications. Pour la première fois, l'information peut se déplacer à la vitesse de la lumière.

- **Télégraphe, années 1830 à 1840** : il permet pour la première fois une communication quasi instantanée sur de longues distances, y compris entre continents ou à travers les océans. L'information est transportée par des fils électriques.
- **Téléphone, années 1870** : il rend possible une communication vocale bidirectionnelle en temps réel, plus naturelle et plus directe.
- **Radio, années 1890** : elle libère la communication de la contrainte des liaisons physiques et pose les bases des communications mobiles et de la diffusion. La radio AM/FM puis la télévision deviennent ensuite les principaux médias de masse du XXe siècle.

### Ère de l'information

Du milieu du XXe siècle à aujourd'hui, l'invention des transistors, des circuits intégrés et des technologies informatiques fait entrer la communication dans une ère numérique et interconnectée.

#### Ordinateurs et Internet

Entre les années 1960 et 1990, la naissance d'ARPANET et l'adoption de **TCP/IP** marquent le début de l'ère Internet. La communication ne se limite plus aux échanges entre personnes : elle s'étend aux interactions entre machines et entre réseaux.

#### Communications mobiles

| Génération | Apport présenté par Huawei |
| --- | --- |
| **1G** | voix analogique et possibilité de téléphoner en déplacement |
| **2G** | voix numérique et introduction du service de messages courts, ou SMS |
| **3G** | accès à l'Internet mobile, navigation web et échange d'e-mails |
| **4G** | véritable ère du haut débit mobile, avec l'essor de la vidéo HD fluide, du jeu en ligne et du paiement mobile |
| **5G** | très haut débit, très faible latence et connectivité massive ; prise en charge de secteurs comme l'IoT, la conduite autonome, la télémédecine et l'automatisation industrielle |

Les sigles 1G, 2G, 3G, 4G et 5G désignent respectivement les première, deuxième, troisième, quatrième et cinquième générations de technologies de communication mobile.

### Ère de l'Internet of Everything

Le développement des technologies de communication continue de s'accélérer.

- **Approfondissement de la 5G et exploration de la 6G** : les évolutions 5G-A ou 5.5G doivent encore améliorer les performances. Huawei indique que la commercialisation de la 6G est envisagée autour de 2030, avec des usages comme la communication holographique, l'interconnexion sensorielle et les jumeaux numériques, afin d'atteindre une connectivité intelligente de toute chose.
- **Convergence de l'IoT et de l'intelligence artificielle** : la communication s'étend des humains à l'ensemble des objets. L'IA peut rendre les réseaux plus efficaces et plus fiables grâce à l'allocation intelligente des ressources, à la prédiction des pannes et à la protection de la sécurité.

## Communication réseau

La communication réseau désigne la transmission d'informations entre des terminaux au moyen d'un réseau de communication de données.

![Trois exemples officiels de communication réseau](<../../../assets/diagrammes/01 Data Communication Network Basics/network-communication-examples-official.webp>)

*Figure officielle Huawei, page PDF 8.*

Huawei présente trois exemples :

### A. Deux ordinateurs directement reliés

Deux ordinateurs, qui sont ici les terminaux, échangent des fichiers au moyen d'un seul câble réseau. Cette liaison constitue le réseau de communication de données le plus simple.

### B. Plusieurs ordinateurs reliés par un équipement réseau

Un petit réseau peut être construit avec un équipement réseau, par exemple un routeur ou un switch, et plusieurs ordinateurs. Les fichiers peuvent alors circuler librement entre deux ordinateurs quelconques grâce au relais de l'équipement réseau.

### C. Téléchargement par Internet

Lorsqu'un ordinateur souhaite obtenir un fichier depuis un site web, il doit d'abord se connecter à Internet avant de pouvoir le télécharger.

Internet est aujourd'hui le plus grand réseau informatique, et donc le plus grand réseau de communication de données, au monde. Son prédécesseur, **ARPANET**, est né en 1969. La diffusion et l'utilisation généralisées d'Internet constituent l'un des signes de l'entrée dans l'ère de l'information.

## Réseau de communication de données

Un réseau de communication de données est constitué notamment :

- de routeurs ;
- de switchs ;
- de firewalls ;
- de contrôleurs d'accès sans fil, ou **WAC** ;
- de points d'accès sans fil, ou **AP** ;
- d'ordinateurs personnels ;
- d'imprimantes réseau ;
- de serveurs.

Sa fonction fondamentale est de réaliser la communication de données.

![Exemple officiel de réseau de communication de données](<../../../assets/diagrammes/01 Data Communication Network Basics/data-communication-network-official.webp>)

*Figure officielle Huawei, page PDF 9.*

Dans la suite du cours, Huawei abrège **data communication network** en **datacom network**.

## Topologie réseau

Une topologie réseau est une représentation structurée de la manière dont différents équipements, par exemple des ordinateurs, des routeurs et des switchs, sont reliés au moyen de médias de transmission comme les paires torsadées et les fibres optiques.

Un diagramme de topologie décrit la structure physique ou logique d'un réseau. Il constitue un élément essentiel de l'ingénierie réseau.

![Exemple officiel de diagramme de topologie réseau](<../../../assets/diagrammes/01 Data Communication Network Basics/network-topology-example-official.webp>)

*Figure officielle Huawei, page PDF 10.*

Huawei insiste sur le fait que la maîtrise du dessin professionnel des topologies réseau demande beaucoup de pratique.

## Types de topologies réseau

Les réseaux peuvent être classés, selon leur topologie, en réseaux en bus, en étoile, en anneau, en arbre, en maillage complet et en maillage partiel.

![Topologies en bus, étoile, anneau, arbre, maillage complet et maillage partiel](<../../../assets/diagrammes/01 Data Communication Network Basics/network-topology-types-official.webp>)

*Figure officielle Huawei, page PDF 11.*

### Bus

Tous les nœuds sont reliés à un bus, par exemple un câble coaxial.

- **Avantages** : installation simple et quantité minimale de câbles. En général, la panne d'un nœud n'empêche pas l'ensemble du réseau de communiquer.
- **Inconvénients** : la panne du bus interrompt la communication de tout le réseau. Les informations envoyées par un nœud peuvent être reçues par tous les autres nœuds, ce qui présente un risque de sécurité.

### Étoile

Tous les nœuds sont reliés à un nœud central.

- **Avantages** : de nouveaux nœuds peuvent être ajoutés facilement. Comme les données passent par le nœud central, la supervision du réseau est facilitée.
- **Inconvénient** : la panne du nœud central affecte la communication de tout le réseau.

### Anneau

Tous les nœuds sont reliés de manière à former un anneau fermé.

- **Avantage** : une quantité réduite de câbles est nécessaire.
- **Inconvénient** : l'ajout d'un nœud est contraignant, car il faut ouvrir l'anneau existant avant d'insérer le nouveau nœud et de refermer l'anneau.

### Arbre

Une topologie en arbre est en réalité une topologie en étoile hiérarchique.

- **Avantage** : plusieurs réseaux en étoile peuvent être reliés rapidement, ce qui facilite l'extension du réseau.
- **Inconvénient** : plus un nœud est haut dans la hiérarchie, plus sa panne provoque des problèmes importants dans le réseau.

### Maillage complet

Tous les nœuds sont interconnectés par des câbles.

- **Avantage** : fiabilité et efficacité de communication élevées.
- **Inconvénients** : chaque nœud doit disposer de nombreux ports physiques et de nombreux câbles. La topologie est coûteuse et difficile à faire évoluer.

### Maillage partiel

Seuls les nœuds essentiels sont interconnectés.

- **Avantage** : coût inférieur à celui d'un maillage complet.
- **Inconvénient** : fiabilité inférieure à celle d'un maillage complet.

Dans les réseaux réels, plusieurs topologies sont souvent combinées afin d'obtenir un compromis entre le coût, les performances et la fiabilité.

## Types de réseaux

Un réseau datacom peut être classé différemment selon le critère retenu. Huawei présente deux modes de classification courants.

### Selon la couverture géographique

- **LAN** : Local Area Network ;
- **MAN** : Metropolitan Area Network ;
- **WAN** : Wide Area Network.

### Selon l'entité qui exploite le réseau

- réseau d'entreprise ;
- réseau opérateur.

## LAN, MAN et WAN

### LAN - Local Area Network

Un LAN regroupe des ordinateurs, des serveurs et des équipements réseau dans une zone géographique limitée. Sa couverture reste généralement inférieure à quelques kilomètres.

- **Fonction principale** : relier plusieurs terminaux relativement proches, par exemple dans un logement, un ou plusieurs bâtiments ou un campus.
- **Technologies citées** : Ethernet et Wi-Fi.
- **Scénarios typiques** : réseau de bureau d'une entreprise, réseau d'un cybercafé et réseau domestique.

### MAN - Metropolitan Area Network

Un MAN peut être considéré comme un LAN de grande taille. Sa construction est plus coûteuse, mais il offre des débits de transmission supérieurs. Il améliore les médias de transmission employés dans un LAN et étend la zone desservie jusqu'à un campus universitaire ou une ville.

- **Fonction principale** : relier des hôtes, des bases de données et des LAN situés à différents endroits d'une même ville.
- **Technologies citées** : technologies proches de celles des LAN, notamment Ethernet à 10 Gbit/s ou 100 Gbit/s et WiMAX.
- **Scénarios typiques** : MAN haut débit, MAN éducatif et réseau privé municipal ou provincial d'administration électronique.

### WAN - Wide Area Network

Un WAN couvre généralement une zone allant de quelques dizaines à plusieurs milliers de kilomètres. Il peut relier les réseaux de plusieurs villes, voire de plusieurs pays, et fournir des communications longue distance.

- **Fonction principale** : relier plusieurs LAN ou MAN géographiquement éloignés, par exemple dans différentes villes ou différents pays.
- **Technologies citées** : PPP, MPLS et SRv6.
- **Scénario typique** : Internet.

Les acronymes utilisés par Huawei signifient :

- **WiMAX** : Worldwide Interoperability for Microwave Access ;
- **PPP** : Point-to-Point Protocol ;
- **MPLS** : Multiprotocol Label Switching ;
- **SRv6** : Segment Routing over IPv6.

### Exemple dans le secteur de l'éducation

![Organisation officielle des LAN, MAN et WAN dans le secteur de l'éducation](<../../../assets/diagrammes/01 Data Communication Network Basics/education-lan-man-wan-official.webp>)

*Figure officielle Huawei, page PDF 15.*

Le schéma présente des LAN d'établissements scolaires et universitaires raccordés aux cœurs MAN des bureaux de l'éducation à l'échelle d'un district ou comté, d'une municipalité et d'une province. Les nœuds WAN des différents niveaux assurent ensuite leur interconnexion et leur accès à Internet.

## Réseau d'entreprise et réseau opérateur

Les réseaux datacom peuvent également être classés selon l'entité qui les exploite.

### Réseau d'entreprise

Un réseau d'entreprise est un réseau privé construit et maintenu par une seule organisation, par exemple une entreprise, une administration ou un établissement scolaire. Il relie les employés, les équipements, les data centers et les agences afin de répondre aux besoins quotidiens liés au travail de bureau, à la production et à la gestion.

- **Technologies citées** : technologies Ethernet comme les VLAN et les VPN, Wi-Fi et technologies de firewall.
- **Scénarios typiques** : réseaux campus, notamment immeubles de bureaux, usines et campus scolaires ; réseaux de data center ; backbones ou WAN construits par l'entreprise.

### Réseau opérateur

Un réseau opérateur est une infrastructure publique construite et exploitée par un fournisseur de services de télécommunications. Il fournit aux clients externes, particuliers ou entreprises, des services d'accès à Internet, de communication vocale et de transfert de données.

- **Technologies citées** : BGP, MPLS et cœur de réseau 5G.
- **Scénarios typiques** : backbones Internet reliant des régions ou des pays ; réseaux mobiles composés de stations de base 4G/5G et de réseaux cœur ; liaisons privées d'entreprise comme MPLS VPN et SD-WAN.

Sauf indication contraire, les réseaux étudiés dans ce cours sont des **réseaux d'entreprise**.

Les acronymes utilisés ici signifient :

- **VLAN** : Virtual Local Area Network ;
- **VPN** : Virtual Private Network ;
- **BGP** : Border Gateway Protocol ;
- **SD-WAN** : Software-Defined Wide Area Network.

## Réseau d'entreprise dans un environnement de bureau

Un réseau de bureau constitue un scénario typique de réseau d'entreprise.

![Réseau d'entreprise officiel dans un environnement de bureau](<../../../assets/diagrammes/01 Data Communication Network Basics/enterprise-office-network-official.webp>)

*Figure officielle Huawei, page PDF 17.*

Il relie les équipements réseau et les terminaux de l'organisation afin de permettre le partage des ressources et l'échange d'informations. Il fournit également aux employés un point d'entrée unifié vers les ressources internes et les services réseau externes.

Le schéma Huawei répartit des AP dans l'espace de travail ouvert, le bureau du responsable, la salle de conférence, la zone de travail mobile et l'accueil. Ces AP sont reliés à un switch d'accès.

## Mise en pratique TianSemi

> [!note] Complément non officiel, directement aligné sur le support Huawei
> L'objectif est de pratiquer la lecture et le dessin de topologies, compétence que le support présente comme essentielle à l'ingénierie réseau.

1. Dessiner le réseau d'un petit bureau comportant cinq PC, une imprimante, deux AP, un switch d'accès, un firewall et un accès Internet.
2. Sur le chemin d'une requête envoyée par un PC vers Internet, identifier la source, le canal et la destination.
3. Indiquer les parties qui constituent le LAN du bureau et expliquer pourquoi la connexion vers un site distant relève d'un WAN.
4. Proposer une topologie en étoile, puis expliquer le risque créé par la panne du nœud central.
5. Ajouter une liaison redondante et expliquer si la topologie obtenue est un maillage complet ou partiel.

## Révision TianSemi

1. Quelles formes d'interaction l'essence de la communication couvre-t-elle selon Huawei ?
2. Quels sont les trois éléments fondamentaux d'un système de communication et quel rôle chacun joue-t-il ?
3. Pourquoi un protocole commun est-il nécessaire à la transmission d'une information ?
4. Quelles évolutions distinguent les communications mobiles 1G, 2G, 3G, 4G et 5G ?
5. Quels avantages et inconvénients Huawei associe-t-il aux six topologies présentées ?
6. Comment distinguer un LAN, un MAN et un WAN par leur portée, leur fonction et leurs technologies ?
7. Quelle différence existe entre un réseau d'entreprise et un réseau opérateur ?
8. Pourquoi le réseau de bureau présenté par Huawei est-il un réseau d'entreprise ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 4-17.
- Huawei Talent, HCIA-Datacom V2.0 : https://e.huawei.com/en/talent/#/cert/product-details?certifiedProductId=1316&authenticationLevel=CTYPE_CARE_HCIA&technicalField=IIC&version=2.0
