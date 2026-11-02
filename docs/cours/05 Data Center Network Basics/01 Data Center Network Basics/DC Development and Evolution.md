---
id: HUA-HCIA-DC-058
title: "DC Development and Evolution"
official_title: "DC Development and Evolution"
course_unit: "5.1.2"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-11
updated: 2026-08-11
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 747-752"
tags: [huawei, hcia, datacom, data-center, evolution, disaster-recovery, virtualization, cloud]
prerequisites:
  - "[[What Is a DC]]"
related:
  - "[[General-Purpose Computing DCN]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Développement et évolution des data centers (DC Development and Evolution)

## De l'ordinateur isolé au cloud

![Les grandes étapes de l'évolution des data centers](<../../../assets/diagrammes/05 Data Center Network Basics/dc-evolution-timeline-fr.svg>)

| Période | Forme dominante | Échelle indicative du support |
| --- | --- | --- |
| Années 1940 | Superordinateur occupant une salle | 1 serveur |
| Années 1960 | Concentration de plusieurs serveurs | Plusieurs serveurs |
| Années 1990 | Salle de serveurs interconnectés pour le modèle client/serveur | Centaines de serveurs |
| Années 2010 | Cloud et centres de calcul intelligent | Dizaines de milliers de serveurs |

L'évolution résulte à la fois de la miniaturisation, de la mise en réseau, de la virtualisation, du cloud et des besoins de l'IA.

## Du centre unique au multi-site

Huawei décrit quatre phases de résilience :

1. un seul DC ;
2. un DC principal avec reprise après sinistre distante ;
3. deux DC métropolitains actifs avec un site de reprise distant ;
4. plusieurs DC multi-sites, combinant active/standby et services actifs sur plusieurs centres.

L'objectif est de maintenir les services malgré une panne de site. **DR** signifie *Disaster Recovery*.

## De la machine physique au cloud

| Phase | Ressource de calcul | Déploiement réseau |
| --- | --- | --- |
| Machine physique | Services directement sur serveurs | Configuration manuelle. |
| Machine virtuelle | VM créées par virtualisation | Contrôleur réseau partiellement automatisé, mais domaines encore séparés. |
| Cloud privé | Ressources dédiées à une organisation | Plateforme unifiée calcul, stockage et réseau. |
| Cloud hybride/public | Services répartis selon sécurité, résilience et coût | Ressources consommées comme services. |

La virtualisation améliore l'utilisation du matériel et accélère le déploiement ou l'extension des services. Le cloud privé reste dédié à une organisation ; le cloud public partage une infrastructure opérée par un fournisseur.

## Conséquence pour le réseau

Chaque transformation des services impose au DCN davantage :

- d'automatisation ;
- d'élasticité ;
- de multi-tenance ;
- de mobilité ;
- de résilience inter-sites ;
- de capacité pour les flux internes.

## Pratique TianSemi : choisir une phase

Une entreprise possède un site principal, veut reprendre ses transactions à plus de 200 km et déployer rapidement des environnements de test. Proposez une phase multi-site et une phase de déploiement cloud, puis justifiez les choix en séparant continuité, automatisation et coût.

## À retenir pour l'examen HCIA-Datacom

- Les DC ont évolué de la salle d'un superordinateur vers le cloud et le calcul intelligent.
- La résilience évolue du centre unique vers plusieurs sites et plusieurs centres.
- La virtualisation fait porter plusieurs VM par un serveur physique.
- Le cloud privé unifie les ressources dédiées à une organisation.
- Le cloud public fournit des ressources partagées comme services.
- L'évolution des services entraîne l'évolution et l'automatisation du DCN.

## Révision TianSemi

1. Quelles innovations distinguent les quatre grandes périodes des DC ?
2. Pourquoi les organisations passent-elles d'un DC unique au multi-site ?
3. Quel rôle joue un site de reprise distant ?
4. Quel bénéfice principal apporte la virtualisation ?
5. Quelle différence existe entre cloud privé et cloud public ?
6. Pourquoi le réseau doit-il évoluer avec le modèle de déploiement des services ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 747 à 752.
