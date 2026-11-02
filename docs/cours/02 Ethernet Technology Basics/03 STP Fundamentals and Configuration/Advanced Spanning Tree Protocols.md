---
id: HUA-HCIA-DC-022
title: "Advanced Spanning Tree Protocols"
official_title: "Advanced Spanning Tree Protocols"
course_unit: "2.3.5"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "intermediate"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-10
updated: 2026-08-10
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 289-297"
tags: [huawei, hcia, datacom, stp, rstp, vbst, mstp, mst-instance]
prerequisites:
  - "[[Basic STP Configuration]]"
related:
  - "[[Basic Concepts of VLANs]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Protocoles Spanning Tree avancés (Advanced Spanning Tree Protocols)

## Limites de STP classique

STP empêche les boucles mais converge lentement parce qu'il dépend fortement de temporisateurs. Des changements fréquents de topologie peuvent donc provoquer des interruptions répétées.

Le protocole classique distingue aussi cinq états dont trois — Blocking, Listening et Learning — ne transfèrent pas le trafic utilisateur. Les rôles et les états ne sont pas clairement séparés, ce qui complexifie la compréhension et le déploiement.

## RSTP

**RSTP**, défini par IEEE 802.1w, améliore STP tout en restant compatible avec lui.

Ses apports principaux :

- convergence plus rapide grâce notamment au mécanisme Proposal/Agreement ;
- ajout des rôles alternate et backup ;
- réduction de cinq états à trois selon le transfert et l'apprentissage ;
- port edge, qui passe immédiatement en Forwarding lorsqu'il relie un terminal ;
- traitement optimisé des BPDU et des changements de topologie.

Huawei déconseille de mélanger STP et RSTP, car l'interopérabilité ferait perdre l'avantage de convergence rapide sur les parties concernées.

## Limite commune de STP et RSTP

Tous les VLAN partagent un seul arbre. Lorsqu'un lien est bloqué, il ne transporte le trafic d'aucun VLAN, ce qui empêche l'équilibrage inter-VLAN et gaspille une partie de la bande passante redondante.

## VBST

**VBST** (*VLAN-based Spanning Tree*) est la solution Huawei qui calcule un arbre distinct pour chaque VLAN.

Avantages :

- élimination des boucles ;
- chemins différents selon les VLAN et équilibrage de charge ;
- meilleure utilisation des liens.

Limite : un grand nombre de VLAN implique autant de calculs d'arbres et consomme davantage de ressources sur les switchs.

## MSTP

**MSTP**, défini par IEEE 802.1s, est compatible avec STP et RSTP. Il regroupe plusieurs VLAN dans une **MSTI** (*Multiple Spanning Tree Instance*).

Chaque instance calcule sa propre topologie. Des VLAN ayant le même besoin de chemin peuvent partager une instance, ce qui permet l'équilibrage tout en évitant un arbre par VLAN.

![Comparaison de STP, RSTP, VBST et MSTP](<../../../assets/diagrammes/02 Ethernet Technology Basics/stp-family-comparison-fr.svg>)

| Protocole | Convergence | Relation VLAN–arbre | Point clé |
| --- | --- | --- | --- |
| STP | Lente | Tous les VLAN partagent un arbre | Base IEEE 802.1D |
| RSTP | Rapide | Tous les VLAN partagent un arbre | IEEE 802.1w, rôles et états optimisés |
| VBST | Rapide selon implémentation | Un arbre par VLAN | Solution Huawei, charge de calcul élevée si nombreux VLAN |
| MSTP | Rapide | Plusieurs VLAN par instance | IEEE 802.1s, compromis entre charge et équilibrage |

## Pratique TianSemi : choisir le protocole

Pour 200 VLAN et deux chemins physiques, comparez :

- un arbre unique, simple mais sans équilibrage par VLAN ;
- 200 arbres VBST, très flexibles mais coûteux en ressources ;
- deux MSTI, par exemple VLAN pairs et VLAN impairs, chacune utilisant un chemin préféré différent.

Justifiez le choix en fonction de la convergence, de l'utilisation des liens, de la compatibilité et de la capacité des équipements.

## À retenir pour l'examen HCIA-Datacom

- RSTP est défini par IEEE 802.1w et converge plus vite que STP.
- Le port edge peut atteindre immédiatement Forwarding.
- STP et RSTP partagent un seul arbre pour tous les VLAN.
- VBST calcule un arbre par VLAN.
- MSTP regroupe plusieurs VLAN dans une MSTI.
- Les MSTI indépendantes permettent l'équilibrage avec moins d'arbres que VBST.

## Révision TianSemi

1. Pourquoi STP classique converge-t-il lentement ?
2. Quelles améliorations essentielles RSTP apporte-t-il ?
3. Pourquoi mélanger STP et RSTP est-il déconseillé ?
4. Quelle limite d'utilisation des liens STP et RSTP partagent-ils ?
5. Quel avantage et quel coût VBST introduit-il ?
6. Qu'est-ce qu'une MSTI ?
7. Comment MSTP permet-il l'équilibrage de charge entre VLAN ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 289 à 297.
