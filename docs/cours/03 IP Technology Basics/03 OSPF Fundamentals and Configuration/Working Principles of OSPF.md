---
id: HUA-HCIA-DC-037
title: "Working Principles of OSPF"
official_title: "Working Principles of OSPF"
course_unit: "3.3.2"
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
source_pages: "PDF 469-483"
tags: [huawei, hcia, datacom, ospf-adjacency, neighbor-state, dr, bdr, ospf-area]
prerequisites:
  - "[[Overview of OSPF]]"
related:
  - "[[Typical OSPF Configuration]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Principes de fonctionnement d'OSPF (Working Principles of OSPF)

## Voisinage et adjacence

Deux routeurs deviennent **voisins** après s'être découverts avec des Hello bidirectionnels. Ils deviennent **adjacents** lorsque leurs LSDB sont synchronisées. Une relation de voisinage ne conduit pas toujours à une adjacence, notamment entre deux DROther sur un réseau multi-accès.

## États et échanges

![États successifs de l'adjacence OSPF et paquets échangés](<../../../assets/diagrammes/03 IP Technology Basics/ospf-adjacency-states-fr.svg>)

| État | Ce qui se produit |
| --- | --- |
| Down | Aucun Hello valide reçu |
| Init | Un Hello du voisin est reçu, sans son propre Router ID dans le champ Neighbor |
| 2-Way | Les Hello prouvent une découverte bidirectionnelle |
| ExStart | Négociation maître/esclave et numéro de séquence DD |
| Exchange | Échange des résumés de LSDB par paquets DD |
| Loading | Demande et réception des LSA manquantes par LSR/LSU/LSAck |
| Full | LSDB synchronisées, adjacence établie |

Pendant ExStart, le routeur possédant le Router ID le plus grand devient maître pour l'échange DD. Ce rôle d'échange n'est pas le rôle DR.

```text
<R1> display ospf peer
<R1> display ospf peer brief
```

Sur un lien point à point, `Full` est l'état stable attendu. Sur un réseau broadcast, `2-Way` est normal entre deux DROther, tandis que les adjacences avec DR et BDR atteignent `Full`.

## Types de réseau OSPF

| Type | Environnement typique | Broadcast | Élection DR/BDR |
| --- | --- | --- | --- |
| P2P | Liaison PPP entre deux équipements | — | Non |
| Broadcast | Ethernet multi-accès | Oui | Oui |
| NBMA | Réseau multi-accès sans broadcast, par exemple Frame Relay | Non | Oui |
| P2MP | Ensemble de liaisons point à point | Selon conception | Non dans le modèle présenté |

Les deux extrémités d'un lien doivent généralement utiliser un type compatible. Une incohérence peut empêcher l'établissement du voisinage.

```text
[R1-GE0/0/0] ospf network-type p2p
```

## DR, BDR et DROther

![Rôles DR, BDR et DROther sur un réseau Ethernet multi-accès](<../../../assets/diagrammes/03 IP Technology Basics/ospf-dr-bdr-fr.svg>)

Sur un réseau multi-accès, des adjacences entre chaque paire produiraient trop de relations et de flooding. OSPF élit donc :

- un **DR**, point central des adjacences ;
- un **BDR**, déjà synchronisé et prêt à remplacer le DR ;
- des **DROther**, adjacents au DR et au BDR mais seulement voisins entre eux.

L'élection compare d'abord la priorité d'interface, puis le Router ID en cas d'égalité. Une priorité 0 exclut l'interface. L'élection est non préemptive : augmenter la priorité après l'élection ne remplace pas automatiquement le DR actif.

## Aire unique et multi-aire

![D'une aire unique à une architecture OSPF multi-aire](<../../../assets/diagrammes/03 IP Technology Basics/ospf-single-multi-area-fr.svg>)

Dans une aire unique, tous les routeurs synchronisent la LSDB de l'aire. À grande échelle, cela augmente la LSDB, le calcul SPF et le volume de flooding.

Le multi-aire :

- limite la portée du flooding et des changements de topologie ;
- autorise la sommation aux frontières d'aire ;
- impose l'aire 0 comme backbone ;
- exige que les aires non backbone se raccordent au backbone, pas directement entre elles.

## Types de routeurs OSPF

| Type | Définition |
| --- | --- |
| IR | Toutes les interfaces OSPF appartiennent à la même aire |
| ABR | Interfaces dans au moins deux aires, dont l'aire 0 |
| BR | Au moins une interface dans l'aire 0 |
| ASBR | Importe des routes externes dans le domaine OSPF |

Un routeur peut cumuler plusieurs rôles.

## Troubleshooting du voisinage

| État ou symptôme | Piste prioritaire |
| --- | --- |
| Aucun voisin | OSPF activé sur l'interface, connectivité IP et Hello |
| Bloqué en Init | Communication unidirectionnelle ou Hello retour absent |
| Bloqué en ExStart/Exchange | MTU, cohérence du lien ou échange DD |
| 2-Way entre deux DROther | État normal sur réseau multi-accès |
| Voisins jamais formés | Type de réseau, aire, paramètres Hello/Dead ou authentification |
| DR inattendu | Priorité, Router ID et ordre de démarrage ; élection non préemptive |

## À retenir pour l'examen HCIA-Datacom

- 2-Way confirme le voisinage bidirectionnel ; Full confirme la synchronisation.
- L'ordre est Down, Init, 2-Way, ExStart, Exchange, Loading, Full.
- DD résume, LSR demande, LSU transporte, LSAck confirme.
- Ethernet utilise par défaut le type broadcast ; PPP le type P2P.
- DR et BDR réduisent les adjacences sur un réseau multi-accès.
- Priorité 0 interdit l'élection ; l'élection est non préemptive.
- Toutes les aires non backbone doivent être reliées à l'aire 0.

## Révision TianSemi

1. Quelle différence existe entre voisin OSPF et adjacent OSPF ?
2. Que prouvent les états 2-Way et Full ?
3. Quels paquets interviennent entre Exchange et Full ?
4. Quels sont les quatre types de réseau OSPF ?
5. Pourquoi DR et BDR sont-ils nécessaires sur Ethernet multi-accès ?
6. Comment l'élection départage-t-elle deux candidats ?
7. Pourquoi une priorité modifiée ne remplace-t-elle pas immédiatement le DR ?
8. Quel rôle joue l'aire 0 dans un domaine multi-aire ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 469 à 483.
