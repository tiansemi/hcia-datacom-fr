---
id: HUA-HCIA-DC-047
title: "Deploying AAA"
official_title: "Deploying AAA"
course_unit: "4.2.2"
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
source_pages: "PDF 600-608"
tags: [huawei, hcia, datacom, aaa, authentication-scheme, authorization-scheme, accounting-scheme, domain]
prerequisites:
  - "[[AAA Overview]]"
related:
  - "[[Configuration Examples for AAA]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Déploiement d'AAA (Deploying AAA)

## Trois schémas indépendants

Sur les commutateurs Huawei présentés dans le cours, le déploiement sépare :

- le **schéma d'authentification** ;
- le **schéma d'autorisation** ;
- le **schéma d'accounting**.

Chaque schéma précise la ou les méthodes à utiliser. Un équipement jouant le rôle de serveur AAA local assure l'authentification et l'autorisation locales, mais pas l'accounting.

| Schéma | Méthodes importantes du cours |
| --- | --- |
| Authentification | locale, RADIUS, HWTACACS, AD, LDAP, aucune authentification |
| Autorisation | locale, RADIUS, HWTACACS, AD, LDAP, aucune autorisation |
| Accounting | RADIUS, HWTACACS, aucun accounting |

Les modes « aucun » supposent un choix explicite de confiance ou d'absence de service. Ils ne constituent pas une valeur sûre par défaut.

## Local ou serveur distant

### Authentification locale

Le compte et son mot de passe sont enregistrés sur l'équipement. Lorsqu'une application comme STelnet, Telnet ou FTP transporte les identifiants, l'équipement consulte sa base locale.

Avantages : traitement rapide et coût d'exploitation réduit. Limites : capacité matérielle et multiplication des bases locales.

### Authentification distante

Le compte réside sur le serveur AAA. L'équipement lui transmet la demande avec le protocole choisi, puis applique le résultat reçu.

Une liste de méthodes peut contenir des solutions de secours. L'équipement essaie la méthode suivante uniquement si la précédente **ne répond pas**. Si un serveur répond en refusant l'utilisateur, le processus s'arrête : ce refus n'est pas assimilé à une panne autorisant un repli local.

## Autorisation et niveau de privilège

En local, le niveau de privilège associé au compte détermine les commandes accessibles après connexion. Un utilisateur ne peut exécuter que les commandes de son niveau ou d'un niveau inférieur.

En autorisation distante, le serveur renvoie les droits. Si des attributs sont aussi configurés dans le domaine, ceux fournis par le serveur ont priorité en cas de conflit ; en l'absence de conflit, les deux ensembles peuvent s'appliquer.

## Accounting

Lorsque l'accounting est activé, l'équipement envoie au serveur des messages `Accounting-Request` décrivant l'activité de la session. Le serveur répond avec `Accounting-Response` et conserve les données nécessaires à l'audit ou à la facturation.

Un schéma d'accounting ne sélectionne qu'une méthode à la fois.

## Le domaine relie les composants

![Un domaine AAA relie les schémas, le serveur et les utilisateurs](<../../../assets/diagrammes/04 Network Security and Services/aaa-domain-binding-fr.svg>)

Chaque utilisateur appartient à un **domaine** et utilise la configuration AAA de ce domaine. Le domaine rassemble notamment :

- les schémas d'authentification, d'autorisation et d'accounting ;
- le modèle du serveur distant, lorsqu'il existe ;
- les informations d'autorisation propres au domaine.

Le nom saisi lors de la connexion peut prendre la forme `utilisateur@domaine`. Par défaut, le séparateur est `@` et le domaine se trouve après ce séparateur.

Huawei distingue deux domaines globaux intégrés :

- `default_admin` pour les utilisateurs administrateurs ;
- `default` pour les utilisateurs ordinaires.

Si aucun domaine valide n'est indiqué, le type d'utilisateur détermine le domaine global utilisé.

## Diagnostic conceptuel

Lorsqu'une connexion AAA échoue, vérifier dans cet ordre :

1. le nom complet saisi et le domaine qui en résulte ;
2. les schémas liés à ce domaine ;
3. l'ordre des méthodes dans chaque schéma ;
4. la présence du compte local ou la disponibilité du serveur distant ;
5. les droits retournés ou configurés ;
6. le service d'accès autorisé pour l'utilisateur.

## À retenir pour l'examen HCIA-Datacom

- Les trois schémas AAA sont déployés indépendamment.
- Un serveur AAA local Huawei n'assure pas l'accounting.
- Une méthode de secours est essayée lorsque la méthode précédente ne répond pas, pas lorsqu'elle refuse explicitement l'utilisateur.
- L'autorisation locale s'appuie notamment sur le niveau de privilège.
- Le domaine est le point d'association entre l'utilisateur et les politiques AAA.
- `default_admin` est le domaine global des administrateurs et `default` celui des utilisateurs ordinaires.
- Par défaut, `@` sépare le nom d'utilisateur du nom de domaine.

## Révision TianSemi

1. Pourquoi Huawei sépare-t-il les schémas d'authentification, d'autorisation et d'accounting ?
2. Dans quel cas la méthode d'authentification suivante est-elle essayée ?
3. Que se passe-t-il si un serveur AAA répond explicitement par un refus ?
4. Quel élément limite les commandes d'un administrateur authentifié localement ?
5. Quelles configurations un domaine AAA peut-il associer ?
6. Quel domaine global s'applique par défaut à un administrateur ?
7. Comment le nom `alice@datacom` est-il interprété avec les paramètres par défaut ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 600 à 608.
