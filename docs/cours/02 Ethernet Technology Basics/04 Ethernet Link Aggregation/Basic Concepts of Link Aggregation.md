---
id: HUA-HCIA-DC-023
title: "Basic Concepts of Link Aggregation"
official_title: "Basic Concepts of Link Aggregation"
course_unit: "2.4.1"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-10
updated: 2026-08-10
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 299-308"
tags: [huawei, hcia, datacom, eth-trunk, link-aggregation, lag, redundancy]
prerequisites:
  - "[[Advanced Spanning Tree Protocols]]"
related:
  - "[[Link Aggregation in Manual Mode]]"
  - "[[Link Aggregation in LACP Mode]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Concepts fondamentaux de l'agrégation de liens (Basic Concepts of Link Aggregation)

## Besoin de bande passante et de fiabilité

Ajouter plusieurs liens physiques parallèles entre deux équipements augmente la capacité potentielle et la redondance, mais crée une boucle de couche 2. STP élimine la boucle en bloquant certains liens ; la bande passante supplémentaire reste alors inutilisée.

**Ethernet Link Aggregation**, appelée **Eth-Trunk** chez Huawei, regroupe plusieurs interfaces physiques dans une seule interface logique. Les liens membres peuvent alors contribuer au transfert sans être vus comme des chemins indépendants par STP.

![Plusieurs liens physiques réunis dans une interface logique Eth-Trunk](<../../../assets/diagrammes/02 Ethernet Technology Basics/eth-trunk-physical-to-logical-fr.svg>)

## Bénéfices

- augmentation de la bande passante disponible sans remplacer les interfaces physiques ;
- répartition du trafic entre plusieurs liens actifs ;
- maintien de la communication lorsqu'un lien membre actif tombe en panne ;
- présentation d'une seule interface logique aux fonctions de couche 2 ou de couche 3 supportées.

Huawei cite deux scénarios : entre switchs, et entre un switch et un serveur dont plusieurs NIC sont regroupées.

## Vocabulaire officiel

| Terme | Définition |
| --- | --- |
| LAG | Lien logique constitué de plusieurs liens physiques |
| Eth-Trunk interface | Interface logique représentant le LAG |
| Member interface/link | Interface physique et lien appartenant au groupe |
| Active/selected interface | Membre qui participe au transfert |
| Inactive/unselected interface | Membre conservé hors transfert, éventuellement comme secours |

Une interface Eth-Trunk se configure ensuite comme une interface Ethernet : VLAN, type de lien ou autres services s'appliquent à l'interface logique plutôt qu'indépendamment aux membres.

## Deux modes

- **Manual mode** : le groupe et ses membres sont configurés manuellement, sans négociation LACP.
- **LACP mode** : les équipements échangent des LACPDU afin de vérifier la cohérence du groupe et d'élire les liens actifs.

Les deux extrémités doivent employer le même mode.

## Contraintes de cohérence

Le support insiste sur les contrôles suivants :

- les interfaces homologues doivent appartenir à l'Eth-Trunk correspondant sur le pair ;
- les nombres de liens et paramètres de débit, duplex et contrôle de flux doivent être planifiés de manière cohérente ;
- une interface physique ne peut pas être elle-même un Eth-Trunk membre ;
- certains services configurés sur une interface empêchent son ajout au groupe ;
- en LACP, des interfaces de débits différents peuvent appartenir au groupe, mais elles ne transfèrent pas simultanément par défaut à des débits différents.

Les restrictions exactes dépendent du modèle et de la version logicielle.

## Pratique TianSemi : choisir entre STP et Eth-Trunk

Deux switchs sont reliés par quatre interfaces 1 Gbit/s.

1. Expliquez ce que ferait STP si les quatre liens restaient indépendants.
2. Représentez-les comme membres d'Eth-Trunk 1.
3. Calculez la capacité théorique agrégée en émission, sans supposer qu'un flux unique utilise les quatre liens.
4. Décrivez le comportement attendu si un lien actif tombe en panne.

Un Eth-Trunk répartit des **flux** entre les liens ; il ne garantit pas qu'un flux unique dépasse le débit d'un membre.

## À retenir pour l'examen HCIA-Datacom

- Eth-Trunk agrège plusieurs liens physiques en un lien logique.
- Il améliore bande passante, utilisation et fiabilité.
- STP voit l'interface logique, ce qui évite de bloquer chaque lien membre comme chemin parallèle indépendant.
- Un membre actif transfère ; un membre inactif ne transfère pas.
- Les modes manuel et LACP doivent être identiques aux deux extrémités.

## Révision TianSemi

1. Quel problème apparaît lorsqu'on ajoute simplement plusieurs liens parallèles ?
2. Pourquoi STP seul n'augmente-t-il pas la bande passante entre deux switchs ?
3. Qu'est-ce qu'une interface Eth-Trunk ?
4. Quelle différence existe entre membre actif et membre inactif ?
5. Quels sont les deux modes d'agrégation présentés ?
6. Pourquoi les deux extrémités doivent-elles avoir une configuration cohérente ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 299 à 308.
