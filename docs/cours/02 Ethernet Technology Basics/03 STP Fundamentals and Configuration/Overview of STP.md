---
id: HUA-HCIA-DC-018
title: "Overview of STP"
official_title: "Overview of STP"
course_unit: "2.3.1"
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
source_pages: "PDF 243-254"
tags: [huawei, hcia, datacom, stp, layer-2-loop, broadcast-storm, mac-flapping]
prerequisites:
  - "[[VLAN Configuration Examples]]"
related:
  - "[[Basic Concepts of STP]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble de STP (Overview of STP)

## Redondance et boucles de couche 2

Une liaison unique entre un switch d'accès et l'amont constitue un point de défaillance : si le lien ou l'équipement tombe en panne, les utilisateurs sont isolés. Ajouter une liaison redondante améliore la disponibilité, mais peut former une boucle de couche 2.

Une boucle peut aussi provenir d'un mauvais câblage ou de liens parallèles qui auraient dû être agrégés dans une interface logique Eth-Trunk.

![STP conserve la redondance tout en bloquant logiquement un chemin](<../../../assets/diagrammes/02 Ethernet Technology Basics/stp-redundancy-loop-free-tree-fr.svg>)

## Conséquences d'une boucle

### Tempête de broadcast

Les trames BUM — broadcast, unicast inconnue et multicast — sont diffusées par les switchs. En présence d'une boucle, elles peuvent circuler et se multiplier sans fin, car l'en-tête de couche 2 ne contient pas de champ équivalent au TTL IP. La bande passante et les ressources des switchs sont alors rapidement consommées.

### MAC address flapping

Un switch apprend une MAC source sur le port qui reçoit la trame. Si la même trame revient par un autre chemin, la même MAC est apprise alternativement sur plusieurs ports. Cette instabilité, appelée **MAC address flapping**, rend la table MAC incohérente et perturbe le transfert.

### Trames dupliquées

Plusieurs copies d'une même trame peuvent atteindre un destinataire, ce qui dégrade la qualité des services et peut provoquer une interruption complète du réseau.

## Rôle de STP

**STP** (*Spanning Tree Protocol*) permet aux switchs d'échanger des BPDU, de calculer une topologie sans boucle et de bloquer logiquement un ou plusieurs ports redondants.

Le lien physique de secours reste présent. Si la topologie change ou si le chemin actif tombe en panne, STP recalcule l'arbre et peut réactiver un port auparavant bloqué.

STP apporte donc simultanément :

- prévention des boucles de couche 2 ;
- maintien d'une redondance physique ;
- adaptation dynamique aux changements de topologie.

## Boucle de couche 2 et boucle de couche 3

| Boucle de couche 2 | Boucle de couche 3 |
| --- | --- |
| Souvent causée par la redondance ou un mauvais câblage | Causée par une boucle de routage |
| Les trames ne possèdent pas de TTL | Le TTL limite la circulation des paquets IP |
| Nécessite STP ou un mécanisme équivalent | Les protocoles de routage possèdent des mécanismes anti-boucle |

## Emplacement dans un campus

Le cours positionne STP dans la partie commutée de couche 2 du campus. Il ne remplace ni le routage de la partie couche 3 ni les mécanismes de haute disponibilité associés.

## Pratique TianSemi : diagnostiquer une boucle

Sur une topologie de trois switchs formant un triangle, imaginez qu'aucun mécanisme STP ne bloque de port.

1. Suivez une trame de broadcast à travers les trois liens.
2. Expliquez pourquoi chaque copie génère d'autres copies.
3. Prévoyez l'effet sur la table MAC d'un switch.
4. Choisissez un lien qui pourrait être bloqué logiquement tout en conservant un chemin entre chaque paire de switchs.

Ne reproduisez pas volontairement une boucle sur un réseau de production. Utilisez un simulateur ou un lab isolé.

## Troubleshooting : indices d'une boucle

- utilisation anormalement élevée des liens et des processeurs ;
- volume massif de broadcasts ou d'unicasts inconnues ;
- adresses MAC qui changent rapidement de port ;
- connectivité instable sur une grande partie du domaine de couche 2 ;
- disparition du problème après déconnexion d'un lien redondant.

Ces indices orientent le diagnostic, mais il faut vérifier la topologie, les rôles STP et les ports affectés avant de conclure.

## À retenir pour l'examen HCIA-Datacom

- La redondance de couche 2 peut créer des boucles.
- Une boucle provoque tempêtes BUM, MAC flapping et trames dupliquées.
- Les trames Ethernet n'ont pas de TTL.
- STP échange des BPDU, calcule un arbre et bloque logiquement les chemins redondants.
- Un port bloqué peut être réactivé après une défaillance du chemin actif.

## Révision TianSemi

1. Pourquoi ajoute-t-on des liens redondants dans un réseau commuté ?
2. Quels trois problèmes majeurs une boucle de couche 2 provoque-t-elle ?
3. Que signifie BUM ?
4. Pourquoi une trame peut-elle circuler indéfiniment en couche 2 ?
5. Comment STP transforme-t-il une topologie physique bouclée ?
6. Quelle différence fondamentale existe entre une boucle de couche 2 et une boucle IP ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 243 à 254.
