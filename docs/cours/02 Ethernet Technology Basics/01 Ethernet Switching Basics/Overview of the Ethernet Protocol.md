---
id: HUA-HCIA-DC-011
title: "Overview of the Ethernet Protocol"
official_title: "Overview of the Ethernet Protocol"
course_unit: "2.1.1"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-10
updated: 2026-08-10
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 153-160"
tags: [huawei, hcia, datacom, ethernet, collision-domain, broadcast-domain, nic]
prerequisites:
  - "[[Network Reference Models and Standard Protocols]]"
related:
  - "[[Overview of Ethernet Frames]]"
  - "[[Working Principles of Ethernet Switches]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble du protocole Ethernet (Overview of the Ethernet Protocol)

## Objectifs officiels couverts

Cette unité introduit les notions nécessaires pour expliquer :

- le rôle d'Ethernet à la couche liaison de données ;
- la différence entre un domaine de collision et un domaine de broadcast ;
- l'évolution d'un Ethernet partagé vers un réseau commuté ;
- le rôle de la carte réseau Ethernet.

![Évolution d'un réseau Ethernet partagé vers un réseau commuté](<../../../assets/diagrammes/02 Ethernet Technology Basics/ethernet-shared-to-switched-network-fr.svg>)

## Ethernet à la couche liaison de données

La communication réseau est un processus d'encapsulation par couches. Quand les données des protocoles supérieurs atteignent la couche liaison, celle-ci ajoute les informations nécessaires pour former une **trame** transmissible sur le lien.

Ethernet est la technologie d'encapsulation dominante à cette couche. Les équipements qui assurent la communication Ethernet locale traitent donc des **trames Ethernet** et des **adresses MAC**.

Le support Huawei présente deux générations de réseau :

- l'Ethernet historique, fondé sur un média partagé et le mécanisme CSMA/CD ;
- l'Ethernet commuté, dans lequel un switch sépare les domaines de collision.

## Ethernet partagé et CSMA/CD

Dans un réseau partagé, plusieurs nœuds utilisent le même média et se disputent la même bande passante. Une transmission peut être reçue par tous les autres nœuds du segment. Si deux équipements émettent simultanément, leurs signaux entrent en collision.

Le mécanisme **CSMA/CD** (*Carrier Sense Multiple Access with Collision Detection*) peut être résumé ainsi :

1. écouter le média avant d'émettre ;
2. émettre seulement s'il est libre ;
3. continuer à surveiller le média pendant l'émission ;
4. arrêter l'émission si une collision est détectée ;
5. signaler la collision, attendre un délai aléatoire, puis retransmettre.

Ce mécanisme répond au problème d'un média partagé. Il ne constitue pas le principe normal d'un lien Ethernet moderne commuté en duplex intégral.

## Domaine de collision

Un **domaine de collision** regroupe les nœuds connectés au même média partagé. Ils se partagent la bande passante et peuvent provoquer des collisions lorsqu'ils émettent en même temps.

| Réseau | Organisation des domaines de collision |
| --- | --- |
| Média partagé avec hub | Un seul domaine pour tous les nœuds connectés |
| Réseau commuté | Chaque port du switch correspond à un domaine de collision indépendant |

En isolant les domaines de collision, le switch empêche qu'une collision se propage d'un port à un autre et améliore fortement les performances d'Ethernet.

## Domaine de broadcast

Un **domaine de broadcast de couche 2** est l'ensemble de la zone atteinte par une trame de broadcast. Une trame dont l'adresse MAC de destination est `FF-FF-FF-FF-FF-FF` doit être traitée par tous les nœuds de ce domaine.

Un switch classique :

- isole les domaines de collision port par port ;
- transmet une trame de broadcast par tous les autres ports concernés ;
- ne sépare donc pas, à lui seul, le domaine de broadcast.

La séparation logique des domaines de broadcast sera étudiée avec les **VLAN**.

## Carte réseau Ethernet

La **NIC** (*Network Interface Card*) est le composant qui relie un équipement au réseau externe. Dans ce cours, les NIC considérées sont des cartes Ethernet.

Une interface réseau peut aussi être appelée **port**. Sur un ordinateur comme sur un switch, la NIC assure le passage entre les données traitées par l'équipement et le flux de bits transmis sur le média.

Chaque NIC Ethernet possède une adresse MAC qui l'identifie sur le réseau local. La structure et les types d'adresses MAC sont détaillés dans la note suivante.

## Erreurs de raisonnement fréquentes

| Confusion | Correction |
| --- | --- |
| « Un switch supprime tous les broadcasts. » | Il les transmet normalement dans le même domaine de broadcast. |
| « Tous les ports d'un switch sont dans le même domaine de collision. » | Chaque port constitue un domaine de collision distinct. |
| « CSMA/CD segmente le réseau. » | CSMA/CD gère les collisions sur un média partagé ; le switch les isole. |
| « Une NIC existe uniquement dans un PC. » | Les interfaces Ethernet des switchs et routeurs utilisent elles aussi des NIC. |

## Pratique TianSemi : lire une topologie

Considérez quatre hôtes raccordés à un hub, puis les mêmes quatre hôtes raccordés chacun à un port de switch.

1. Comptez les domaines de collision dans les deux topologies.
2. Déterminez quels hôtes reçoivent une trame envoyée à `FF-FF-FF-FF-FF-FF`.
3. Expliquez pourquoi le switch améliore les performances sans réduire, à ce stade, le domaine de broadcast.

Résultat attendu : le hub forme un seul domaine de collision ; le switch en forme un par port ; sans VLAN ni équipement de couche 3, tous les hôtes restent dans le même domaine de broadcast.

## À retenir pour l'examen HCIA-Datacom

- Ethernet encapsule les données de couche supérieure sous forme de trames à la couche liaison.
- CSMA/CD est associé à l'Ethernet partagé et à la gestion des collisions.
- Un hub ne sépare pas les domaines de collision.
- Chaque port de switch forme un domaine de collision indépendant.
- Un switch transmet les broadcasts dans le domaine de broadcast.
- `FF-FF-FF-FF-FF-FF` est l'adresse MAC de broadcast.
- Une NIC connecte l'équipement au réseau et possède une adresse MAC.

## Résumé fidèle du cours

Ethernet est la technologie dominante de couche liaison. Les réseaux Ethernet historiques partageaient un média et utilisaient CSMA/CD. Les switchs ont remplacé les hubs parce qu'ils isolent les domaines de collision, mais ils transmettent toujours le trafic de broadcast au sein du même domaine. La NIC constitue l'interface entre l'équipement et le réseau Ethernet.

## Révision TianSemi

1. Quel est le rôle d'Ethernet dans le processus d'encapsulation ?
2. Comment fonctionne CSMA/CD lorsqu'une collision est détectée ?
3. Quelle différence existe entre un domaine de collision et un domaine de broadcast ?
4. Combien de domaines de collision crée un switch à quatre ports tous utilisés ?
5. Comment un switch traite-t-il normalement une trame destinée à `FF-FF-FF-FF-FF-FF` ?
6. Quel est le rôle d'une NIC Ethernet ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 153 à 160.
