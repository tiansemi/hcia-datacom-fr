---
id: HUA-HCIA-DC-076
title: "Introduction to iMaster NCE"
official_title: "Introduction to iMaster NCE"
course_unit: "7.1.5"
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
source_pages: "PDF 973-978"
tags: [huawei, hcia, datacom, imaster-nce, campus, automation, controller, ztp, oam]
prerequisites:
  - "[[RESTCONF]]"
related:
  - "[[Introduction to Network Programming and Automation]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Introduction à iMaster NCE (Introduction to iMaster NCE)

## Le « cerveau » d’un réseau basé sur le cloud

iMaster NCE réunit trois capacités : **gestion**, **contrôle** et **analyse**. La plateforme vise une gestion centralisée, l’automatisation du cycle de vie et une exploitation plus intelligente.

![Architecture et bénéfices d’iMaster NCE-Campus](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/imaster-nce-campus-fr.svg>)

Sa valeur ne réside pas dans un protocole supplémentaire. Elle orchestre plusieurs mécanismes — HTTP, SSH, SNMP et NETCONF — pour transformer une intention ou une politique en opérations cohérentes sur l’infrastructure.

## Architecture produit

Huawei présente une architecture en trois niveaux :

1. **plateforme cloud unifiée** : déploiement cloud public ou privé, extension élastique et haute fiabilité ;
2. **gestion, contrôle et analyse** : conception programmable, données, automatisation et analyse intelligente ;
3. **applications par scénario** : fonctions adaptées aux domaines et secteurs.

La plateforme cherche à couvrir plusieurs domaines, couches, constructeurs, services et locataires. Des API REST ouvertes permettent l’intégration avec des applications tierces.

## iMaster NCE-Campus

iMaster NCE-Campus est la plateforme principale de configuration et de gestion dans la solution campus Huawei. Elle gère les équipements par des protocoles southbound tels que SNMP, NETCONF, HTTP et SSH.

### Simplifier

- **planification** : vues d’ingénierie au niveau bâtiment et placement automatisé via WLAN Planner ;
- **déploiement** : enregistrement automatique, ZTP et livraison de services de bout en bout ;
- **provisionnement** : politiques et orientation intelligente du trafic selon les applications ;
- **O&M** : visibilité sur trafic, qualité, alarmes, journaux, topologie et cartes GIS.

### Étendre de manière élastique

- ajout de sites, équipements et services cloud à la demande ;
- prise en charge de réseaux distribués à grande échelle ;
- multi-tenant et choix entre exploitation interne et Managed Service Provider.

### Ouvrir

- exposition de données telles que les enregistrements d’authentification et statistiques ;
- API northbound pour les applications et services à valeur ajoutée ;
- intégration avec l’écosystème métier.

## Du protocole à la plateforme

| Niveau | Question | Exemple |
| --- | --- | --- |
| Protocole | Comment échanger avec un équipement ? | SNMP, NETCONF, RESTCONF, SSH. |
| Contrôleur | Comment appliquer une politique à plusieurs équipements ? | iMaster NCE-Campus. |
| Application | Comment répondre à un besoin métier ou opérationnel ? | santé du réseau, actifs, O&M intelligent. |

iMaster NCE-Campus peut s’appuyer sur DHCP Option 148 et NETCONF pour l’enregistrement automatique des équipements, puis appliquer la configuration prévue depuis le contrôleur.

## Pratique TianSemi : scénario ZTP raisonné

Une entreprise ouvre dix agences identiques.

1. définissez les paramètres que le DHCP doit fournir à un switch à l’état usine ;
2. décrivez son enregistrement auprès d’iMaster NCE-Campus ;
3. distinguez les flux southbound vers les équipements et northbound vers les applications ;
4. listez les politiques communes à livrer automatiquement ;
5. déterminez les données à collecter pour l’O&M ;
6. prévoyez le comportement en cas d’indisponibilité temporaire du contrôleur.

## Limites à garder en tête

- Une plateforme centralisée doit être dimensionnée et sécurisée comme un composant critique.
- L’automatisation amplifie une bonne politique comme une mauvaise ; modèles et validations sont indispensables.
- Les protocoles southbound doivent être protégés et leurs droits limités.
- La visibilité dépend de la qualité des données remontées par les équipements.

## À retenir pour l’examen HCIA-Datacom

- iMaster NCE combine gestion, contrôle et analyse.
- iMaster NCE-Campus est la plateforme de configuration et de gestion du campus Huawei.
- Elle utilise notamment SNMP, NETCONF, HTTP et SSH vers l’infrastructure.
- Ses bénéfices sont résumés par simplification, élasticité et ouverture.
- ZTP accélère l’enregistrement et le déploiement de masse.
- Les API northbound permettent l’intégration d’applications tierces.

## Révision TianSemi

1. Quelles trois capacités iMaster NCE intègre-t-il ?
2. Quels sont les trois grands niveaux de son architecture ?
3. Quel rôle remplit iMaster NCE-Campus ?
4. Quels protocoles southbound sont cités par Huawei ?
5. Comment ZTP simplifie-t-il le déploiement ?
6. Que signifient les bénéfices « simplifié, élastique et ouvert » ?
7. Quelle différence existe entre API northbound et protocole southbound ?
8. Pourquoi le contrôleur doit-il être considéré comme critique ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 973 à 978.
