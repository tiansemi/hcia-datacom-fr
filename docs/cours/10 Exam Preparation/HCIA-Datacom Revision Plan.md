---
id: HUA-HCIA-DC-EXAM-001
title: "HCIA-Datacom Revision Plan"
category: "Huawei HCIA-Datacom Exam Preparation"
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
module: "10 Exam Preparation"
type: "revision-plan"
source: "TianSemi synthesis from HCIA-Datacom V2.0 Training Material and Lab Guide"
source_pages: "Training PDF 1-1117; Lab Guide PDF 14-270"
tags: [huawei, hcia, datacom, revision, exam-preparation]
publication_status: "not-started"
site_url:
---

# Plan de révision HCIA-Datacom V2.0

## Positionnement

Ce plan TianSemi organise la matière officielle en cycles de maîtrise. Il ne décrit ni le format actuel de l’examen ni sa pondération : ces éléments doivent être vérifiés sur Huawei Talent au moment de l’inscription.

![Cycle TianSemi de préparation HCIA-Datacom](<../../assets/diagrammes/10 Exam Preparation/hcia-datacom-revision-cycle-fr.svg>)

## Test initial

Avant de relire les notes, réalisez trois diagnostics à froid :

1. dessiner le trajet d’un paquet entre deux VLAN jusqu’à un réseau externe ;
2. expliquer comment un STA obtient son IP dans une architecture WAC + Fit AP en transfert direct ;
3. choisir les commandes qui prouvent successivement l’état physique, le VLAN, l’IP, la route et le service.

Classez chaque domaine de 0 à 3 :

| Niveau | Critère observable |
| --- | --- |
| 0 — absent | impossible d’expliquer le mécanisme |
| 1 — reconnu | notion reconnue, mais réponse dépendante de la note |
| 2 — applicable | configuration ou diagnostic réalisable avec quelques hésitations |
| 3 — démontré | explication, application et preuve produites sans aide |

## Les six cycles

### Cycle 1 — Fondations et VRP

- Communication, équipements, topologies et modèles de référence.
- Encapsulation/décapsulation et rôle des protocoles.
- Navigation VRP, modes de commande, sauvegarde et vérifications de base.
- Preuve : expliquer une communication client–serveur et lire une configuration simple.

### Cycle 2 — LAN Ethernet

- Trames, MAC, apprentissage, flooding et forwarding.
- VLAN, access/trunk/hybrid, PVID et marquage 802.1Q.
- STP : élection, rôles/états, coûts et convergence.
- Eth-Trunk manuel et LACP.
- Preuve : prévoir le chemin d’une trame et justifier les ports STP actifs ou bloqués.

### Cycle 3 — IPv4, IPv6 et routage

- Sous-réseautage, ICMP et DHCP.
- Routes directes, statiques, défaut, préférence et longest prefix match.
- OSPF : voisinage, LSDB, SPF, areas et vérification.
- IPv6, ICMPv6, NDP, SLAAC et DHCPv6 stateless/stateful.
- Preuve : calculer un plan d’adressage puis diagnostiquer une route manquante.

### Cycle 4 — Sécurité et services

- ACL : types, wildcard, ordre de correspondance et point d’application.
- AAA local et principes d’authentification/autorisation/accounting.
- SSH comme accès distant recommandé ; limites de Telnet et FTP.
- NAT source, destination et bidirectionnel.
- Preuve : prédire l’effet d’une règle et confirmer la décision par compteur/session.

### Cycle 5 — Data center, WLAN et exploitation

- Évolution et architectures générales des DCN.
- 802.11, BSS/ESS, canaux, accès radio et architectures WAC/Fit AP.
- Profils WLAN, CAPWAP, management/service VLAN et modes de transfert.
- SNMP, NETCONF, RESTCONF, iMaster NCE et automatisation Paramiko.
- Preuve : reconstruire la chaîne AP → WAC → service et choisir une interface d’automatisation adaptée.

### Cycle 6 — Dépannage et campus intégré

- Méthode : symptôme, périmètre, hypothèse, preuve, correction minimale, retest.
- Révision des quatre pannes du lab 16.
- Exécution ou lecture active du lab 17 de bout en bout.
- Preuve : produire une recette couvrant DHCP, OSPF, WLAN, NAT, SSH et sauvegarde.

## Format d’une séance

| Bloc | Action | Livrable |
| --- | --- | --- |
| Rappel | restituer sans note | schéma ou réponse courte |
| Compréhension | relire uniquement les lacunes | explication reformulée |
| Pratique | lab ou analyse de sortie | capture/sortie annotée |
| Contrôle | questions et flashcards | erreurs classées |
| Bouclage | planifier la prochaine révision | date + objectif précis |

Privilégiez le rappel actif. Relire une note sans tenter d’abord de restituer donne une impression trompeuse de maîtrise.

## Critère de passage entre cycles

Un cycle est consolidé lorsque vous pouvez :

- expliquer chaque mécanisme central sans reprendre les formulations de la note ;
- identifier les dépendances avec le cycle précédent ;
- choisir une commande de vérification et interpréter sa sortie ;
- résoudre au moins un scénario inhabituel, pas seulement reproduire le lab ;
- obtenir le niveau 3 sur les points critiques du tracker.

## Révision TianSemi

1. Pourquoi le rappel actif précède-t-il la relecture dans une séance ?
2. Quelle preuve distingue le niveau 2 du niveau 3 ?
3. Pourquoi Ethernet doit-il être consolidé avant le WLAN ?
4. Comment utiliser le lab 16 pour réviser plusieurs domaines simultanément ?
5. Quel cycle mérite le plus de temps selon votre test initial ?
6. Quelle sortie de commande pourriez-vous annoter aujourd’hui pour prouver une maîtrise ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1 à 1117.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 14 à 270.
- [[HCIA-Datacom V2.0 Learning Path]].
