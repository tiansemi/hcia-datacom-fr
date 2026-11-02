---
id: HUA-HCIA-DC-007
title: Data Communication Process
aliases:
  - Data Encapsulation and Decapsulation
official_title: Data Communication Process
course_unit: 1.2.3
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
updated: 2026-08-10
source: HCIA-Datacom V2.0 Training Material
source_pages: PDF 79-85
tags:
  - huawei
  - hcia
  - datacom
  - encapsulation
  - decapsulation
prerequisites:
  - "[[Network Reference Models and Standard Protocols]]"
related:
  - "[[01 Data Communication Network Basics/Data Communication Process]]"
lab_required: false
publication_status: not-started
site_url:
---

# Processus de communication de données (Data Communication Process)

## Place dans le cours

L'unité **1.2.3 Data Communication Process** applique le modèle TCP/IP équivalent à une communication web. Elle suit les données depuis le navigateur de l'émetteur jusqu'au serveur web et distingue trois phases :

1. l'**encapsulation** sur l'hôte émetteur ;
2. la **transmission et le traitement** dans le réseau intermédiaire ;
3. la **décapsulation** sur l'hôte récepteur.

Cette unité complète l'introduction générale de l'unité 1.1.3 en nommant les PDU et les informations ajoutées à chaque couche.

## Vue d'ensemble du trajet

![Encapsulation chez le client, traitement par le switch et le routeur, puis décapsulation sur le serveur](<../../../assets/diagrammes/01 Data Communication Network Basics/data-encapsulation-transit-decapsulation-fr.svg>)

L'exemple Huawei utilise un navigateur qui accède au site web de Huawei. Le chemin logique peut être lu ainsi :

```text
Données applicatives
  -> segment TCP
    -> paquet IP
      -> trame Ethernet
        -> flux de bits et signaux
          -> réseau intermédiaire
        -> flux de bits reçus
      -> trame Ethernet
    -> paquet IP
  -> segment TCP
-> données remises au serveur web
```

## Encapsulation sur l'émetteur

L'**encapsulation** consiste à transmettre les données vers les couches inférieures. Chaque couche ajoute les informations dont son protocole a besoin pour accomplir sa fonction.

| Étape | Traitement présenté dans le support | PDU obtenue |
| --- | --- | --- |
| Application | le navigateur appelle HTTP et produit les données de la requête | **data** |
| Transport | TCP ajoute notamment les ports source et destination | **segment** |
| Réseau | IPv4 ou IPv6 ajoute l'en-tête IP | **packet** |
| Liaison de données | Ethernet ajoute son en-tête et un trailer contenant le FCS | **frame** |
| Physique | les données numériques sont converties en signaux adaptés au média | **bitstream** |

### Couche application

Le navigateur utilise HTTP pour préparer la demande. Le schéma Huawei ne représente pas explicitement l'en-tête HTTP : il simplifie l'ensemble sous le nom **Data**.

La couche application ne connaît pas encore les détails du média physique. Elle remet ses données au module de transport demandé par le protocole applicatif.

### Couche transport

HTTP utilise TCP dans l'exemple. Le module TCP ajoute un en-tête contenant notamment :

- un port source qui identifie l'application cliente ;
- un port destination qui identifie le service serveur.

L'ensemble formé par l'en-tête TCP et les données devient un **segment**.

### Couche réseau

Le segment est remis au module IPv4 sur un réseau IPv4 ou au module IPv6 sur un réseau IPv6. Le module ajoute l'en-tête IP, notamment les adresses source et destination nécessaires au transfert entre réseaux.

La PDU devient un **paquet** (*packet*).

### Couche liaison de données

Dans l'exemple, Ethernet reçoit le paquet IP et ajoute :

- un en-tête Ethernet ;
- un trailer contenant le **FCS** (*Frame Check Sequence*), utilisé pour la détection d'erreurs.

La PDU devient une **trame** (*frame*). Le paquet IP constitue alors la charge utile de cette trame.

### Couche physique

La couche physique convertit les données numériques en signaux adaptés au média :

- signaux électriques sur un support en cuivre ;
- signaux optiques sur une fibre ;
- ondes électromagnétiques sur un réseau sans fil.

Le support nomme la PDU transmise **bitstream**, c'est-à-dire un flux de bits.

## En-tête, trailer et charge utile

Trois termes structurent la lecture de l'encapsulation :

| Terme | Sens |
| --- | --- |
| **Header** | informations placées avant la charge utile par un protocole |
| **Trailer** | informations placées après la charge utile ; Ethernet ajoute notamment le FCS |
| **Payload** | données transportées par la PDU courante, qui correspondent généralement à la PDU de la couche supérieure |

Ainsi, le segment TCP devient la charge utile du paquet IP, puis le paquet IP devient la charge utile de la trame Ethernet.

> [!important]
> Encapsuler ne signifie pas chiffrer. L'encapsulation organise les informations nécessaires à chaque couche ; le chiffrement est une fonction distincte.

## Traitement dans le réseau intermédiaire

Les données complètement encapsulées sont transmises sur le réseau. Huawei distingue le traitement des équipements en fonction de la couche utilisée pour prendre la décision de transfert.

### Switch de couche 2

Dans la représentation simplifiée du support, un switch :

1. reçoit les bits sur une interface ;
2. reconstitue et traite la trame ;
3. utilise les informations de couche 2 nécessaires à la commutation ;
4. transmet la trame vers l'interface appropriée.

La décision de commutation Ethernet repose principalement sur l'adresse MAC de destination et la table d'adresses MAC.

### Routeur de couche 3

Dans la représentation simplifiée du support, un routeur :

1. reçoit et traite la trame du lien entrant ;
2. examine les informations du paquet IP ;
3. prend une décision de routage à partir de la destination et de sa table de routage ;
4. prépare la transmission sur le lien suivant.

Le support renvoie les détails à **Ethernet Switching Basics** et **IP Routing Basics**.

### Portée des informations

Les informations n'ont pas toutes la même portée :

- les informations de liaison décrivent la livraison sur le lien courant ;
- les informations IP permettent le transfert entre l'émetteur et le réseau de destination ;
- les ports TCP identifient les applications aux extrémités.

Cette distinction explique pourquoi un switch, un routeur et un serveur n'examinent pas exactement les mêmes champs.

## Décapsulation sur le récepteur

La **décapsulation** est le processus inverse. Le serveur reçoit les données depuis les couches basses et les remet progressivement à la couche application.

1. La couche physique reçoit les signaux et restitue le flux de bits.
2. La couche liaison reconstruit la trame, vérifie les informations nécessaires et retire l'encapsulation Ethernet.
3. La couche réseau traite l'en-tête IP et remet la charge utile au protocole indiqué.
4. La couche transport traite le segment TCP et utilise le port destination pour identifier le service.
5. La couche application reçoit les données destinées au programme serveur web.

Les champs des en-têtes guident donc la remise de la PDU au bon protocole, puis à la bonne application.

## Collaboration entre modules

Huawei insiste sur une idée centrale : la génération et la transmission des données exigent la collaboration de plusieurs modules, mais chaque module reste responsable de sa propre fonction.

| Module | Responsabilité dans l'exemple |
| --- | --- |
| HTTP | construire et interpréter la demande web |
| TCP | identifier les applications et fournir le transport fiable utilisé par HTTP |
| IP | transférer le paquet entre les hôtes et réseaux |
| Ethernet | transporter la trame sur un segment de réseau |
| Physique | convertir et transporter les bits sous forme de signaux |

Cette séparation facilite le développement, la normalisation, la compatibilité et le dépannage.

## Pratique TianSemi : reconstruire une encapsulation

Le Lab Guide Huawei ne propose pas de laboratoire dédié à cette unité. L'exercice suivant permet néanmoins de vérifier la compréhension sans inventer de configuration CLI.

### Situation

Un poste Ethernet envoie une requête HTTP utilisant TCP et IPv4 vers un serveur situé sur un autre réseau. Le trafic traverse un switch puis un routeur.

### Travail demandé

1. Écrivez les cinq PDU dans l'ordre d'encapsulation.
2. Indiquez quelle couche ajoute les ports source et destination.
3. Indiquez quelle couche ajoute les adresses IP.
4. Indiquez quelle couche ajoute l'en-tête Ethernet et le FCS.
5. Expliquez ce que le switch examine pour commuter la trame.
6. Expliquez ce que le routeur examine pour choisir le prochain chemin.
7. Reconstituez l'ordre de décapsulation sur le serveur.

### Résultat attendu

```text
Application : data
Transport   : TCP header + data = segment
Réseau      : IP header + segment = packet
Liaison     : Ethernet header + packet + FCS = frame
Physique    : bitstream converti en signaux
```

Le switch intervient principalement à la couche liaison ; le routeur traite la couche réseau ; le serveur poursuit la décapsulation jusqu'à la couche application.

## Diagnostic guidé par l'encapsulation

Lorsqu'une communication échoue, la structure en couches aide à poser les bonnes questions :

| Observation | Question de diagnostic |
| --- | --- |
| aucun signal reçu | le média et la couche physique fonctionnent-ils ? |
| trame rejetée | le FCS ou les informations Ethernet sont-ils valides ? |
| paquet sans chemin | l'adressage IP et la décision de routage sont-ils cohérents ? |
| serveur atteint mais service inaccessible | le protocole de transport et le port destination sont-ils corrects ? |
| données reçues mais requête non comprise | le protocole applicatif interprète-t-il correctement les données ? |

L'objectif n'est pas de conclure qu'une panne appartient toujours à une seule couche, mais de localiser la fonction à examiner en priorité.

## À retenir pour l'examen HCIA-Datacom

- L'encapsulation descend de la couche application vers la couche physique.
- La décapsulation remonte de la couche physique vers la couche application.
- Les PDU présentées sont : data, segment, packet, frame et bitstream.
- TCP ajoute notamment les ports source et destination.
- IP ajoute les informations nécessaires au transfert entre réseaux.
- Ethernet ajoute un en-tête et un trailer contenant le FCS.
- Un switch traite principalement les informations de couche 2 ; un routeur traite celles de couche 3.
- La charge utile d'une couche correspond généralement à la PDU reçue de la couche supérieure.
- Encapsulation et chiffrement sont deux opérations différentes.

## Révision TianSemi

1. Dans quel ordre les cinq couches encapsulent-elles une requête web sur l'émetteur ?
2. Comment les PDU sont-elles nommées aux couches application, transport, réseau, liaison et physique ?
3. Quelle différence faut-il établir entre un header, un trailer et une payload ?
4. Quelles informations TCP permettent d'identifier les applications aux deux extrémités ?
5. Pourquoi un switch et un routeur n'examinent-ils pas les mêmes informations ?
6. Quel est le rôle du FCS ajouté par Ethernet ?
7. Dans quel ordre le serveur décapsule-t-il les données reçues ?
8. Pourquoi l'encapsulation améliore-t-elle la compatibilité entre protocoles, matériels et logiciels ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 79 à 85.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 7 à 13 pour le sommaire des laboratoires ; aucun laboratoire n'est dédié à cette unité.
