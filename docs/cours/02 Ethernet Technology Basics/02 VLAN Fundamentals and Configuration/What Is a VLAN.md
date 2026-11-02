---
id: HUA-HCIA-DC-014
title: "What Is a VLAN?"
official_title: "What Is a VLAN?"
course_unit: "2.2.1"
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
source_pages: "PDF 189-195"
tags: [huawei, hcia, datacom, vlan, broadcast-domain, layer-2]
prerequisites:
  - "[[Working Principles of Ethernet Switches]]"
related:
  - "[[Basic Concepts of VLANs]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Qu'est-ce qu'un VLAN ? (What Is a VLAN?)

## Problème d'un réseau Ethernet traditionnel

Un switch diffuse une trame de broadcast et une trame unicast inconnue par les autres ports du domaine concerné. Dans un grand réseau de couche 2, ce comportement peut transmettre des trames à de nombreux hôtes qui n'en sont pas les destinataires.

Plus le domaine de broadcast devient vaste, plus les effets suivants sont importants :

- trafic inutile et consommation de bande passante ;
- exposition de trames à davantage de terminaux ;
- difficulté à isoler les services et les incidents ;
- dégradation possible des performances et de la robustesse.

![Segmentation d'un grand domaine de broadcast en plusieurs VLAN](<../../../assets/diagrammes/02 Ethernet Technology Basics/vlan-broadcast-domain-segmentation-fr.svg>)

## Définition d'un VLAN

Un **VLAN** (*Virtual Local Area Network*) divise logiquement un grand domaine de broadcast en plusieurs domaines plus petits sur une infrastructure commutée.

Chaque VLAN constitue un domaine de broadcast indépendant :

- les hôtes d'un même VLAN peuvent communiquer directement à la couche 2 ;
- les hôtes de VLAN différents ne communiquent pas directement à la couche 2 ;
- une communication entre VLAN nécessite un traitement de couche 3 ;
- les broadcasts restent confinés dans leur VLAN.

Cette segmentation est logique et n'est pas limitée par la position géographique des utilisateurs. Des hôtes raccordés à des switchs différents peuvent appartenir au même VLAN si l'infrastructure transporte ce VLAN entre les équipements.

## Avantages présentés par Huawei

| Avantage | Effet |
| --- | --- |
| Groupes de travail flexibles | Regrouper logiquement des utilisateurs situés à différents endroits |
| Broadcasts restreints | Réduire le trafic inutile et économiser les ressources réseau |
| Sécurité LAN améliorée | Séparer le trafic de groupes qui ne doivent pas communiquer directement |
| Robustesse accrue | Limiter l'effet d'une défaillance de couche 2 à un VLAN |

Un VLAN participe à la segmentation, mais ne remplace pas à lui seul les contrôles de sécurité de couche 3 ou les politiques d'accès.

## Pratique TianSemi : proposer une segmentation

Une entreprise possède un service Finance, un service Technique et des postes invités, tous raccordés aux mêmes switchs.

1. Proposez un VLAN pour chaque groupe.
2. Indiquez qui reçoit un broadcast émis par un poste Finance.
3. Expliquez pourquoi Finance ne peut pas joindre directement Technique à la couche 2.
4. Identifiez la fonction supplémentaire nécessaire pour autoriser une communication contrôlée entre les deux VLAN.

Résultat attendu : trois domaines de broadcast distincts ; seul le VLAN Finance reçoit son broadcast ; un équipement ou une fonction de couche 3 est nécessaire pour le trafic inter-VLAN.

## Erreurs fréquentes

- Confondre VLAN et sous-réseau IP : ils sont souvent associés dans une conception, mais l'un segmente la couche 2 et l'autre structure l'adressage de couche 3.
- Croire que deux hôtes du même VLAN doivent être proches physiquement : l'appartenance est logique.
- Croire qu'un VLAN route le trafic : le routage inter-VLAN relève de la couche 3.
- Croire qu'un switch crée automatiquement plusieurs domaines de broadcast : sans configuration VLAN, les ports appartiennent par défaut au VLAN 1 dans le contexte du cours.

## À retenir pour l'examen HCIA-Datacom

- Un VLAN est un domaine de broadcast logique.
- Les communications directes de couche 2 sont limitées au même VLAN.
- Les communications entre VLAN passent par la couche 3.
- Les VLAN ne dépendent pas de la localisation géographique.
- Ils réduisent les broadcasts inutiles et améliorent isolation et robustesse.

## Révision TianSemi

1. Quel problème principal des grands réseaux de couche 2 les VLAN résolvent-ils ?
2. Pourquoi dit-on qu'un VLAN est un domaine de broadcast ?
3. Deux hôtes de VLAN différents peuvent-ils communiquer directement à la couche 2 ?
4. Quels sont les quatre avantages des VLAN cités par Huawei ?
5. L'appartenance à un VLAN dépend-elle de la position géographique ?
6. Quelle fonction est nécessaire pour communiquer entre deux VLAN ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 189 à 195.
