---
id: HUA-HCIA-DC-OVERVIEW-001
title: "HCIA-Datacom V2.0 Learning Path"
official_title: "HCIA-Datacom V2.0 Training Content"
category: "Huawei HCIA-Datacom Program Overview"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-06
updated: 2026-08-11
module: "00 Program Overview"
type: "learning-path"
source: "HCIA-Datacom V2.0 Training Material and Lab Guide"
source_pages: "Training PDF 1-1117; Lab Guide PDF 14-270"
tags: [huawei, hcia, datacom, learning-path, study-plan]
publication_status: "not-started"
site_url:
---

# Parcours d’apprentissage HCIA-Datacom V2.0

## Finalité du parcours

HCIA-Datacom V2.0 construit une compréhension progressive des réseaux de données : partir de la communication et des équipements, comprendre Ethernet et IP, ajouter les services et la sécurité, puis exploiter et intégrer ces technologies dans un réseau de campus.

Cette note reprend la hiérarchie officielle du Training Material. Elle sert de carte de navigation TSOS ; elle ne remplace ni les unités détaillées ni les labs Huawei.

![Progression des huit domaines HCIA-Datacom vers le campus intégré](<../../assets/diagrammes/00 Program Overview/hcia-datacom-learning-path-fr.svg>)

## Les huit domaines officiels

| Ordre | Domaine | Pages Training PDF | Résultat attendu |
| --- | --- | --- | --- |
| 1 | [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/01 Data Communication Network Basics/Home\|Data Communication Network Basics]] | 1–151 | expliquer une communication, ses équipements, ses modèles et l’usage de VRP |
| 2 | [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/02 Ethernet Technology Basics/Home\|Ethernet Technology Basics]] | 153–333 | construire un LAN commuté avec VLAN, STP et agrégation de liens |
| 3 | [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/03 IP Technology Basics/Home\|IP Technology Basics]] | 335–560 | adresser, sous-réseauter et router en IPv4/IPv6 avec DHCP et OSPF |
| 4 | [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/04 Network Security and Services/Home\|Network Security and Services]] | 562–736 | appliquer ACL, AAA, services d’administration et NAT |
| 5 | [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/05 Data Center Network Basics/Home\|Data Center Network Basics]] | 738–780 | distinguer les évolutions et architectures générales des DCN |
| 6 | [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/06 WLAN Technology Basics/Home\|WLAN Technology Basics]] | 782–931 | comprendre 802.11, les architectures WLAN et déployer un service Fit AP |
| 7 | [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/07 Network O&M and Troubleshooting/Home\|Network O&M and Troubleshooting]] | 937–1078 | superviser, automatiser et dépanner avec une méthode fondée sur les preuves |
| 8 | [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/08 Typical Campus Networking Solution/Home\|Typical Campus Networking Solution]] | 1080–1117 | assembler les technologies dans un projet de campus cohérent |

Les numéros de pages sont ceux du PDF TianSemi de référence. Quelques pages de transition séparent les unités.

## Logique de progression

### 1. Comprendre avant de configurer

Le premier domaine établit le vocabulaire : information, données, protocoles, encapsulation, topologies, équipements et système d’exploitation réseau. Sans cette base, une commande reste difficile à interpréter ou à dépanner.

### 2. Construire le transport local

Ethernet introduit la trame, l’apprentissage MAC et la commutation. Les VLAN créent des domaines logiques, STP évite les boucles et Eth-Trunk apporte capacité et résilience.

### 3. Relier les réseaux

IPv4/IPv6, ICMP, DHCP, routes directes et statiques, puis OSPF permettent d’aller au-delà du LAN. Le sous-réseautage est une dépendance forte pour le routage, les ACL et le dépannage.

### 4. Contrôler et fournir des services

ACL et AAA traitent respectivement le filtrage et le contrôle d’accès. SSH fournit l’administration distante protégée ; NAT traduit les adresses aux frontières. FTP et Telnet sont étudiés pour comprendre leur fonctionnement et leurs limites, pas comme choix de production recommandés.

### 5. Élargir l’architecture

Les notions de data center et de WLAN montrent deux environnements spécifiques. Le WLAN réutilise directement VLAN, DHCP, routage et sécurité : il ne doit donc pas être étudié isolément.

### 6. Exploiter puis intégrer

SNMP, NETCONF, RESTCONF, Paramiko et la méthode de troubleshooting préparent l’exploitation. Le dernier domaine et le lab de campus valident la capacité à assembler le tout dans un même système.

## Articulation avec les 17 labs

| Étape pratique | Labs officiels |
| --- | --- |
| Prise en main | 01 — opérations de base |
| LAN Ethernet | 02 — VLAN, 03 — STP, 04 — agrégation |
| Routage et adressage | 05 — routes directes, 06 — statiques, 07 — autoconfiguration, 08 — OSPF, 09 — IPv6 |
| Sécurité et services | 10 — ACL, 11 — fichiers, 12 — accès distant, 13 — NAT |
| WLAN et automatisation | 14 — WAC + Fit AP, 15 — Paramiko optionnel |
| Intégration | 16 — dépannage, 17 — déploiement de campus |

Accès : [[01 Domains/Huawei/01 Datacom/HCIA-Datacom V2.0/09 Labs/Home|09 Labs]].

## Méthode TianSemi recommandée

Pour chaque unité :

1. lire l’explication française en gardant le schéma de référence sous les yeux ;
2. reformuler les mécanismes sans consulter la note ;
3. exécuter ou analyser la pratique associée ;
4. produire une preuve : sortie `display`, table, capture ou résultat de test ;
5. traiter les questions de révision et transformer les erreurs en points faibles ;
6. revenir à l’unité après un délai, sans simplement la relire immédiatement.

Le Training Material compte 84 unités TSOS et le Lab Guide 17 activités. Le lab 15 est indiqué comme optionnel dans le guide ; il reste utile pour relier SSH, Python et automatisation.

## Jalons de maîtrise

- **Fondations acquises** : expliquer l’encapsulation et lire une configuration VRP simple.
- **LAN opérationnel** : prédire le comportement MAC/VLAN/STP et vérifier un Eth-Trunk.
- **IP opérationnel** : calculer un sous-réseau, lire une table de routage et diagnostiquer DHCP/OSPF.
- **Services maîtrisés** : expliquer l’ordre ACL, le modèle AAA, le rôle de SSH et les effets du NAT.
- **Campus intégré** : suivre le chemin d’un paquet filaire ou WLAN de bout en bout et localiser une rupture.
- **Exploitation maîtrisée** : choisir les bonnes preuves avant de modifier une configuration.

## Révision TianSemi

1. Pourquoi le WLAN dépend-il des acquis Ethernet et IP ?
2. Quelles notions faut-il maîtriser avant d’étudier OSPF ?
3. Quelle différence existe entre apprendre une commande et démontrer un résultat opérationnel ?
4. Pourquoi le dépannage se situe-t-il après les modules technologiques ?
5. Quels labs valident le passage d’un LAN commuté à un réseau routé ?
6. Quel jalon vous paraît aujourd’hui le moins maîtrisé, et quelle preuve permettrait de le mesurer ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, unités officielles des pages PDF 1 à 1117.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, labs 01 à 17, pages PDF 14 à 270.
- Huawei Talent, plan de contenu HCIA-Datacom V2.0 fourni avec le projet TianSemi.
