---
id: HUA-HCIA-DC-062
title: "WLAN Overview"
official_title: "WLAN Overview"
course_unit: "6.1.1"
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
source_pages: "PDF 782-790"
tags: [huawei, hcia, datacom, wlan, wifi, ieee-802-11, radio]
prerequisites:
  - "[[Dual-Track Architecture of DCs]]"
related:
  - "[[Basic WLAN Concepts]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble du WLAN (WLAN Overview)

## Pourquoi un réseau local sans fil

Un LAN câblé offre des performances stables, mais impose des prises, du câblage et une position fixe. Le WLAN répond principalement à trois besoins :

- mobilité des utilisateurs ;
- déploiement dans des lieux difficiles ou coûteux à câbler ;
- connexion d'un grand nombre d'objets intelligents.

## Définition

Un **Wireless Local Area Network** est un réseau local qui utilise des technologies radio au lieu de câbles pour relier les terminaux et transporter les données. Sa couverture s'étend typiquement d'une pièce à un bâtiment ou un campus.

Les cours suivants emploient WLAN pour un réseau fondé sur **Wi-Fi et IEEE 802.11**.

![Relation entre WLAN, Wi-Fi et IEEE 802.11](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-wifi-80211-fr.svg>)

- **WLAN** désigne la catégorie de réseau local sans fil ;
- **IEEE 802.11** définit les spécifications MAC et PHY utilisées par Wi-Fi ;
- **Wi-Fi** est la technologie WLAN la plus répandue et une marque de la Wi-Fi Alliance.

Tout Wi-Fi est un WLAN, mais tout WLAN n'est pas nécessairement du Wi-Fi.

## Bandes utilisées

Les WLAN Wi-Fi utilisent principalement les bandes sans licence :

- 2,4 GHz ;
- 5 GHz ;
- 6 GHz, introduite dans le parcours des générations récentes et soumise aux réglementations nationales.

## Scénarios

| Environnement | Exemples |
| --- | --- |
| Domicile | Téléphones, ordinateurs, télévision, jeux et objets connectés. |
| Entreprise ou école | Bureau mobile, visioconférence, salles de cours, bibliothèques et dortoirs. |
| Lieu public | Café, aéroport, métro ou hôtel. |

## Repères d'évolution

| Année | Étape du support |
| --- | --- |
| 1971 | ALOHAnet, réseau radio à commutation de paquets. |
| 1997 | Première norme IEEE 802.11. |
| 1999 | Création de WECA et publication de 802.11a/b. |
| 2003 | 802.11g. |
| 2009 | 802.11n, 2,4/5 GHz et débit théorique jusqu'à 600 Mbit/s. |
| 2013 | 802.11ac et passage au gigabit. |
| 2019 | Wi-Fi 6. |
| 2024 | Wi-Fi 7, haut débit et faible latence. |

La Wi-Fi Alliance certifie l'interopérabilité des produits et gère la marque Wi-Fi.

## Pratique TianSemi : qualifier un besoin

Pour un entrepôt, listez les terminaux, la mobilité, les obstacles, les applications et les zones difficiles à câbler. Indiquez ensuite ce que le WLAN résout et ce qu'il faudra encore étudier : couverture radio, capacité, interférences, sécurité et raccordement filaire des points d'accès.

## À retenir pour l'examen HCIA-Datacom

- Le WLAN utilise les ondes radio et apporte mobilité et souplesse de déploiement.
- IEEE 802.11 définit les couches MAC et PHY du Wi-Fi.
- Wi-Fi est une marque de la Wi-Fi Alliance.
- Tout Wi-Fi est WLAN, mais l'inverse n'est pas toujours vrai.
- Les principales bandes sont 2,4, 5 et 6 GHz.
- ALOHAnet précède la première norme IEEE 802.11 de 1997.

## Révision TianSemi

1. Quelle limite principale du LAN câblé le WLAN corrige-t-il ?
2. Quelle différence existe entre WLAN, Wi-Fi et IEEE 802.11 ?
3. Quel rôle joue la Wi-Fi Alliance ?
4. Quelles bandes de fréquences le support associe-t-il au WLAN ?
5. Quels scénarios quotidiens utilisent un WLAN ?
6. Pourquoi un WLAN ne supprime-t-il pas tout besoin de réseau filaire ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 782 à 790.
