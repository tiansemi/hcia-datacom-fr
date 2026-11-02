---
id: HUA-HCIA-DC-019
title: "Basic Concepts of STP"
official_title: "Basic Concepts of STP"
course_unit: "2.3.2"
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
source_pages: "PDF 255-265"
tags: [huawei, hcia, datacom, stp, bridge-id, bpdu, root-path-cost, port-id]
prerequisites:
  - "[[Overview of STP]]"
related:
  - "[[Working Principles of STP]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Concepts fondamentaux de STP (Basic Concepts of STP)

## Bridge ID et pont racine

Chaque switch exécutant STP possède un **Bridge ID (BID)** unique, composé de :

- une priorité de pont sur 16 bits ;
- une adresse MAC de pont sur 48 bits.

La priorité par défaut est `32768`. Elle peut varier de 0 à 65535, mais doit être un multiple de 4096. Le switch dont le BID est le plus petit devient le **root bridge** : priorité la plus faible d'abord, puis adresse MAC la plus faible en cas d'égalité.

Il n'existe qu'un seul root bridge dans une topologie STP continue. Il est le centre logique du calcul, pas nécessairement le centre physique du réseau.

## Cost et Root Path Cost

Chaque port STP possède un **cost**. Par défaut, plus la bande passante est élevée, plus le coût est faible. Huawei utilise par défaut la méthode IEEE 802.1t et prend aussi en charge IEEE 802.1D-1998 et une méthode historique Huawei.

Le **Root Path Cost (RPC)** est la somme des coûts du chemin vers le root bridge. Le RPC du root bridge vaut 0. Un switch non racine préfère le chemin de RPC le plus faible.

Tous les switchs d'un même réseau doivent employer la même méthode de calcul des coûts pour que les comparaisons restent cohérentes.

## Port ID

Le **Port ID (PID)** combine :

- une priorité de port sur les 4 bits de poids fort ;
- un numéro de port sur les 12 bits restants.

Sur les switchs Huawei du cours, la priorité de port par défaut est 128. Elle varie de 0 à 240 par pas de 16. Le PID sert de critère de départage pendant l'élection des rôles de port.

## BPDU

Les **Bridge Protocol Data Units** transportent les informations nécessaires au calcul STP.

| Type | Rôle |
| --- | --- |
| Configuration BPDU | Élection du root bridge et détermination des rôles et états de ports |
| TCN BPDU | Notification d'un changement de topologie vers l'amont |

Une BPDU de configuration contient notamment Root ID, RPC, Bridge ID, Port ID et les temporisateurs.

| Temporisateur | Valeur par défaut du cours | Rôle |
| --- | ---: | --- |
| Hello Time | 2 s | Intervalle d'émission des BPDU par le root bridge |
| Max Age | 20 s | Durée avant invalidation d'une information BPDU non renouvelée |
| Forward Delay | 15 s | Durée passée en Listening puis en Learning |

## Comparaison des BPDU

![Ordre de comparaison des informations STP dans une BPDU](<../../../assets/diagrammes/02 Ethernet Technology Basics/stp-bpdu-comparison-order-fr.svg>)

STP sélectionne la BPDU supérieure en comparant successivement :

1. le plus petit Root ID ;
2. le plus petit RPC ;
3. le plus petit Bridge ID émetteur ;
4. le plus petit Port ID émetteur.

Le premier critère qui diffère décide. Il ne faut pas additionner ou pondérer ces valeurs.

## Rôles de ports introduits dans le cours

- **Root port** : meilleur port d'un switch non racine vers le root bridge ; un seul par switch non racine.
- **Designated port** : port choisi pour envoyer les BPDU sur un segment ; un seul par lien.
- **Alternate port** : port qui n'est ni root ni designated et qui est bloqué pour supprimer la boucle.

Tous les ports du root bridge sont généralement designated. Le root bridge ne possède pas de root port.

## Pratique TianSemi : comparer deux BPDU

Comparez :

```text
BPDU A : Root ID 4096.AAAA, RPC 20000, Bridge ID 8192.BBBB, Port ID 128.1
BPDU B : Root ID 4096.AAAA, RPC 10000, Bridge ID 32768.CCCC, Port ID 128.2
```

La BPDU B est supérieure : les Root ID sont identiques, puis son RPC est plus faible. Les champs Bridge ID et Port ID ne sont plus examinés.

## À retenir pour l'examen HCIA-Datacom

- Le plus petit BID gagne ; priorité avant MAC.
- La priorité de pont par défaut est 32768, par pas de 4096.
- Un débit plus élevé produit normalement un coût plus faible.
- Le RPC est le coût cumulé vers la racine.
- La priorité de port par défaut est 128, par pas de 16.
- Ordre BPDU : Root ID, RPC, Bridge ID, Port ID.

## Révision TianSemi

1. De quels champs se compose un BID ?
2. Comment départage-t-on deux switchs ayant la même priorité de pont ?
3. Quelle relation existe entre bande passante et coût STP ?
4. Qu'est-ce que le Root Path Cost ?
5. De quels champs se compose un PID ?
6. Quels sont les deux types de BPDU présentés ?
7. Dans quel ordre compare-t-on les quatre informations déterminantes d'une BPDU ?
8. Quelles sont les valeurs par défaut de Hello Time, Max Age et Forward Delay ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 255 à 265.
