---
id: HUA-HCIA-DC-077
title: "Introduction to Network Programming and Automation"
official_title: "Introduction to Network Programming and Automation"
course_unit: "7.2.1"
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
source_pages: "PDF 984-987"
tags: [huawei, hcia, datacom, automation, programming, python, ansible, operations]
prerequisites:
  - "[[Introduction to iMaster NCE]]"
related:
  - "[[Overview of Programming Languages and Introduction to Python]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Introduction à la programmation et à l’automatisation réseau

## Pourquoi automatiser

L’exploitation traditionnelle oblige souvent l’ingénieur à se connecter manuellement à chaque équipement, exécuter des commandes et vérifier le résultat. Cette méthode devient lente, coûteuse et difficile à auditer lorsque les équipements et les opérations se multiplient.

![Décider et sécuriser une automatisation réseau](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/network-automation-lifecycle-fr.svg>)

Huawei cite trois scénarios révélateurs :

- mettre à niveau des milliers d’équipements ;
- auditer régulièrement la conformité, par exemple STelnet et Spanning Tree ;
- renouveler périodiquement les comptes et secrets sur un parc important.

L’automatisation consiste à utiliser des outils ou du code pour déployer, exploiter et maintenir le réseau avec moins d’interventions manuelles.

## Ce que les plateformes de gestion résolvent — et ce qu’elles ne résolvent pas

Les plateformes masquent les différences de CLI et réalisent des opérations en lot. Elles restent néanmoins souvent spécifiques à un constructeur, évoluent moins vite que certains besoins métiers et imposent encore un workflow dans leur propre interface.

La programmation apporte une couche de flexibilité : l’ingénieur décrit la règle, les données d’entrée, l’action et la validation. Il peut aussi intégrer plusieurs systèmes dans un même processus.

## Outils et approches

| Approche | Modèle présenté | Usage typique |
| --- | --- | --- |
| Script Python | Code impératif et bibliothèques | Collecte, contrôle et configuration personnalisée. |
| Script shell | Suite de commandes | Tâches simples sur un système. |
| Ansible | Outil Python, exécution SSH | Configuration en lot et orchestration sans agent. |
| SaltStack | Master et Minions | Gestion de configuration et exécution distante. |
| Puppet | Agents et communication HTTPS | Gestion déclarative d’états. |
| Chef | Infrastructure as Code | Configuration et déploiement automatisés. |

Le cours se concentre sur l’automatisation **au niveau de l’équipement** avec Python. Les API des plateformes de gestion constituent un autre niveau, traité dans leurs guides de développement.

## Quand l’automatisation est pertinente

Automatiser est particulièrement utile lorsque la tâche est :

- répétitive ;
- longue ou effectuée sur de nombreux équipements ;
- fondée sur des règles claires ;
- vérifiable automatiquement ;
- appelée à être réexécutée.

Une modification unique et rare peut coûter plus de temps à automatiser qu’à réaliser manuellement. La décision doit intégrer le coût de développement, de test, de maintenance et le risque d’impact à grande échelle.

## Workflow minimal

Le Training Material présente un cas simple :

1. préparer un fichier de commandes ;
2. écrire un programme Python ;
3. ouvrir une connexion SSH/Telnet ;
4. livrer les commandes à l’équipement.

Pour une exploitation maîtrisée, ajoutez : inventaire, validation des entrées, précontrôles, exécution par lot limité, post-contrôles, journalisation et plan de retour arrière.

## Pratique TianSemi : choisir avant de coder

Pour chacun des scénarios suivants, décidez s’il faut automatiser et justifiez :

1. modifier un seul VLAN sur un switch de lab ;
2. vérifier la présence de STelnet sur 500 switches ;
3. sauvegarder chaque nuit la configuration de 80 équipements ;
4. changer un mot de passe sur 1 000 appareils ;
5. mettre à jour un routeur critique sans environnement de test.

Pour un scénario retenu, rédigez : entrée, préconditions, action, résultat attendu, preuve, journal et retour arrière.

## Risques à contrôler

| Risque | Mesure |
| --- | --- |
| Erreur multipliée sur tout le parc | Canari, lots limités et arrêt sur erreur. |
| Secret exposé dans le code | Coffre de secrets ou saisie sécurisée, jamais Git. |
| Équipement inattendu | Inventaire, identité SSH et contrôle du modèle/version. |
| Faux succès | Lire stdout, stderr, état final et code de retour. |
| Absence d’audit | Horodatage, cible, action, résultat et opérateur. |
| Perte de service | Fenêtre, sauvegarde et procédure de rollback testée. |

## À retenir pour l’examen HCIA-Datacom

- L’automatisation réduit les tâches manuelles répétitives et améliore l’efficacité de l’O&M.
- Elle peut s’appuyer sur des langages ou sur des outils spécialisés.
- Python est adapté aux tâches répétitives, longues et fondées sur des règles.
- Ansible utilise SSH et est construit sur Python ; SaltStack suit un modèle Master/Minion.
- Toutes les tâches ne méritent pas un développement d’automatisation.
- Le cas d’usage fondamental du cours consiste à générer puis livrer des commandes aux équipements.

## Révision TianSemi

1. Quels problèmes pose l’exploitation équipement par équipement ?
2. Quels avantages et limites présentent les plateformes de gestion ?
3. Comment définir l’automatisation réseau ?
4. Dans quels cas un script Python apporte-t-il le plus de valeur ?
5. Pourquoi une tâche ponctuelle ne mérite-t-elle pas toujours d’être automatisée ?
6. Quelles différences de modèle existent entre Ansible, SaltStack et Puppet ?
7. Quelles étapes de sécurité faut-il ajouter au workflow simple fichier + script ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 984 à 987.
