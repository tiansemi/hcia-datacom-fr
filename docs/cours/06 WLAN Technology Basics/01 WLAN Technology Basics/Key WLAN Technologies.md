---
id: HUA-HCIA-DC-064
title: "Key WLAN Technologies"
official_title: "Key WLAN Technologies"
course_unit: "6.1.3"
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
source_pages: "PDF 805-819"
tags: [huawei, hcia, datacom, wlan, qam, ofdm, channel-bonding, mimo, csma-ca]
prerequisites:
  - "[[Basic WLAN Concepts]]"
related:
  - "[[Introduction to 802.11 Standards]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Technologies clés du WLAN (Key WLAN Technologies)

## Les leviers du débit radio

![Les quatre leviers de capacité WLAN et le mécanisme d'accès au canal](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-performance-levers-fr.svg>)

Huawei regroupe les progrès du débit autour de quatre leviers :

1. une modulation transportant davantage de bits par symbole ;
2. davantage de sous-porteuses orthogonales ;
3. une largeur de canal plus grande ;
4. plusieurs flux spatiaux grâce à MIMO.

Ces gains restent soumis à la qualité radio et au partage du canal par CSMA/CA.

## QAM : amplitude et phase

La **Quadrature Amplitude Modulation** utilise simultanément amplitude et phase. Chaque point de constellation représente un symbole : sa distance à l'origine décrit l'amplitude, et son angle la phase.

Une modulation comportant `2ⁿ` symboles transporte `n` bits par symbole :

| Modulation | Symboles | Bits par symbole |
| --- | ---: | ---: |
| 64-QAM | 64 | 6 |
| 1024-QAM | 1024 | 10 |
| 4096-QAM | 4096 | 12 |

Une modulation plus dense augmente le débit potentiel, mais exige une meilleure qualité de signal pour distinguer des points plus proches.

## OFDM : plusieurs sous-porteuses

**Orthogonal Frequency Division Multiplexing** divise un flux rapide en flux parallèles plus lents, transmis sur des sous-porteuses différentes. Leur orthogonalité permet de les superposer spectralement : au pic d'une sous-porteuse, les autres ont une amplitude nulle.

OFDM améliore l'utilisation du canal sans faire interférer les sous-porteuses correctement alignées.

## Agrégation de canaux

Des canaux voisins non chevauchants peuvent être regroupés en 40, 80, 160 ou 320 MHz selon la bande et la norme. Le canal primaire porte notamment les trames de gestion ; les autres sont secondaires.

Une largeur supérieure fournit davantage de sous-porteuses et augmente le débit, mais consomme davantage de spectre. Elle peut réduire les possibilités de réutilisation dans un environnement dense.

## MIMO et flux spatiaux

| Système | Antennes | Effet principal |
| --- | --- | --- |
| SISO | 1 émission, 1 réception | Un chemin et un flux. |
| SIMO | 1 émission, plusieurs réceptions | Diversité de réception. |
| MISO | Plusieurs émissions, 1 réception | Diversité d'émission. |
| MIMO | Plusieurs émissions et réceptions | Plusieurs flux spatiaux simultanés. |

Le multiplexage spatial découpe les données en flux indépendants transmis par plusieurs antennes. Un système `M×N` ne peut généralement utiliser plus de flux que le plus petit nombre entre M et N. Ainsi, 3×2 MIMO fournit au maximum deux flux.

## Pourquoi CSMA/CA

Un émetteur radio ne peut pas détecter fiablement une collision pendant sa propre transmission. Il peut aussi subir :

- le **nœud caché**, lorsque deux stations ne s'entendent pas mais émettent vers le même récepteur ;
- le **nœud exposé**, lorsqu'une station attend alors que sa transmission n'aurait pas perturbé le récepteur voisin.

CSMA/CA cherche donc à éviter les collisions :

1. écouter le canal ;
2. attendre l'Interframe Space requis ;
3. effectuer un backoff aléatoire si nécessaire ;
4. transmettre ;
5. attendre l'ACK ;
6. retransmettre en l'absence d'ACK.

## Pratique TianSemi

Pour un AP 4×4 utilisant 80 MHz et 1024-QAM :

1. donnez le nombre maximal de flux spatiaux avec un client 2×2 ;
2. expliquez pourquoi passer à 160 MHz ne double pas toujours le débit réel ;
3. indiquez l'effet d'un signal faible sur 1024-QAM ;
4. décrivez la réaction CSMA/CA lorsque le canal est occupé.

## À retenir pour l'examen HCIA-Datacom

- QAM combine amplitude et phase ; 64-QAM transporte 6 bits par symbole.
- OFDM répartit les données sur des sous-porteuses orthogonales.
- L'agrégation augmente la largeur du canal et le nombre de sous-porteuses.
- MIMO augmente capacité ou fiabilité grâce aux antennes et flux spatiaux.
- Le nombre de flux est limité par la plus petite dimension du système MIMO.
- CSMA/CA écoute, attend, recule aléatoirement et exige un ACK.
- Le WLAN évite les collisions car leur détection fiable est impraticable en radio.

## Révision TianSemi

1. Quels quatre leviers augmentent le débit WLAN ?
2. Combien de bits un symbole 4096-QAM transporte-t-il ?
3. Pourquoi les sous-porteuses OFDM peuvent-elles se chevaucher ?
4. Quel est le rôle du canal primaire dans une agrégation ?
5. Combien de flux un système 3×2 MIMO peut-il transporter au maximum ?
6. Quelle différence existe entre nœud caché et nœud exposé ?
7. Pourquoi CSMA/CA attend-il un ACK après une trame de données ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 805 à 819.
