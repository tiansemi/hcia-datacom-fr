---
id: HUA-HCIA-DC-057
title: "What Is a DC"
official_title: "What Is a DC"
course_unit: "5.1.1"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-11
updated: 2026-08-11
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 738-746"
tags: [huawei, hcia, datacom, data-center, dcn, computing, storage, infrastructure]
prerequisites:
  - "[[Bidirectional NAT]]"
related:
  - "[[DC Development and Evolution]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Qu'est-ce qu'un data center ? (What Is a DC)

## Rôle d'un data center

Un **data center (DC)** est un lieu où une organisation traite et stocke de grandes quantités de données. Il centralise des capacités que les postes de travail isolés ne peuvent plus fournir à l'échelle de l'entreprise.

Un DC ne se limite pas à une salle de serveurs. Il réunit :

- bâtiments et locaux techniques ;
- alimentation, refroidissement et protection incendie ;
- serveurs, systèmes de stockage et équipements réseau ;
- connexions de données ;
- supervision et sécurité ;
- plateformes et applications.

## Les cinq niveaux

![Les cinq niveaux constitutifs d'un data center](<../../../assets/diagrammes/05 Data Center Network Basics/dc-five-layers-fr.svg>)

| Niveau | Contenu |
| --- | --- |
| L0 | Génie civil, campus, bâtiment et raccordements municipaux. |
| L1 | Alimentation, refroidissement, salle IT et systèmes auxiliaires. |
| L2 | Calcul, stockage et réseau. |
| L3 | Virtualisation, cloud, big data, bases de données et systèmes d'exploitation. |
| L4 | Applications métier et services numériques. |

Le cours développe principalement L2 et les niveaux supérieurs.

## Trois systèmes matériels

Huawei compare le DC moderne à un ordinateur agrandi :

- le **système de calcul**, formé des serveurs, traite les données ;
- le **système de stockage** conserve données et informations ;
- le **DCN** interconnecte calcul, stockage et services de sécurité.

Toutes les communications entre calcul et stockage passent par le **Data Center Network**. Le DCN transporte donc les services du data center et assure leur acheminement.

## Connexions externes

Un DCN peut rejoindre :

- les réseaux de campus et les branches de l'entreprise ;
- Internet ou un WAN ;
- un autre data center au moyen d'un **Data Center Interconnect (DCI)**.

Une **SAN** est un réseau dédié à haut débit reliant des serveurs à des équipements de stockage centralisés.

## Spine, leaf et services

Dans l'architecture illustrée par Huawei :

- les commutateurs **spine** fournissent la commutation IP rapide du cœur ;
- les nœuds **leaf** raccordent serveurs, pare-feu, load balancers et autres équipements ;
- l'architecture exacte dépend du scénario de service.

## Pratique TianSemi : cartographier un DC

Pour un service de commerce en ligne, classez dans L0 à L4 : bâtiment, alimentation, refroidissement, serveurs, stockage, switches, hyperviseur, base de données et application. Tracez ensuite les flux client, application, base de données et sauvegarde en indiquant ceux que transporte le DCN.

## À retenir pour l'examen HCIA-Datacom

- Un DC traite et stocke massivement les données d'une organisation.
- L0 à L4 couvrent bâtiment, infrastructure, matériel, plateformes et applications.
- Calcul, stockage et DCN constituent le cœur de la couche matérielle.
- Le DCN transporte les services et relie les ressources du data center.
- DCI relie plusieurs data centers ; SAN relie serveurs et stockage.
- Spine assure le transit rapide et leaf raccorde les ressources.

## Révision TianSemi

1. Pourquoi une entreprise construit-elle ou loue-t-elle un data center ?
2. Quels éléments montrent qu'un DC est plus qu'une salle de serveurs ?
3. Que contiennent les niveaux L0 à L4 ?
4. Quel est le rôle du DCN entre calcul et stockage ?
5. Quelle différence existe entre DCI et SAN ?
6. Quels équipements un leaf peut-il raccorder ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 738 à 746.
