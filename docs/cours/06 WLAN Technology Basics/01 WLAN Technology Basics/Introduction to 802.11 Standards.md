---
id: HUA-HCIA-DC-065
title: "Introduction to 802.11 Standards"
official_title: "Introduction to 802.11 Standards"
course_unit: "6.1.4"
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
source_pages: "PDF 820-833"
tags: [huawei, hcia, datacom, ieee-802-11, wifi-4, wifi-5, wifi-6, wifi-7, ofdma, mu-mimo, mlo]
prerequisites:
  - "[[Key WLAN Technologies]]"
related:
  - "[[WLAN Networking Overview]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Introduction aux normes 802.11 (Introduction to 802.11 Standards)

## Deux familles de spécifications

IEEE 802.11 définit :

- des spécifications **PHY** : bandes, modulation, codage et débit maximal ;
- des compléments **MAC** : QoS, sécurité, roaming, mesh ou contrôle de puissance.

Cette note se concentre sur l'évolution PHY et les technologies de capacité présentées dans le cours.

## Générations Wi-Fi

![Évolution des générations Wi-Fi et de leurs principaux leviers](<../../../assets/diagrammes/06 WLAN Technology Basics/wifi-generations-fr.svg>)

| Génération | Norme | Bandes | Modulation maximale | Largeur maximale | Débit théorique maximal du support |
| --- | --- | --- | --- | --- | ---: |
| — | 802.11b | 2,4 GHz | — | 22 MHz | 11 Mbit/s |
| — | 802.11a/g | 5 GHz / 2,4 GHz | jusqu'à 64-QAM pour g | 20 MHz | 54 Mbit/s |
| Wi-Fi 4 | 802.11n | 2,4 ou 5 GHz | 64-QAM | 40 MHz | 600 Mbit/s |
| Wi-Fi 5 | 802.11ac Wave 2 | 5 GHz | 256-QAM | 160 ou 80+80 MHz | 6,9 Gbit/s |
| Wi-Fi 6 | 802.11ax | 2,4 ou 5 GHz | 1024-QAM | 160 ou 80+80 MHz | 9,6 Gbit/s |
| Wi-Fi 7 | 802.11be | 2,4, 5 ou 6 GHz | 4096-QAM | 320 MHz | 23,05 Gbit/s |

Ces valeurs sont des maxima théoriques du standard, pas le débit d'un terminal réel.

## De quoi dépend le débit théorique

Le support relie le débit à :

- nombre de flux spatiaux ;
- bits codés par sous-porteuse ;
- taux de codage ;
- nombre de sous-porteuses utiles ;
- durée du symbole et Guard Interval.

Le débit **atteignable par un produit** peut être inférieur si l'équipement implémente moins de flux ou de largeur. Le débit **réel d'accès** est encore réduit par distance, obstacles, interférences, contention et clients concurrents.

## Wi-Fi 6 : OFDMA

Avec OFDM classique, un utilisateur occupe toutes les sous-porteuses pendant son créneau, même pour un petit paquet. **OFDMA** divise le canal en Resource Units et les attribue à plusieurs utilisateurs dans le même intervalle.

OFDMA améliore l'efficacité et réduit la latence, surtout pour de petits paquets et des besoins de faible bande passante.

## SU-MIMO et MU-MIMO

- **SU-MIMO** utilise plusieurs flux pour un seul client ;
- **MU-MIMO** distribue des flux indépendants entre plusieurs clients utilisant simultanément la largeur de canal.

802.11ac introduit MU-MIMO en downlink ; 802.11ax étend les capacités et introduit l'uplink MU-MIMO. OFDMA partage les sous-porteuses, alors que MU-MIMO partage les flux spatiaux : les deux mécanismes ne doivent pas être confondus.

## Wi-Fi 7 : MRU et MLO

**Multi-RU** permet d'attribuer plusieurs RU, contiguës ou non, à un même utilisateur. Cela améliore son débit et l'utilisation des fragments de spectre.

**Multi-Link Operation** établit plusieurs liens simultanés sur différentes bandes :

- répartition de charge pour augmenter le débit ;
- transmission multiple et réception sélective pour augmenter la fiabilité.

Wi-Fi 7 combine aussi 4096-QAM, la bande 6 GHz et des canaux jusqu'à 320 MHz.

## Pratique TianSemi : ne pas confondre les débits

Pour un AP annoncé « Wi-Fi 7 23 Gbit/s » et un client 2×2 limité à 160 MHz :

1. identifiez les paramètres non implémentés par le client ;
2. distinguez débit théorique du standard, débit atteignable du produit et débit réel ;
3. listez quatre causes de réduction du débit réel ;
4. choisissez OFDMA ou MU-MIMO pour de nombreux capteurs à petits paquets, puis justifiez.

## À retenir pour l'examen HCIA-Datacom

- 802.11 décrit MAC et PHY ; les générations commerciales regroupent certaines normes PHY.
- Wi-Fi 4 correspond à 802.11n, Wi-Fi 5 à 802.11ac, Wi-Fi 6 à 802.11ax et Wi-Fi 7 à 802.11be.
- 802.11be prend en charge 6 GHz, 4096-QAM et 320 MHz.
- Le débit réel reste inférieur au débit atteignable, lui-même inférieur ou égal au maximum théorique.
- OFDMA partage le canal en RU ; MU-MIMO partage les flux spatiaux.
- MRU attribue plusieurs RU à un utilisateur ; MLO utilise plusieurs liens.

## Révision TianSemi

1. Quelle différence existe entre spécifications PHY et MAC 802.11 ?
2. Quelles normes correspondent aux générations Wi-Fi 4 à 7 ?
3. Quels paramètres entrent dans le calcul du débit théorique ?
4. Pourquoi un produit n'atteint-il pas nécessairement le maximum de la norme ?
5. Quelle différence fondamentale existe entre OFDMA et MU-MIMO ?
6. Qu'apporte MRU par rapport à OFDMA Wi-Fi 6 ?
7. Quels deux objectifs MLO peut-il poursuivre ?
8. Quelle norme du tableau prend en charge la bande 6 GHz ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 820 à 833.
