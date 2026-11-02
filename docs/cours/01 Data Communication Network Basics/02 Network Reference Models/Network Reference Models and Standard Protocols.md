---
id: HUA-HCIA-DC-006
title: Network Reference Models and Standard Protocols
official_title: Network Reference Models and Standard Protocols
course_unit: 1.2.2
category: Huawei HCIA-Datacom Topic
domain: Datacom
vendor: Huawei
certification: HCIA-Datacom
version: V2.0
difficulty: beginner
status: draft
author: MOULO OHOLO Jean Noel
reviewed: false
created: 2026-07-13
updated: 2026-08-10
source: HCIA-Datacom V2.0 Training Material
source_pages: PDF 54-78
tags:
  - huawei
  - hcia
  - datacom
  - osi
  - tcp-ip
  - protocols
prerequisites:
  - "[[Applications and Data]]"
related:
  - "[[02 Network Reference Models/Data Communication Process]]"
lab_required: false
publication_status: not-started
site_url:
---

# Modèles de référence réseau et protocoles standards (Network Reference Models and Standard Protocols)

## Place dans le cours

L'unité **1.2.2 Network Reference Models and Standard Protocols** explique comment les fonctions nécessaires à une communication réseau sont réparties en couches. Elle présente successivement les organismes de normalisation, les modèles OSI et TCP/IP, le modèle TCP/IP équivalent utilisé par Huawei, puis les principales fonctions et technologies de chaque couche.

Cette unité répond à trois questions :

1. pourquoi les protocoles réseau sont-ils normalisés ?
2. comment les modèles de référence organisent-ils leurs fonctions ?
3. quels protocoles, adresses, médias et unités de données trouve-t-on à chaque couche ?

L'encapsulation et la décapsulation détaillées sont étudiées dans l'unité suivante, **Data Communication Process**.

## Protocoles réseau et normalisation

Un **protocole réseau** est un ensemble de règles et de conventions prédéfinies que des équipements respectent pour communiquer. Ces règles peuvent notamment préciser le format des données, leur ordre, la manière d'identifier les participants et la réaction attendue lorsqu'un événement se produit.

Huawei distingue deux familles :

- les **protocoles ouverts**, définis et publiés par des organismes de normalisation ;
- les **protocoles propriétaires**, définis par un fournisseur et destinés à ses propres produits ou solutions.

### Principaux organismes cités par Huawei

| Organisme | Rôle présenté dans le cours | Exemple cité |
| --- | --- | --- |
| **IETF** | développe et promeut les technologies et protocoles Internet | suite TCP/IP et RFC |
| **IEEE** | élabore notamment des normes pour les réseaux locaux filaires et sans fil | IEEE 802.3 et IEEE 802.11 |
| **ISO** | produit des normes internationales et a défini le modèle de référence OSI | ISO/IEC 7498-1 |
| **IEC** | normalise les domaines électriques et électroniques à l'échelle internationale | normes électrotechniques |

Une **RFC** (*Request for Comments*) est une publication de l'IETF. Elle peut décrire un standard, un protocole, une pratique ou une information liée au fonctionnement d'Internet. Toutes les RFC n'ont donc pas nécessairement le même statut normatif.

> [!important]
> La normalisation permet à des équipements et logiciels de constructeurs différents d'interopérer. Un modèle de référence, lui, organise les fonctions : il ne remplace pas les protocoles qui réalisent concrètement ces fonctions.

## Pourquoi organiser les communications en couches ?

Un équipement exécute plusieurs protocoles pour assurer une communication. Une architecture en couches permet de :

- séparer les responsabilités ;
- développer ou faire évoluer une couche sans redéfinir tout le système ;
- limiter les dépendances entre protocoles de couches différentes ;
- faciliter l'apprentissage, la conception et le diagnostic ;
- favoriser la normalisation et l'interopérabilité.

Chaque couche fournit des services à la couche supérieure et s'appuie sur la couche inférieure. Cette séparation reste logique : sur un équipement réel, plusieurs fonctions peuvent être mises en œuvre par les mêmes composants matériels ou logiciels.

## Trois représentations à ne pas confondre

Huawei présente le modèle OSI à sept couches, le modèle TCP/IP à quatre couches et un modèle TCP/IP équivalent à cinq couches.

![Correspondance entre les modèles OSI, TCP/IP et TCP/IP équivalent](<../../../assets/diagrammes/01 Data Communication Network Basics/osi-tcpip-model-mapping-fr.svg>)

### Modèle de référence OSI

L'ISO a proposé le modèle **OSI** (*Open Systems Interconnection*) dans les années 1980. Il décompose la communication en sept couches et sert surtout de cadre de compréhension et de conception.

| Couche | Fonction résumée conformément au support |
| --- | --- |
| **7. Application** | fournit aux applications des interfaces vers les services réseau |
| **6. Présentation** | assure l'encodage, la conversion et la représentation des données |
| **5. Session** | établit, gère et termine les sessions entre applications |
| **4. Transport** | fournit une communication de bout en bout et gère notamment séquence et rythme de transmission |
| **3. Réseau** | fournit l'adressage logique, détermine le chemin et transfère les données entre réseaux |
| **2. Liaison de données** | organise les bits en trames, assure la communication sur le lien et détecte les erreurs |
| **1. Physique** | transmet les flux de bits et définit les caractéristiques physiques du média |

### Modèle de référence TCP/IP

Le modèle **TCP/IP** provient de la conception et de la mise en œuvre d'ARPANET, puis a été enrichi par les travaux de l'IETF. Il résume une architecture issue de protocoles effectivement déployés.

Huawei rappelle les étapes suivantes :

- les RFC 793 et 791 ont respectivement décrit TCP et IP au début des années 1980 ;
- les RFC 1122 et 1123 ont ensuite précisé les exigences relatives aux hôtes et aux protocoles applicatifs du modèle à quatre couches.

| Couche TCP/IP | Fonction principale |
| --- | --- |
| **Application** | fournit les protocoles directement utilisés par les utilisateurs ou par les fonctions système |
| **Transport** | assure la communication de bout en bout, principalement avec TCP ou UDP |
| **Internet** | transfère les paquets entre hôtes et réseaux, principalement avec IP |
| **Interface réseau** | transmet les trames entre nœuds adjacents sur un même réseau physique |

### Modèle TCP/IP équivalent utilisé par Huawei

Le **modèle TCP/IP équivalent** n'est pas un modèle officiel défini par un organisme de normalisation. C'est une représentation pédagogique qui combine la pratique du modèle TCP/IP et la séparation plus détaillée du modèle OSI.

Il divise la couche interface réseau du modèle TCP/IP en deux couches :

1. liaison de données ;
2. physique.

Huawei utilise donc cinq couches dans la suite du cours : **application, transport, réseau, liaison de données et physique**. Les ingénieurs réseau se concentrent surtout sur les quatre couches basses, tandis que les ingénieurs IT travaillent davantage sur la couche application.

### Correspondance essentielle pour l'examen

| OSI | TCP/IP | TCP/IP équivalent Huawei |
| --- | --- | --- |
| Application + Présentation + Session | Application | Application |
| Transport | Transport | Transport |
| Réseau | Internet | Réseau |
| Liaison de données + Physique | Interface réseau | Liaison de données + Physique |

## Protocoles et unités de données de la pile TCP/IP

Le modèle indique **où** se situe une fonction ; le protocole indique **comment** cette fonction est réalisée. La pile présentée par Huawei associe les protocoles suivants aux couches du modèle équivalent.

![Protocoles, fonctions et PDU des cinq couches du modèle TCP/IP équivalent](<../../../assets/diagrammes/01 Data Communication Network Basics/tcpip-protocol-stack-pdu-fr.svg>)

| Couche | Protocoles ou technologies cités | PDU présentée dans le cours |
| --- | --- | --- |
| **Application** | Telnet, FTP, TFTP, SNMP, HTTP, SMTP, DNS, DHCP | data |
| **Transport** | TCP, UDP | segment |
| **Réseau** | IP, ICMP, IGMP | packet |
| **Liaison de données** | Ethernet, PPP, PPPoE | frame |
| **Physique** | cuivre, fibre optique, câble hybride, ondes électromagnétiques | bitstream |

Une **PDU** (*Protocol Data Unit*) est l'unité de données manipulée par une couche. Les noms ci-dessus correspondent à la terminologie utilisée dans ce support Huawei. Dans d'autres documentations, le mot *datagramme* peut notamment être employé pour UDP.

## Couche application : fournir les services réseau

La couche application fournit aux logiciels les interfaces nécessaires pour utiliser le réseau. Ses protocoles déterminent aussi le protocole de transport et les ports à employer.

### Protocoles d'application cités

| Protocole | Fonction résumée |
| --- | --- |
| **Telnet** | accès distant en mode texte à un terminal |
| **FTP** | transfert de fichiers avec des interactions client-serveur complètes |
| **TFTP** | transfert simple de fichiers, avec moins de fonctions que FTP |
| **SNMP** | gestion et supervision d'équipements réseau |
| **HTTP** | accès aux ressources d'un serveur web |
| **SMTP** | transfert de courrier électronique |
| **DNS** | résolution d'un nom de domaine en adresse IP |
| **DHCP** | attribution de paramètres réseau aux clients |

Le support associe explicitement les ports suivants à ses exemples :

| Protocole | Transport et port présentés |
| --- | --- |
| HTTP | TCP 80 |
| SMTP | TCP 25 |
| Telnet | TCP 23 |
| FTP | TCP 20 et 21 |
| DNS | UDP 53 |

### Exemple DNS

Chaque hôte communique au moyen d'une adresse IP, mais un utilisateur mémorise plus facilement un nom de domaine. DNS applique un modèle client-serveur :

1. le client DNS reçoit le nom demandé par l'application ;
2. il envoie une requête au serveur DNS ;
3. le serveur recherche la correspondance entre le nom et l'adresse IP ;
4. il renvoie l'adresse au client ;
5. le client peut alors communiquer avec le serveur visé.

Les noms et adresses représentés dans le support sont des exemples destinés à illustrer ce mécanisme.

### Exemple HTTP

HTTP fonctionne lui aussi selon une architecture client-serveur. Le navigateur demande une ressource identifiée par une **URL**, et le serveur web renvoie le contenu correspondant.

Une URL précise notamment le protocole et l'emplacement de la ressource. Par exemple, une URL web commence par `http://`, tandis qu'une ressource FTP peut être identifiée par une URL commençant par `ftp://`.

## Couche transport : relier les applications de bout en bout

La couche transport reçoit les données de la couche application, ajoute les informations nécessaires au transport et établit une communication de bout en bout entre processus applicatifs.

TCP et UDP utilisent des **ports source et destination** pour identifier les applications qui émettent et reçoivent les données.

| Protocole | Caractéristiques présentées par Huawei | Exemples d'usage cités |
| --- | --- | --- |
| **TCP** | orienté connexion, fiable, livraison ordonnée, contrôle de flux | web, transfert de fichiers, courrier électronique |
| **UDP** | sans connexion, faible surcharge et faible latence, sans garantie de livraison | appels vidéo, jeux en ligne, DNS |

> [!attention]
> « Sans garantie » ne signifie pas qu'UDP échoue systématiquement. Cela signifie que le protocole ne fournit pas lui-même les mécanismes de fiabilité et d'ordre présentés pour TCP.

## Couche réseau : adresser et router les paquets

La couche réseau transfère des données d'un hôte à un autre, y compris lorsque plusieurs réseaux intermédiaires doivent être traversés.

- **IPv4** et **IPv6** fournissent l'adressage logique ;
- **ICMP** transporte des messages de contrôle et aide au diagnostic ;
- **IGMP** gère l'appartenance des hôtes aux groupes multicast directement connectés.

### Processus IP présenté dans le support

1. La couche réseau reçoit les données de la couche transport.
2. Elle ajoute un en-tête IP contenant les adresses source et destination.
3. Chaque routeur intermédiaire lit l'adresse IP de destination.
4. Il recherche une entrée correspondante dans sa table de routage.
5. Il transmet le paquet selon l'instruction de cette entrée.
6. L'hôte destination vérifie l'adresse et accepte le paquet qui lui est destiné.

Les protocoles de routage, comme OSPF ou BGP, aident les routeurs à construire leurs tables. Ils ne transportent pas directement le contenu applicatif de l'utilisateur.

## Couche liaison de données : transférer les trames sur un lien

La couche liaison de données se situe entre les couches réseau et physique. Elle fournit une communication à l'intérieur d'un segment, organise les données en trames, utilise un adressage physique et assure la détection d'erreurs.

### Ethernet et adresse MAC

Ethernet est la technologie de liaison la plus courante dans les LAN. Le support retrace son évolution depuis les idées d'ALOHAnet et du CSMA/CD jusqu'à la normalisation IEEE 802.3, aux topologies en étoile et aux débits élevés.

Une adresse **MAC** :

- appartient à la couche liaison de données ;
- est aussi appelée adresse Ethernet, physique ou matérielle ;
- mesure 48 bits, soit 6 octets ;
- est représentée par 12 chiffres hexadécimaux ;
- permet aux switchs Ethernet de décider vers où transférer une trame.

### PPP sur les liaisons WAN

PPP est présenté comme un protocole de liaison WAN point à point, utilisé par exemple entre une agence et un siège.

Huawei met en avant les caractéristiques suivantes :

- prise en charge des liaisons synchrones et asynchrones ;
- extension possible, notamment avec PPPoE ;
- négociation des paramètres de liaison par **LCP** ;
- authentification facultative avec **PAP** ou **CHAP** ;
- négociation du protocole réseau par **NCP** ;
- absence de mécanisme de retransmission propre à PPP.

![États et négociations du processus d'établissement d'une liaison PPP](<../../../assets/diagrammes/01 Data Communication Network Basics/ppp-link-establishment-process-fr.svg>)

### Établissement d'une liaison PPP

1. **Dead** : la liaison physique n'est pas disponible.
2. **Establish** : LCP négocie notamment le mode, la MRU, le numéro magique et le mode d'authentification.
3. **Authenticate**, si l'authentification est configurée : PAP ou CHAP vérifie les pairs.
4. **Network** : NCP sélectionne et configure le protocole réseau ; IPCP peut négocier les paramètres IPv4 et MPLSCP ceux de MPLS.
5. **Terminate** : la liaison libère ses ressources avant de revenir à l'état Dead.

Un échec de négociation LCP, d'authentification ou de négociation NCP empêche le passage normal à l'étape suivante.

## Couche physique : transporter les bits sur le média

La couche physique convertit les données numériques en signaux adaptés au média : électriques, optiques ou électromagnétiques. Elle définit notamment les câbles, connecteurs, niveaux électriques, débits et interfaces.

| Média | Caractéristiques présentées dans le support |
| --- | --- |
| **Paire torsadée** | catégories 5, 5e ou 6, connecteur RJ45, portée maximale courante de 100 m ; variantes UTP et STP |
| **Fibre monomode** | un seul mode de propagation, grande bande passante et longues distances |
| **Fibre multimode** | plusieurs modes de propagation, dispersion plus importante et distances plus courtes |
| **Module optique** | conversion entre signaux électriques et optiques |
| **Câble hybride** | fibre et cuivre dans une même gaine pour transporter données et alimentation |
| **Onde électromagnétique** | transmission sans fil entre un point d'accès ou routeur sans fil et des terminaux |

Le support distingue les câbles hybrides 1.0 et 2.0 selon la manière dont leurs interfaces optiques et électriques sont intégrées. Ce détail doit être retenu comme une évolution de connectique, pas comme une nouvelle couche réseau.

## Lecture transversale d'une communication

Pour une requête web, chaque couche apporte une fonction différente :

| Couche | Question utile | Élément observé dans l'exemple |
| --- | --- | --- |
| Application | quel service est demandé ? | HTTP, DNS, URL |
| Transport | quelles applications communiquent ? | TCP ou UDP, ports source et destination |
| Réseau | quels hôtes et réseaux sont concernés ? | adresses IP, paquet, décision de routage |
| Liaison | comment atteindre le prochain nœud ? | trame Ethernet ou PPP, adresse MAC sur Ethernet |
| Physique | comment les bits sont-ils effectivement transmis ? | cuivre, fibre ou onde électromagnétique |

Cette lecture évite de mélanger des identifiants qui n'ont pas la même portée : un port identifie une application, une adresse IP un hôte ou une interface logique, et une adresse MAC une interface sur un segment Ethernet.

## Pratique TianSemi : classer les éléments d'un échange web

Le Lab Guide Huawei ne propose pas de laboratoire dédié à cette unité. La pratique suivante sert donc à consolider le modèle sans inventer de configuration CLI.

### Situation

Un poste connecté en Ethernet ouvre `http://www.example.com`. Il interroge d'abord un serveur DNS, puis envoie une requête HTTP vers le serveur web à travers un routeur.

### Travail demandé

1. Dessinez les cinq couches du modèle TCP/IP équivalent sur le client.
2. Placez DNS et HTTP à la couche application.
3. Associez UDP 53 à l'échange DNS et TCP 80 à l'exemple HTTP du support.
4. Identifiez les adresses IP source et destination du paquet.
5. Identifiez les adresses MAC utilisées sur le premier segment Ethernet.
6. Nommez la PDU à chaque couche : data, segment, packet, frame, bitstream.
7. Expliquez ce que le routeur consulte et ce que le switch consulte pour prendre leur décision de transfert.

### Vérification attendue

- le switch raisonne à partir des informations de liaison Ethernet, notamment les adresses MAC ;
- le routeur raisonne à partir de l'adresse IP de destination et de sa table de routage ;
- le serveur distingue les services au moyen du protocole de transport et des ports ;
- le média physique transporte les bits sans interpréter la requête HTTP.

## Diagnostic guidé par les couches

L'organisation en couches fournit une méthode de dépannage. Il faut localiser la fonction défaillante avant de chercher une cause précise.

| Symptôme | Couche à examiner en priorité | Question de départ |
| --- | --- | --- |
| aucun signal ou lien actif | Physique | le média, le connecteur et l'interface sont-ils opérationnels ? |
| communication impossible sur le segment Ethernet | Liaison | la trame peut-elle atteindre le prochain nœud et les adresses MAC sont-elles apprises ? |
| réseau distant injoignable | Réseau | l'adressage IP et le chemin de routage sont-ils cohérents ? |
| service inaccessible alors que l'hôte répond | Transport | le protocole et le port attendus sont-ils disponibles ? |
| nom de domaine non résolu | Application | le client DNS obtient-il une réponse correcte ? |

Cette méthode ne signifie pas qu'une panne appartient toujours à une seule couche. Elle fournit un ordre d'analyse et aide à choisir les observations pertinentes.

## À retenir pour l'examen HCIA-Datacom

- OSI comporte sept couches ; le modèle TCP/IP de référence en comporte quatre.
- Huawei utilise ensuite un modèle TCP/IP équivalent à cinq couches.
- Ce modèle équivalent est pédagogique et n'est pas un standard officiel.
- Les protocoles ouverts favorisent l'interopérabilité entre constructeurs.
- Les PDU présentées sont : data, segment, packet, frame et bitstream.
- TCP est orienté connexion et fiable ; UDP est sans connexion et ne garantit pas la livraison.
- IP fournit l'adressage logique et le transfert entre réseaux.
- Ethernet domine les LAN ; PPP est présenté pour les liaisons WAN point à point.
- LCP négocie la liaison PPP, PAP ou CHAP assure l'authentification facultative, et NCP négocie le protocole réseau.
- La couche physique transporte les bits sous forme de signaux adaptés au média.

## Révision TianSemi

1. Quelle différence faut-il établir entre un modèle de référence et un protocole réseau ?
2. Comment les sept couches OSI correspondent-elles aux quatre couches TCP/IP ?
3. Pourquoi le modèle TCP/IP équivalent utilisé par Huawei comporte-t-il cinq couches ?
4. Quelles PDU Huawei associe-t-il aux couches application, transport, réseau, liaison et physique ?
5. Comment les ports, les adresses IP et les adresses MAC identifient-ils des éléments différents d'une communication ?
6. Quelle différence essentielle faut-il retenir entre TCP et UDP ?
7. Comment un routeur traite-t-il l'adresse IP de destination d'un paquet ?
8. Quel est le rôle respectif de LCP, PAP ou CHAP, et NCP dans l'établissement d'une liaison PPP ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 54 à 78.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 7 à 13 pour le sommaire des laboratoires ; aucun laboratoire n'est dédié à cette unité.
