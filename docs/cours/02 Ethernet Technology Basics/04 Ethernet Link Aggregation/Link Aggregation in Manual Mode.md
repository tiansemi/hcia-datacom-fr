---
id: HUA-HCIA-DC-024
title: "Link Aggregation in Manual Mode"
official_title: "Link Aggregation in Manual Mode"
course_unit: "2.4.2"
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
source_pages: "PDF 309-312"
tags: [huawei, hcia, datacom, eth-trunk, manual-mode, load-balancing]
prerequisites:
  - "[[Basic Concepts of Link Aggregation]]"
related:
  - "[[Link Aggregation in LACP Mode]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Agrégation de liens en mode manuel (Link Aggregation in Manual Mode)

## Fonctionnement

En mode manuel, l'administrateur crée l'Eth-Trunk et ajoute explicitement les interfaces membres. Aucun paquet LACP n'est échangé pour vérifier l'autre extrémité.

En fonctionnement normal :

- tous les liens membres opérationnels sont actifs ;
- les flux sont répartis entre les liens actifs ;
- lorsqu'un membre tombe physiquement Down, les autres reprennent sa part de trafic.

Ce mode est utile si l'un des équipements est ancien ou ne prend pas en charge LACP.

![Différence de contrôle entre l'agrégation manuelle et LACP](<../../../assets/diagrammes/02 Ethernet Technology Basics/manual-vs-lacp-detection-fr.svg>)

## Limites du mode manuel

L'absence de négociation empêche l'équipement de vérifier que les interfaces distantes :

- appartiennent au même équipement ;
- sont ajoutées au même Eth-Trunk ;
- forment réellement un chemin de bout en bout cohérent.

Une erreur de câblage peut par exemple relier un membre local à un troisième switch. Le trafic réparti sur ce lien atteint alors le mauvais équipement.

Le mode manuel s'appuie aussi principalement sur l'état physique du port. Un lien peut rester Up tout en étant incapable de transférer correctement ; il continue alors d'être choisi et provoque des pertes intermittentes.

## Contrôle avant activation

1. Vérifier que chaque câble relie bien les deux mêmes équipements.
2. Associer les ports homologues au même ID Eth-Trunk.
3. Configurer le même mode des deux côtés.
4. Comparer débit, duplex, MTU et services appliqués.
5. Tester la perte d'un membre puis son rétablissement.

## Quand ne pas choisir le mode manuel

Préférer LACP lorsque les deux équipements le prennent en charge et que l'on a besoin :

- de valider automatiquement l'identité et la cohérence du pair ;
- de maintenir des liens inactifs comme secours ;
- de sélectionner un nombre maximal de membres actifs ;
- de détecter certaines incohérences invisibles au seul état physique.

## Pratique TianSemi : trouver l'erreur

Quatre ports de `SW1` appartiennent à Eth-Trunk 1. Trois sont reliés à `SW2`, mais le quatrième est accidentellement relié à `SW3` et reste physiquement Up.

1. Expliquez pourquoi le mode manuel peut sélectionner ce lien.
2. Prévoyez le symptôme : certains flux fonctionnent, d'autres échouent selon le hachage.
3. Décrivez les contrôles de câblage et de configuration nécessaires.
4. Expliquez ce que la négociation LACP apporterait.

## À retenir pour l'examen HCIA-Datacom

- Le mode manuel n'utilise pas LACP.
- Tous les liens membres valides sont normalement actifs.
- La défaillance physique d'un membre redistribue le trafic sur les autres.
- Le mode manuel ne vérifie pas automatiquement la cohérence du pair.
- Un port Up peut être incapable de transférer et rester néanmoins sélectionné.

## Révision TianSemi

1. Dans quel cas le mode manuel est-il nécessaire ?
2. Combien de liens membres participent normalement au transfert ?
3. Que se passe-t-il lorsqu'un membre actif tombe physiquement Down ?
4. Quelles incohérences distantes le mode manuel ne détecte-t-il pas ?
5. Pourquoi un port physiquement Up peut-il provoquer des pertes ?
6. Quel avantage de contrôle LACP apporte-t-il ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 309 à 312.
