---
id: HUA-HCIA-DC-068
title: "WLAN Networking Application Scenarios"
official_title: "WLAN Networking Application Scenarios"
course_unit: "6.2.3"
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
source_pages: "PDF 866-872"
tags: [huawei, hcia, datacom, wlan, design, fat-ap, leader-ap, wac, enterprise, campus]
prerequisites:
  - "[[WLAN Networking Architecture]]"
related:
  - "[[WLAN Fundamentals]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Scénarios d’application des architectures WLAN (WLAN Networking Application Scenarios)

## Partir du besoin, pas du nom de l’architecture

Le choix d’une architecture WLAN dépend de la taille du site, du nombre de terminaux, de la couverture, du roaming, de la coexistence filaire/sans fil et de l’organisation entre siège et agences.

![Choix d’une architecture WLAN selon le scénario](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-scenario-decision-fr.svg>)

## Très petit site : un Fat AP

Dans une petite boutique ou une micro-entreprise de faible superficie :

- un Fat AP peut servir de passerelle et de sortie Internet ;
- le nombre de stations simultanées reste faible ;
- seul l’accès sans fil est requis ;
- aucun roaming n’est nécessaire ;
- les besoins d’accès à Internet restent simples.

Ce choix évite un contrôleur et limite le coût, mais il n’est pas destiné à une croissance importante.

## Petite entreprise : Leader AP + Fit AP

Lorsque plusieurs AP sont nécessaires, l’architecture Leader AP étend la couverture et centralise leur configuration. Un switch peut parallèlement raccorder des terminaux filaires, comme des caméras ou imprimantes.

Ce scénario convient à certaines boutiques expérientielles, plateformes logistiques et petites entreprises. Il apporte le roaming et une administration unifiée sans WAC dédié. Si les exigences d’accès Internet sont particulières, Huawei recommande généralement une passerelle indépendante.

## Petite ou moyenne entreprise distribuée : WAC central au siège

Le siège et les agences sont reliés par un WAN et des protocoles de routage. Le WAC et le serveur d’authentification sont placés au siège ; le WAC gère les Fit AP du siège et des agences.

- Au siège, le transfert peut être direct ou tunnelé selon le besoin.
- Dans les agences, le transfert local est généralement privilégié.
- Les utilisateurs des agences reçoivent leurs adresses depuis les passerelles locales et accèdent à Internet par la sortie de leur agence.

Ce modèle centralise la politique tout en évitant de faire remonter inutilement tout le trafic utilisateur vers le siège.

## Grande ou moyenne entreprise : WAC par zone

Pour un environnement plus vaste, Huawei présente un WAC au siège et un WAC dans les agences. Chaque contrôleur gère les Fit AP de sa zone. Le serveur d’authentification reste généralement centralisé au siège.

Cette répartition réduit la dépendance au WAN pour le contrôle local et adapte la capacité à la taille de chaque site.

## Matrice de sélection

| Besoin dominant | Architecture cohérente | Point de vigilance |
| --- | --- | --- |
| Un seul petit espace, peu de stations, sans roaming | Fat AP | Administration manuelle et faible évolutivité. |
| Plusieurs AP sur un petit site, roaming simple | Leader AP + Fit AP | Capacité du leader et mode de transfert. |
| Plusieurs bâtiments ou sites, politique centralisée | WAC + Fit AP | Joignabilité CAPWAP et dimensionnement du WAC. |
| Agences avec accès Internet local | WAC central + transfert local en agence | Continuité du contrôle via le WAN. |
| Très grande organisation avec besoins locaux forts | WAC par zone | Cohérence des politiques et authentification centralisée. |

## Méthode de décision TianSemi

Posez les questions dans cet ordre :

1. Combien de zones et d’AP faut-il couvrir ?
2. Combien de stations seront simultanément connectées ?
3. Le roaming est-il nécessaire ?
4. Une administration centralisée est-elle requise ?
5. Le réseau doit-il aussi raccorder des terminaux filaires ?
6. Le trafic utilisateur doit-il sortir localement ou traverser un contrôleur ?
7. Existe-t-il un WAN entre siège et agences ?
8. Quelle dépendance au WAN est acceptable pour le contrôle et l’authentification ?

## Pratique TianSemi : trois études de cas

Pour chaque cas, choisissez une architecture et justifiez-la par au moins trois critères.

### Cas A — Café indépendant

Une salle, 25 clients au maximum, un seul accès Internet, aucun besoin de roaming.

### Cas B — Petite société

Deux étages, six AP, postes filaires, imprimantes et caméras, mobilité des employés entre les étages.

### Cas C — Entreprise multisite

Un siège, huit agences, politiques de sécurité communes, accès Internet local dans les agences et serveur d’authentification au siège.

Ajoutez ensuite, pour chaque choix, le principal risque de saturation ou de dépendance et la mesure qui le réduit.

## Pièges de conception

| Décision fragile | Conséquence probable | Ajustement |
| --- | --- | --- |
| Multiplier les Fat AP sur un site en croissance | Configurations incohérentes et absence de pilotage global | Passer à Leader AP ou WAC + Fit AP. |
| Utiliser un leader AP comme passerelle sous forte charge | Goulot d’étranglement | Mode bridge, passerelle indépendante et transfert direct. |
| Tunneliser tout le trafic des agences vers le siège | Dépendance et consommation du WAN | Évaluer le transfert local. |
| Déployer un seul WAC sans étudier la capacité et les pannes | Point de concentration critique | Dimensionner et répartir le contrôle selon l’échelle. |

## À retenir pour l’examen HCIA-Datacom

- Un Fat AP suffit pour un très petit site, peu de stations et aucun roaming.
- Leader AP convient à un petit réseau multi-AP demandant centralisation légère et roaming.
- WAC + Fit AP répond aux déploiements d’entreprise moyens ou grands.
- Dans les agences, le transfert local permet une sortie Internet directe par la passerelle locale.
- Une grande organisation peut déployer des WAC distincts au siège et dans les agences.
- Le choix se fonde sur l’échelle, la mobilité, la centralisation et le chemin du trafic.

## Révision TianSemi

1. Pourquoi un Fat AP est-il adapté à une petite boutique ?
2. Quel besoin supplémentaire justifie le passage à Leader AP ?
3. Quels équipements peuvent rester raccordés au switch dans une petite entreprise ?
4. Pourquoi les agences utilisent-elles souvent le transfert local ?
5. Où le serveur d’authentification est-il généralement placé dans les scénarios multisites Huawei ?
6. Quand déployer un WAC dans chaque zone plutôt qu’un unique WAC central ?
7. Quels critères faut-il recueillir avant de choisir l’architecture WLAN ?
8. Quel risque apparaît si un leader AP en mode gateway traite un trafic important ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 866 à 872.

