---
id: HUA-HCIA-DC-083
title: "Basic Concepts of Campus Networks"
official_title: "Basic Concepts of Campus Networks"
course_unit: "8.1.1"
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
source_pages: "PDF 1080-1090"
tags: [huawei, hcia, datacom, campus, lan, architecture, access, aggregation, core]
prerequisites:
  - "[[Common Network Troubleshooting Cases]]"
related:
  - "[[Practices in Campus Network Projects]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Concepts fondamentaux des réseaux de campus

## Ce que Huawei appelle un réseau de campus

Un **réseau de campus** est un réseau local qui relie des personnes et des objets dans une zone géographique délimitée. Il est généralement construit et administré par une seule entité : entreprise, établissement scolaire, administration, usine, banque ou centre commercial.

Cette définition repose sur trois idées :

- le périmètre reste compatible avec des technologies LAN ;
- la responsabilité d’administration est clairement définie ;
- le réseau fournit un accès aux ressources internes et, selon les politiques établies, aux services externes.

Si plusieurs entités administrent séparément différentes parties d’un même site, Huawei considère chaque partie gérée comme un campus distinct. La taille peut aller d’un environnement SOHO à un ensemble de bâtiments couvrant plusieurs kilomètres carrés. Au-delà du périmètre adapté aux technologies LAN, l’interconnexion relève plutôt d’un MAN ou d’un WAN.

Les technologies d’accès typiques sont :

- Ethernet, conforme à la famille IEEE 802.3, pour les accès filaires ;
- Wi-Fi, conforme à la famille IEEE 802.11, pour les accès sans fil.

## Une architecture hiérarchique et modulaire

Huawei recommande de concevoir le campus à la fois par **couches** et par **zones fonctionnelles**.

![Architecture hiérarchique et zones d’un réseau de campus](<../../../assets/diagrammes/08 Typical Campus Networking Solution/campus-network-architecture-fr.svg>)

### Les couches du réseau

| Couche | Fonction principale | Question de conception |
| --- | --- | --- |
| Accès | Connecter terminaux, téléphones, imprimantes et points d’accès | Comment les utilisateurs entrent-ils dans le réseau ? |
| Agrégation | Regrouper les flux et porter certaines fonctions de routage ou de sécurité | Où consolider les accès et appliquer des politiques communes ? |
| Cœur | Assurer la commutation à haut débit entre les différentes parties du campus | Comment relier les zones avec une infrastructure stable et performante ? |

La hiérarchie clarifie la fonction de chaque équipement. Elle facilite la localisation d’un défaut, permet d’étendre une couche sans reconstruire tout le réseau et rend les chemins de trafic plus prévisibles : accès, agrégation, cœur, puis destination.

### Les zones fonctionnelles

| Zone | Rôle |
| --- | --- |
| Sortie du campus | Frontière avec Internet ou le WAN ; contrôle des échanges internes et externes. |
| Centre de données | Héberge serveurs et applications destinés aux utilisateurs internes ou externes. |
| Gestion du réseau | Regroupe les plateformes de supervision et de contrôle, par exemple contrôleur SDN, WAC et serveur de journaux. |

Le découpage modulaire isole les fonctions. Une politique de sécurité, une ACL ou une politique de routage peut être appliquée à une frontière clairement identifiée. Un problème affectant une zone a ainsi moins de chances de se propager au campus entier, et chaque module peut évoluer à son propre rythme.

## Choisir une architecture adaptée à l’échelle

Huawei classe les campus selon leur nombre d’utilisateurs et d’éléments réseau. Ces valeurs servent de repères de conception, pas de seuils universels indépendants du contexte.

| Taille | Repères Huawei | Architecture et contexte typiques |
| --- | --- | --- |
| Petit campus | Moins de 200 utilisateurs et moins de 25 éléments réseau | Un seul site, exigences simples, parfois sans hiérarchie explicite. Exemple : chaîne de cafés. |
| Campus moyen | 200 à 2 000 utilisateurs et 25 à 100 éléments réseau | Conception par fonctions, souvent en trois couches : accès, agrégation et cœur. Exemple : entreprise de commerce international. |
| Grand campus | Plus de 2 000 utilisateurs et plus de 100 éléments réseau | Plusieurs bâtiments ou campus, modules complets, WAN et accès distant possibles. Exemple : grande entreprise. |

La taille seule ne suffit pas. La couverture, la criticité des services, la sécurité, le nombre de sites et les perspectives de croissance influencent aussi le choix.

## Technologies réparties dans l’architecture

Les protocoles ne sont pas déployés partout de manière identique. Leur emplacement dépend de la fonction de la couche ou de la zone.

- À l’accès : VLAN, agrégation de liens et mécanismes de prévention des boucles.
- À l’agrégation : agrégation de liens, spanning tree, DHCP, routage statique ou OSPF selon l’architecture.
- Au cœur : OSPF, routage statique, ACL et AAA peuvent assurer interconnexion et contrôle.
- À la sortie : NAT, OSPF ou routes statiques relient le campus aux réseaux externes.
- Dans la gestion : SSH, SNMP ou NETCONF permettent administration et supervision.
- Pour le WLAN : AP, WAC et CAPWAP assurent accès radio et contrôle centralisé selon le modèle retenu.

Cette répartition n’est pas une recette automatique. Elle montre que chaque technologie répond à une fonction précise du campus.

## Lire un campus comme un ingénieur

Face à une topologie, procédez dans cet ordre :

1. délimiter le campus et identifier l’entité qui l’administre ;
2. repérer les terminaux et leur couche d’accès ;
3. suivre les flux vers l’agrégation puis le cœur ;
4. distinguer sortie, centre de données et gestion ;
5. relever les frontières où doivent s’appliquer routage, sécurité et supervision ;
6. vérifier que l’architecture correspond à l’échelle et aux besoins réels.

## Pratique TianSemi : cartographier un campus

Choisissez un petit site réel ou imaginaire : école, bureau, boutique ou agence.

1. Dessinez le périmètre administré et les connexions externes.
2. Listez les utilisateurs, terminaux, serveurs et services attendus.
3. Placez les équipements dans les couches accès, agrégation et cœur ; si une couche n’est pas nécessaire, justifiez son absence.
4. Identifiez les zones sortie, centre de données et gestion.
5. Associez à chaque frontière les technologies utiles : VLAN, STP, OSPF, NAT, ACL, SSH ou SNMP.
6. Simulez la panne d’un équipement et expliquez comment la structure choisie aide à localiser l’impact.

Le livrable est une topologie annotée accompagnée d’une courte justification de l’architecture.

## À retenir pour l’examen HCIA-Datacom

- Un campus est un LAN limité géographiquement et généralement géré par une seule entité.
- Ethernet et Wi-Fi sont les technologies d’accès LAN typiques.
- La conception hiérarchique clarifie les rôles, simplifie le dépannage et facilite l’évolution.
- Les couches principales sont l’accès, l’agrégation et le cœur.
- Les zones typiques comprennent la sortie, le centre de données et la gestion du réseau.
- Petit, moyen et grand campus se distinguent notamment par le nombre d’utilisateurs, d’éléments réseau et la complexité de l’architecture.
- Les protocoles sont placés selon la fonction de la couche ou de la zone, pas ajoutés uniformément.

## Révision TianSemi

1. Quels critères permettent de définir un réseau de campus selon Huawei ?
2. Pourquoi plusieurs domaines d’administration peuvent-ils correspondre à plusieurs campus sur un même site ?
3. Quels bénéfices apporte une conception hiérarchique ?
4. Quelle différence existe entre une couche réseau et une zone fonctionnelle ?
5. Quel est le rôle respectif des couches accès, agrégation et cœur ?
6. Quelles fonctions assurent les zones de sortie, de centre de données et de gestion ?
7. Quels repères Huawei distinguent petit, moyen et grand campus ?
8. Pourquoi NAT, STP, OSPF et SNMP ne sont-ils pas nécessairement déployés au même endroit ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1080 à 1090.
