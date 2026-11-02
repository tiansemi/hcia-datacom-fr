---
id: HUA-HCIA-DC-066
title: "WLAN Networking Overview"
official_title: "WLAN Networking Overview"
course_unit: "6.2.1"
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
source_pages: "PDF 839-844"
tags: [huawei, hcia, datacom, wlan, ssid, vap, bss, bssid, ess, essid]
prerequisites:
  - "[[Introduction to 802.11 Standards]]"
related:
  - "[[WLAN Networking Architecture]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d’ensemble de la mise en réseau WLAN (WLAN Networking Overview)

## Ce que voit l’utilisateur et ce que distingue le réseau

Lorsqu’un téléphone ou un ordinateur recherche un réseau Wi-Fi, il affiche des noms de WLAN. Ces noms sont des **SSID**. Le SSID permet à l’utilisateur de reconnaître un service sans exposer toute la structure technique qui le fournit.

![Relations entre SSID, VAP, BSS, BSSID et ESS](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-identifiers-hierarchy-fr.svg>)

Un seul point d’accès peut diffuser plusieurs services. Pour y parvenir, il héberge plusieurs points d’accès virtuels, ou **VAP**. Chaque VAP forme un **BSS** avec les stations qui lui sont associées et possède un **BSSID** propre.

## Les identifiants fondamentaux

### SSID : le nom du WLAN

Le **Service Set Identifier** remplit quatre fonctions principales :

- identifier un WLAN dans la liste affichée par une station ;
- séparer logiquement plusieurs services sur les mêmes équipements radio ;
- désigner le WLAN auquel la station demande à s’associer ;
- rattacher un service à un ensemble cohérent de paramètres réseau et de sécurité.

Un SSID est donc un **nom de service**, pas l’adresse physique d’un point d’accès.

### VAP : un point d’accès logique

Un **Virtual Access Point** est une entité logique créée sur un AP physique. Chaque VAP peut porter un SSID et des paramètres distincts. Un même AP peut ainsi proposer, par exemple, un WLAN `Employes` et un WLAN `Invites` sans multiplier les équipements ni réserver un canal radio par service.

### BSS et BSSID : la cellule radio et son identifiant

Le **Basic Service Set** est l’unité de base d’un WLAN : un VAP et les stations qui lui sont associées. Sa zone utile correspond à la couverture radio de ce VAP.

Le **Basic Service Set Identifier** identifie ce BSS au niveau liaison. Il correspond à l’adresse MAC du VAP et doit être unique. Sur un même AP :

- chaque VAP forme un BSS distinct ;
- chaque BSS possède un BSSID distinct ;
- plusieurs BSS peuvent néanmoins afficher le même SSID.

Le SSID est comparable au **nom** d’un service ; le BSSID est son **identité technique unique**.

### ESS et ESSID : étendre la couverture

La couverture d’un seul BSS est limitée. Pour couvrir une zone plus vaste, plusieurs BSS peuvent diffuser le même SSID. Leur ensemble forme un **Extended Service Set**.

Le nom commun utilisé par les BSS de cet ESS est l’**ESSID**. Dans l’usage courant, le terme SSID désigne souvent cet ESSID. Une station peut alors se déplacer d’une cellule à l’autre tout en percevant un même WLAN.

## Relation à mémoriser

| Élément | Nature | Unicité | Rôle |
| --- | --- | --- | --- |
| SSID | Nom logique | Peut être répété | Présente le WLAN à l’utilisateur. |
| VAP | Entité logique sur un AP | Une instance par service radio | Porte les paramètres d’un WLAN. |
| BSS | Cellule de service | Un par VAP | Regroupe un VAP et ses stations. |
| BSSID | Adresse MAC du VAP | Unique | Identifie techniquement le BSS. |
| ESS | Ensemble de BSS | Regroupement logique | Étend un même service sur plusieurs cellules. |
| ESSID | Nom commun de l’ESS | Partagé par ses BSS | Donne l’impression d’un WLAN continu. |

## Comment une station rejoint-elle le WLAN ?

À ce stade du cours, le processus peut être lu ainsi :

1. la station recherche les WLAN disponibles ;
2. elle sélectionne un SSID ;
3. elle s’authentifie, par exemple au moyen d’une clé prépartagée ;
4. elle s’associe au BSS fourni par un VAP précis ;
5. l’architecture WLAN prend ensuite en charge la sécurité, la gestion et l’acheminement des données.

Le seul SSID ne suffit donc pas à garantir un réseau sûr, stable ou facile à administrer. Il faut aussi choisir une architecture adaptée.

## Pratique TianSemi : lire un déploiement multi-SSID

Un AP diffuse `Employes` et `Invites`. Un second AP diffuse les mêmes noms dans une autre zone.

1. Représentez les quatre VAP.
2. Attribuez un BSSID distinct à chacun.
3. Regroupez les deux BSS `Employes` dans un ESS et les deux BSS `Invites` dans un autre.
4. Expliquez pourquoi les deux SSID peuvent être identiques d’un AP à l’autre alors que les BSSID doivent rester différents.
5. Indiquez ce que la station voit et ce que l’infrastructure doit distinguer.

## Erreurs de compréhension fréquentes

| Confusion | Correction |
| --- | --- |
| « SSID et BSSID sont synonymes. » | Le SSID est un nom ; le BSSID est l’adresse MAC unique d’un VAP. |
| « Un AP ne fournit qu’un WLAN. » | Plusieurs VAP permettent plusieurs SSID sur un même AP. |
| « Un ESS est la couverture d’un seul AP. » | Un ESS regroupe plusieurs BSS partageant le même SSID. |
| « Deux AP d’un même ESS ont le même BSSID. » | Ils partagent le SSID, mais chaque VAP conserve son propre BSSID. |

## À retenir pour l’examen HCIA-Datacom

- Le SSID identifie un WLAN pour l’utilisateur et rattache le service à sa configuration.
- Un AP peut héberger plusieurs VAP et donc plusieurs SSID.
- Un VAP et ses stations forment un BSS ; le BSSID est l’adresse MAC du VAP.
- Un ESS réunit plusieurs BSS partageant le même SSID afin d’étendre la couverture.
- SSID et BSSID ne doivent pas être confondus : un nom peut se répéter, une identité technique reste unique.
- La sélection du SSID et l’authentification ne décrivent qu’une partie de l’accès ; l’architecture WLAN reste déterminante.

## Révision TianSemi

1. Quelles sont les quatre fonctions principales d’un SSID ?
2. Pourquoi crée-t-on plusieurs VAP sur un même AP ?
3. De quels éléments un BSS est-il constitué ?
4. Pourquoi le BSSID doit-il être unique ?
5. Quelle différence existe entre SSID et BSSID ?
6. Comment un ESS étend-il la couverture d’un WLAN ?
7. Pourquoi plusieurs BSS d’un ESS partagent-ils un SSID mais pas un BSSID ?
8. Pourquoi la seule configuration d’un SSID ne garantit-elle pas un WLAN sûr et stable ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 839 à 844.

