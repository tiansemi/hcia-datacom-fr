---
id: HUA-HCIA-DC-061
title: "Dual-Track Architecture of DCs"
official_title: "Dual-Track Architecture of DCs"
course_unit: "5.1.5"
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
source_pages: "PDF 777-780"
tags: [huawei, hcia, datacom, dual-track, general-purpose-computing, intelligent-computing, dcn]
prerequisites:
  - "[[Intelligent Computing DCN]]"
related:
  - "[[WLAN Technology Basics]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Architecture à deux voies des data centers (Dual-Track Architecture of DCs)

## Partager le site, séparer les réseaux

Les futurs data centers font coexister calcul général et calcul intelligent. Ils peuvent partager bâtiment, alimentation, refroidissement et orchestration, mais leurs plans réseau restent séparés.

![L'architecture dual-track partage l'infrastructure tout en isolant les deux réseaux](<../../../assets/diagrammes/05 Data Center Network Basics/dc-dual-track-fr.svg>)

Cette organisation vise une **coexistence physique avec isolation logique** :

- mutualiser les ressources et réduire les coûts de construction et d'exploitation ;
- conserver des frontières de performance claires ;
- éviter qu'une charge d'un domaine perturbe l'autre ;
- orchestrer les ressources au niveau global.

## Deux réseaux complémentaires

| Voie | Charges | Propriétés réseau dominantes |
| --- | --- | --- |
| Calcul général | Web, bases de données, cloud et applications diverses | Flexibilité, isolation, disponibilité, automatisation. |
| Calcul intelligent | IA, HPC et calcul parallèle | Très haut débit, absence de perte et très faible latence. |

Le réseau général ressemble à une artère assurant la continuité des services. Le réseau intelligent ressemble à une piste dédiée permettant d'exploiter au maximum la puissance de calcul. Aucun ne remplace l'autre.

## Décision d'architecture

Une conception correcte commence par le profil réel des charges :

1. identifier applications, volumes et motifs de trafic ;
2. classer les ressources de calcul général et intelligent ;
3. dimensionner chaque fabric selon ses objectifs ;
4. définir les limites de faute et de performance ;
5. mutualiser seulement les composants qui ne compromettent pas ces limites ;
6. mettre en place une orchestration et une supervision unifiées.

## Pratique TianSemi : répartir les services

Classez les éléments suivants entre voie générale, voie intelligente et infrastructure partagée : portail web, base clients, cluster d'entraînement GPU, stockage HPC, alimentation, refroidissement et plateforme d'orchestration. Pour chaque choix, justifiez l'exigence dominante et le risque d'une fabric unique.

## À retenir pour l'examen HCIA-Datacom

- L'architecture dual-track fait coexister calcul général et calcul intelligent.
- L'infrastructure physique peut être partagée, tandis que les plans réseau restent séparés.
- L'isolation préserve les frontières de performance.
- L'orchestration unifiée réduit les coûts et améliore l'utilisation des ressources.
- Les deux voies sont complémentaires, pas substituables.
- Le calcul général utilise notamment spine-leaf, M-LAG et VXLAN ; l'intelligent ajoute PFC, ECN et RDMA.

## Révision TianSemi

1. Que signifie « coexistence physique avec isolation logique » ?
2. Pourquoi les deux plans réseau restent-ils séparés ?
3. Quelles infrastructures peuvent être mutualisées ?
4. Quelles exigences distinguent les deux voies ?
5. Quel avantage apporte l'orchestration unifiée ?
6. Pourquoi une voie ne peut-elle pas simplement remplacer l'autre ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 777 à 780.
