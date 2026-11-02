---
id: HUA-HCIA-DC-046
title: "AAA Overview"
official_title: "AAA Overview"
course_unit: "4.2.1"
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
source_pages: "PDF 591-599"
tags: [huawei, hcia, datacom, aaa, authentication, authorization, accounting, radius]
prerequisites:
  - "[[ACL Configuration Examples]]"
related:
  - "[[Deploying AAA]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble d'AAA (AAA Overview)

## Pourquoi contrôler les utilisateurs

Dans un réseau d'entreprise, les participants ne sont pas seulement des adresses IP. Il faut savoir **qui** demande l'accès, **ce qu'il peut faire** et, si nécessaire, **quelles ressources il utilise**.

Huawei distingue notamment :

- les **utilisateurs d'accès**, qui consomment des services réseau depuis leurs terminaux ;
- les **administrateurs réseau**, qui se connectent aux équipements pour les exploiter et les maintenir.

Un contrôle fondé uniquement sur des adresses IP ou MAC devient difficile à administrer à grande échelle. AAA fournit un cadre de contrôle centré sur l'identité de l'utilisateur.

## Les trois fonctions d'AAA

![Les trois fonctions AAA appliquées à une session utilisateur](<../../../assets/diagrammes/04 Network Security and Services/aaa-three-functions-fr.svg>)

| Fonction | Question traitée | Résultat attendu |
| --- | --- | --- |
| **Authentication** | Qui êtes-vous ? | Vérifier l'identité et décider si l'accès peut commencer. |
| **Authorization** | Que pouvez-vous faire ? | Accorder des droits et limiter les services disponibles. |
| **Accounting** | Qu'avez-vous utilisé ? | Enregistrer les opérations, la durée ou le trafic d'une session. |

Ces fonctions sont distinctes. Un déploiement peut utiliser uniquement l'authentification, ou combiner les trois selon le besoin métier.

## Architecture client/serveur

AAA suit une architecture client/serveur :

- le **client AAA** est généralement l'équipement réseau qui reçoit la demande de connexion, déclenche le contrôle et gère l'accès ;
- le **serveur AAA** conserve les informations nécessaires et centralise l'authentification, l'autorisation et éventuellement la comptabilisation.

Dans un déploiement local, l'équipement joue lui-même le rôle de serveur. Dans un déploiement distant, il échange avec un serveur externe.

## Protocoles associés à AAA

| Solution | Transport | Particularité dans le cours Huawei |
| --- | --- | --- |
| **RADIUS** | UDP, ports 1812 et 1813 | Authentification et autorisation combinées ; protocole le plus courant. |
| **HWTACACS** | TCP, port 49 | Authentification, autorisation et accounting indépendants. |
| **LDAP** | TCP/IP | Accès à un annuaire ; ne fournit pas l'accounting. |
| **AD** | Service d'annuaire Windows fondé sur LDAP | Authentification et autorisation des utilisateurs Windows ; pas d'accounting. |

Un client et un serveur RADIUS protègent leurs échanges à l'aide d'une **clé partagée** connue des deux extrémités. Le serveur conserve généralement une base d'utilisateurs, une base de clients RADIUS et un dictionnaire d'attributs.

## Deux scénarios administratifs

### AAA local

L'équipement compare les identifiants saisis avec sa propre base locale, puis applique les droits configurés. Cette approche est rapide et simple pour un petit nombre d'administrateurs, mais dépend de la capacité de stockage et de la gestion propre à chaque équipement.

### AAA distant avec RADIUS

L'équipement transmet la demande au serveur RADIUS. Celui-ci vérifie l'utilisateur et renvoie le résultat. La centralisation facilite la gestion d'un grand nombre de comptes et d'équipements.

## Choisir l'approche

| Critère | AAA local | AAA distant |
| --- | --- | --- |
| Dépendance réseau | Faible | Le serveur doit être joignable. |
| Administration | Répétée sur chaque équipement | Centralisée. |
| Échelle naturelle | Faible nombre de comptes | Parc important et nombreux utilisateurs. |
| Usage typique du cours | Administrateurs | Utilisateurs d'accès ou administration centralisée. |

## À retenir pour l'examen HCIA-Datacom

- AAA signifie authentication, authorization et accounting.
- L'authentification vérifie l'identité, l'autorisation attribue des droits et l'accounting enregistre l'utilisation.
- Un équipement réseau agit généralement comme client AAA.
- AAA peut être local ou s'appuyer sur un serveur distant.
- RADIUS est le protocole AAA le plus couramment utilisé dans les scénarios réels présentés par Huawei.
- RADIUS utilise UDP 1812 pour l'authentification et UDP 1813 pour l'accounting.
- HWTACACS utilise TCP 49 et sépare les trois fonctions.

## Révision TianSemi

1. Quel problème de gestion AAA résout-il mieux qu'un contrôle fondé uniquement sur les adresses IP ?
2. Quelle différence existe entre authentification et autorisation ?
3. Quel est le rôle du client AAA dans l'architecture Huawei ?
4. Pourquoi AAA distant est-il adapté à une administration centralisée ?
5. Quels ports et quel protocole de transport RADIUS utilise-t-il ?
6. Quelle différence structurelle distingue RADIUS de HWTACACS ?
7. LDAP et AD fournissent-ils une fonction d'accounting ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 591 à 599.
