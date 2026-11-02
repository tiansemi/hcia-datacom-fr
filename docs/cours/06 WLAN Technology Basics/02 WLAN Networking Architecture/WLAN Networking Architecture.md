---
id: HUA-HCIA-DC-067
title: "WLAN Networking Architecture"
official_title: "WLAN Networking Architecture"
course_unit: "6.2.2"
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
source: "HCIA-Datacom V2.0 Training Material; HCIA-Datacom V2.0 Lab Guide"
source_pages: "Training PDF 845-865; Lab Guide PDF 172-186 (document pages 170-184)"
tags: [huawei, hcia, datacom, wlan, wac, fit-ap, fat-ap, leader-ap, capwap, vlan]
prerequisites:
  - "[[WLAN Networking Overview]]"
related:
  - "[[WLAN Networking Application Scenarios]]"
  - "[[WLAN Service Configuration Procedure]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Architecture de mise en réseau WLAN (WLAN Networking Architecture)

## Trois architectures, trois niveaux de centralisation

Huawei distingue les architectures **WAC + Fit AP**, **Fat AP** et **Leader AP**. Le bon choix dépend surtout du nombre d’AP, du besoin de gestion centralisée, de la mobilité attendue et des compétences d’exploitation.

![Comparaison des architectures WAC + Fit AP, Fat AP et Leader AP](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-architectures-comparison-fr.svg>)

| Architecture | Contrôle | Atouts | Limites principales | Échelle typique |
| --- | --- | --- | --- | --- |
| WAC + Fit AP | WAC central | Gestion, sécurité, mise à niveau et extension centralisées | WAC requis ; conception et exploitation plus exigeantes | Entreprises et campus moyens ou grands |
| Fat AP | Chaque AP | Déploiement autonome, simple et économique | Configuration AP par AP, fonctions et évolutivité limitées | Domicile, mini-boutique, très petit site |
| Leader AP | Un Fat AP pilote des Fit AP | Centralisation légère, roaming, pas de WAC dédié | Capacité inférieure à celle d’un WAC | Micro et petites entreprises |

## WAC + Fit AP

### Répartition des rôles

Le **Wireless Access Controller** assure notamment :

- le contrôle d’accès WLAN ;
- la gestion et la configuration des AP ;
- la gestion de l’itinérance ;
- les contrôles de sécurité ;
- éventuellement la centralisation du trafic utilisateur.

Les **Fit AP** conservent les fonctions radio indispensables : émission et réception 802.11, chiffrement et déchiffrement des trames, fonctions de couche physique et collecte de statistiques sur l’interface air. Ils sont configurés et administrés par le WAC.

Cette séparation facilite le déploiement de nombreux AP, l’ajustement coordonné des canaux et puissances, les mises à niveau groupées et l’application homogène des politiques.

### CAPWAP : canal de contrôle entre WAC et AP

**CAPWAP** (*Control And Provisioning of Wireless Access Points*) est un protocole de couche application reposant sur UDP. Il permet au WAC de maintenir la connectivité avec les AP, de leur livrer les configurations et, selon le mode de transfert, de transporter les données utilisateur.

| Tunnel | Port UDP | Fonction |
| --- | ---: | --- |
| Contrôle | 5246 | Gestion, état, configuration et contrôle des AP. |
| Données | 5247 | Transport des données utilisateur lorsque le transfert par tunnel est utilisé. |

Les tunnels peuvent utiliser DTLS pour protéger les échanges.

### WAC en ligne ou hors chemin

- **In-path** : le WAC se trouve sur le chemin de transmission et peut aussi jouer un rôle d’agrégation. Les paquets de gestion et les données des AP passent par lui. La topologie est simple, mais le WAC peut devenir un goulot d’étranglement.
- **Off-path** : le WAC est raccordé latéralement au réseau existant. Il traite la gestion des AP ; les données peuvent être envoyées directement vers le réseau supérieur ou être centralisées selon le mode de transfert. Cette option s’intègre mieux aux réseaux étendus ou existants.

### Réseau de couche 2 ou de couche 3

- En **Layer 2**, WAC et Fit AP appartiennent au même domaine de broadcast. Les AP peuvent découvrir le WAC par diffusion. La configuration est simple, mais l’échelle reste limitée.
- En **Layer 3**, WAC et AP sont dans des sous-réseaux différents. Le routage doit assurer leur joignabilité et les AP doivent obtenir l’adresse du WAC, par exemple au moyen de DHCP. Cette architecture est plus flexible et recommandée pour les campus moyens ou grands.

Les choix in-path/off-path et Layer 2/Layer 3 sont indépendants : Huawei présente donc quatre combinaisons possibles.

## Planification des VLAN

Huawei distingue :

- le **VLAN de gestion**, qui transporte les échanges CAPWAP, y compris les données encapsulées si le transfert par tunnel est retenu ;
- le **VLAN de service**, qui transporte les données du service WLAN.

Il est recommandé de séparer VLAN de gestion et VLAN de service. La relation SSID–VLAN dépend du besoin : `1:1`, `1:N`, `N:1` ou `N:M`. Par exemple, un même SSID peut être associé à des VLAN différents selon la zone lorsque des politiques de transfert distinctes sont nécessaires.

## Fat AP

Un Fat AP fonctionne de manière autonome : il accepte les stations, chiffre les données et les transfère sans contrôleur dédié. Chaque AP doit cependant être configuré séparément.

Deux modes courants existent :

- **bridge** : l’AP relie le WLAN au réseau filaire ; un routeur indépendant sert de passerelle de service ;
- **gateway** : le Fat AP sert lui-même de passerelle WLAN et peut fournir l’accès Internet avec NAT.

Le mode bridge convient lorsque clients filaires et sans fil partagent le même VLAN et la même passerelle. Le mode gateway convient lorsque les domaines doivent être séparés ou lorsque l’AP constitue la sortie Internet.

## Leader AP

Dans cette architecture, un AP fonctionne en mode Fat et devient le **leader AP** ; les autres fonctionnent en mode Fit. Le leader utilise CAPWAP pour les gérer et les configurer de manière centralisée. Il permet aussi le roaming entre AP d’un même ESS.

Les AP sont généralement dans le même réseau de couche 2. Leurs adresses peuvent provenir d’un serveur DHCP externe, du serveur DHCP intégré au leader, ou être configurées statiquement.

Deux modes sont possibles :

- **bridge**, recommandé par Huawei : une passerelle indépendante fournit notamment DHCP ; le transfert direct limite la charge du leader ;
- **gateway** : le leader fournit passerelle, NAT et DHCP. Le trafic des Fit AP lui revient par tunnel, ce qui simplifie le réseau mais peut limiter la capacité et créer un goulot d’étranglement.

## Vérifications Huawei utiles

Ces commandes sont pertinentes dans une architecture WAC + Fit AP et sont utilisées dans le lab officiel :

```text
<WAC> display ap all
<WAC> display vap all
<WAC> display station all
<WAC> display ip pool interface Vlanif101 used
```

Elles permettent respectivement de vérifier l’état des AP, les VAP de service, les stations associées et l’utilisation du pool d’adresses. Sur le switch d’accès, `display lldp neighbor interface ...` aide à identifier l’AP et son adresse MAC avant son admission sur le WAC.

## Pratique TianSemi : choisir puis valider une architecture

Une entreprise possède 18 AP répartis sur trois bâtiments. Le réseau filaire existe déjà, le WAC sera installé dans le cœur, les AP sont placés dans plusieurs sous-réseaux et le trafic utilisateur doit sortir localement.

1. Justifiez le choix WAC + Fit AP plutôt que Fat AP ou Leader AP.
2. Choisissez in-path ou off-path.
3. Choisissez Layer 2 ou Layer 3 entre WAC et AP.
4. Séparez le VLAN de gestion et les VLAN de service.
5. Décrivez comment les AP découvrent le WAC.
6. Indiquez les résultats attendus de `display ap all`, `display vap all` et `display station all`.

## Troubleshooting guidé

| Symptôme | Contrôle prioritaire | Explication possible |
| --- | --- | --- |
| Un Fit AP ne passe pas en ligne | Joignabilité WAC–AP, source CAPWAP, adresse du WAC, admission de l’AP | Le tunnel de contrôle ne s’établit pas. |
| L’AP est en ligne mais aucun SSID n’est visible | État du VAP, profil lié au groupe AP, radio concernée | Le service WLAN n’est pas appliqué à l’AP. |
| La station s’associe mais n’obtient pas d’adresse | VLAN de service, DHCP et chemin de transfert | Le plan de service n’atteint pas le serveur DHCP. |
| Le WAC est saturé | Mode in-path ou transfert par tunnel, volume utilisateur | Le trafic centralisé traverse le contrôleur. |
| Le leader AP ralentit fortement | Mode gateway, trafic tunnelé, nombre de Fit AP | Le leader cumule contrôle et transfert. |

## À retenir pour l’examen HCIA-Datacom

- WAC + Fit AP centralise la gestion et convient aux réseaux moyens ou grands.
- CAPWAP utilise UDP 5246 pour le contrôle et UDP 5247 pour les données tunnelées.
- In-path place le WAC sur le chemin ; off-path l’insère latéralement.
- Layer 2 simplifie la découverte ; Layer 3 améliore l’évolutivité mais exige la joignabilité IP et la découverte du WAC.
- Les VLAN de gestion et de service doivent de préférence être distincts.
- Un Fat AP est autonome ; un leader AP centralise un petit groupe de Fit AP sans WAC dédié.
- Le mode bridge du leader AP avec transfert direct réduit sa charge et est recommandé par Huawei.

## Révision TianSemi

1. Comment WAC et Fit AP se répartissent-ils les fonctions ?
2. Quels rôles remplissent les tunnels CAPWAP de contrôle et de données ?
3. Quelle différence existe entre un WAC in-path et off-path ?
4. Pourquoi une architecture Layer 3 est-elle plus extensible qu’une architecture Layer 2 ?
5. Pourquoi séparer VLAN de gestion et VLAN de service ?
6. Dans quels cas choisir un Fat AP en mode gateway ?
7. Comment un leader AP gère-t-il les Fit AP ?
8. Pourquoi le mode bridge est-il recommandé pour le leader AP ?
9. Quelles commandes permettent de vérifier AP, VAP et stations sur un WAC ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 845 à 865.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 172 à 186, correspondant aux pages 170 à 184 du document, lab « WAC + Fit AP Networking Configuration ».

