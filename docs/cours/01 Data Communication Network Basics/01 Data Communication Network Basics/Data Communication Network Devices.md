---
id: HUA-HCIA-DC-002
title: "Data Communication Network Devices"
official_title: "Data Communication Network Devices"
course_unit: "1.1.2"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-06
updated: 2026-07-13
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 18-30"
tags: [huawei, hcia, datacom, network-devices]
prerequisites:
  - "[[Data Communication Network]]"
related:
  - "[[01 Data Communication Network Basics/Data Communication Process]]"
  - "[[03 Network Device Operating Systems/Operating System Overview]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Équipements des réseaux de communication de données (Data Communication Network Devices)

## Objectif du cours

Cette unité permet de distinguer les principaux équipements d'un réseau de communication de données, de comprendre leurs fonctions fondamentales et d'identifier la structure des équipements fixes et modulaires.

## Équipements dans un réseau de campus d'entreprise

Le schéma Huawei d'un campus de bureau associe plusieurs zones et rôles :

- **salle d'équipements du campus** : routeur, firewall, switch cœur et WAC ;
- **data center** : switchs et serveurs fournissant les applications et le stockage ;
- **étages de bureaux** : switchs d'accès pour les terminaux filaires et les AP ;
- **zones utilisateurs** : réception, espace ouvert, salle de réunion et terminaux sans fil.

Les équipements d'infrastructure reçoivent et transfèrent les données en collaboration. Les terminaux utilisent cette infrastructure pour accéder aux services.

## Switch

Un switch transfère efficacement des trames dans un LAN en examinant l'adresse MAC de destination.

Selon le scénario, Huawei distingue notamment :

- les switchs de campus, utilisés aux couches d'accès ou de cœur ;
- les switchs de data center, reliant les ressources de calcul et de stockage avec des exigences élevées de performance et de fiabilité.

Selon leur construction, les switchs peuvent être :

- **fixes** : composants de service intégrés dans un châssis, nombre de ports généralement déterminé ;
- **modulaires** : fonctions réparties sur différentes cartes, forte capacité et haute disponibilité.

Dans le schéma du cours, les switchs fixes servent à l'accès et le switch modulaire agrège le trafic au cœur du campus.

## Routeur

Un routeur transfère des paquets entre réseaux. Il examine l'adresse IP de destination, sélectionne un chemin approprié et transmet le paquet au routeur suivant ou à la destination.

Fonctions citées par Huawei :

- maintenance de la table de routage et des informations de route ;
- découverte des routes et sélection de chemin ;
- transfert de paquets ;
- accès WAN ;
- traduction d'adresses réseau ;
- certaines fonctions de sécurité.

Selon le scénario, Huawei distingue les routeurs backbone, métropolitains et d'accès. Un routeur d'accès peut servir de sortie d'un campus vers un WAN ou Internet.

## Firewall

Un firewall protège les communications entre réseaux. Il surveille, limite et peut modifier les flux qui le traversent afin de protéger les informations, la structure et l'état du réseau interne.

Il est généralement placé à la sortie du réseau pour séparer les zones internes et externes. Ses fonctions peuvent inclure :

- isolation de réseaux ayant des niveaux de sécurité différents ;
- contrôle d'accès ;
- authentification des utilisateurs ;
- accès distant ;
- chiffrement et VPN ;
- traduction d'adresses réseau.

## Équipements WLAN

Un WLAN utilise des signaux sans fil pour permettre l'accès et la transmission de données. Le Wi-Fi correspond aux technologies WLAN fondées sur la famille IEEE 802.11.

### Access Point

Huawei présente trois modes principaux :

| Mode | Gestion | Scénario indiqué |
| --- | --- | --- |
| Fat AP | fonctionne et se configure indépendamment | domicile |
| Fit AP | administré de manière centralisée par un WAC | moyenne ou grande entreprise |
| Cloud AP | administré par une plateforme cloud | petite ou moyenne entreprise |

### Wireless Access Controller

Le WAC est généralement placé à la couche d'agrégation ou de cœur. Il gère et configure les Fit AP de façon centralisée et contribue à fournir des services WLAN performants, sûrs et fiables.

## Schéma physique et topologie logique

Huawei distingue :

- le **schéma de connexion physique**, qui représente les équipements, ports et câbles réels ;
- la **topologie logique simplifiée**, qui met l'accent sur les rôles et les relations de communication.

Les équipements modernes peuvent combiner plusieurs fonctions. Un switch peut assurer des fonctions de commutation et de routage ; un routeur peut intégrer du routage, de la sécurité ou du sans-fil. Le choix d'un équipement doit donc partir des besoins de service plutôt que de son seul nom.

## Composition des équipements réseau

Huawei utilise les exemples CloudEngine S12700H-8 et CloudEngine S5755-H24T4Y2CZ pour présenter deux architectures.

### Équipement modulaire

Les fonctions sont réparties sur des cartes :

| Module | Fonction décrite par Huawei |
| --- | --- |
| MPU | contrôle, gestion et supervision du système |
| SFU | contrôle, supervision et échange de données |
| LPU | traitement des paquets et gestion du trafic |
| SPU | traitement de services à valeur ajoutée |
| CMU | supervision de l'environnement et des seuils de température |

Les LPU communiquent par l'intermédiaire des SFU. Une architecture modulaire est généralement utilisée au cœur du réseau et vise une forte fiabilité.

### Équipement fixe

Dans un équipement fixe, les modules de service sont intégrés au châssis. Le support cite notamment le châssis, les alimentations, les ventilateurs et la Switch Control Unit. Ces équipements sont courants à l'accès ou dans les petits réseaux d'entreprise.

Le S5755-H présenté dans le cours possède notamment des ports d'accès, des ports haut débit, un port console, un port USB et un port de management. Le bouton PNP peut provoquer une réinitialisation ou un retour aux paramètres usine ; Huawei signale explicitement le risque d'interruption de service.

## Résumé fidèle du cours

- Le switch transfère des trames dans le LAN à partir des adresses MAC.
- Le routeur transfère des paquets entre réseaux à partir des adresses IP et de sa table de routage.
- Le firewall contrôle les flux entre zones de sécurité.
- Les AP et WAC fournissent et administrent l'accès WLAN.
- Les équipements peuvent être fixes ou modulaires selon leur architecture.
- Le choix d'un équipement dépend du rôle, du scénario et des exigences de service.

## Révision TianSemi

1. Quelle différence fondamentale existe entre le transfert réalisé par un switch et celui réalisé par un routeur ?
2. Quel est le rôle du WAC dans une architecture Fit AP ?
3. Pourquoi un switch modulaire est-il généralement placé au cœur du réseau ?
4. Quelles cartes assurent respectivement le contrôle, la commutation et le traitement des paquets dans l'exemple modulaire ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 18-30.
- Huawei Talent, HCIA-Datacom V2.0 : https://e.huawei.com/en/talent/#/cert/product-details?certifiedProductId=1316&authenticationLevel=CTYPE_CARE_HCIA&technicalField=IIC&version=2.0
