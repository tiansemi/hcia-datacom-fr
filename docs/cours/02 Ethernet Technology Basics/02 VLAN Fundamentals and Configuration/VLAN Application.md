---
id: HUA-HCIA-DC-016
title: "VLAN Application"
official_title: "VLAN Application"
course_unit: "2.2.3"
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
source_pages: "PDF 216-218"
tags: [huawei, hcia, datacom, vlan, planning, campus-network]
prerequisites:
  - "[[Basic Concepts of VLANs]]"
related:
  - "[[VLAN Configuration Examples]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Application des VLAN (VLAN Application)

## Principes de planification

Huawei propose de structurer les VLAN selon le besoin métier plutôt que selon la position physique seule :

- **par service** : voix, vidéo, documents ;
- **par département** : ingénierie, marketing, finance ;
- **par application ou usage** : serveurs, bureaux, salles de classe.

![Exemple de planification VLAN cohérente avec les services d'un campus](<../../../assets/diagrammes/02 Ethernet Technology Basics/vlan-campus-planning-fr.svg>)

Le support recommande des identifiants consécutifs pour mieux utiliser et administrer la plage disponible. Il suggère aussi d'associer la logique des VID à celle des sous-réseaux IP afin de faciliter la lecture du plan.

## Exemple de campus Huawei

Le scénario comprend trois bâtiments : administration, enseignement et bureaux. Certains services existent dans plusieurs bâtiments, mais les utilisateurs d'un même service appartiennent au même VLAN logique.

| VLAN du support | Segment IP | Usage |
| ---: | --- | --- |
| 1 | `X.16.10.0/24` | Utilisateurs de bureaux |
| 2 | `X.16.20.0/24` | Utilisateurs Finance |
| 3 | `X.16.30.0/24` | Salles de classe |
| 100 | `Y.16.100.0/24` | Gestion des équipements |

Les lettres `X` et `Y` sont des valeurs génériques dans le support ; elles ne constituent pas des adresses à recopier telles quelles dans une configuration réelle.

## Scénario multi-entreprises

Plusieurs entreprises d'un même bâtiment partagent une infrastructure et un accès Internet. Leurs réseaux arrivent sur des ports différents d'un switch de couche 2.

L'affectation par interface place chaque entreprise dans un VLAN indépendant :

- entreprise 1 dans le VLAN 2 ;
- entreprise 2 dans le VLAN 3 ;
- entreprise 3 dans le VLAN 4.

Chaque VLAN devient un groupe de travail virtuel séparé à la couche 2, tandis qu'un équipement de couche 3 fournit l'accès contrôlé aux ressources communes ou à Internet.

## Méthode TianSemi de planification

1. Inventorier les groupes qui doivent partager des broadcasts.
2. Identifier les groupes qui doivent être séparés.
3. Attribuer un VLAN et un segment IP cohérents à chaque groupe.
4. Réserver un VLAN distinct pour la gestion des équipements.
5. Déterminer les ports access et les liaisons qui devront transporter plusieurs VLAN.
6. Documenter le VID, le sous-réseau, le service, les ports et les règles inter-VLAN.

## Contrôle de cohérence avant configuration

| Question | Risque évité |
| --- | --- |
| Deux services isolés partagent-ils le même VLAN ? | Fuite de trafic de couche 2 |
| Un même service possède-t-il des VID différents sur deux switchs ? | Rupture de continuité logique |
| Le VLAN de gestion est-il réservé ? | Mélange du trafic utilisateur et d'administration |
| Les VID et sous-réseaux sont-ils documentés ? | Erreurs de maintenance |
| Le routage inter-VLAN est-il explicitement prévu ? | Attente incorrecte de communication entre VLAN |

## À retenir pour l'examen HCIA-Datacom

- Les VLAN peuvent être planifiés par service, département ou application.
- Des utilisateurs géographiquement séparés peuvent appartenir au même VLAN.
- L'affectation par interface est la méthode la plus courante.
- Un plan cohérent rapproche VID, sous-réseaux et usages.
- Le VLAN de gestion doit être distingué des VLAN utilisateurs.

## Révision TianSemi

1. Quels trois critères de planification VLAN Huawei propose-t-il ?
2. Pourquoi utiliser des VID consécutifs et documentés ?
3. Comment regrouper un même département présent dans plusieurs bâtiments ?
4. Pourquoi prévoir un VLAN de gestion distinct ?
5. Dans le scénario multi-entreprises, quel mécanisme assure l'isolation de couche 2 ?
6. Quel équipement ou quelle fonction permet ensuite l'accès à Internet ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 216 à 218.
