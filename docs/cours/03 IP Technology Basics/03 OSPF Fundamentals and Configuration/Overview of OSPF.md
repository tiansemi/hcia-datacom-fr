---
id: HUA-HCIA-DC-036
title: "Overview of OSPF"
official_title: "Overview of OSPF"
course_unit: "3.3.1"
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
source_pages: "PDF 447-468"
tags: [huawei, hcia, datacom, ospf, link-state, lsa, lsdb, spf, router-id]
prerequisites:
  - "[[Advanced Routing Features]]"
related:
  - "[[Working Principles of OSPF]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble d'OSPF (Overview of OSPF)

## Pourquoi un protocole dynamique ?

Les routes statiques conviennent aux réseaux petits et stables. Quand le nombre d'équipements ou de changements augmente, leur maintenance devient lourde et elles ne convergent pas automatiquement après une panne.

Un protocole de routage dynamique échange des informations, recalcule les meilleurs chemins et met à jour les routes lorsque la topologie évolue.

## Classification présentée par Huawei

| Critère | Catégorie | Exemples |
| --- | --- | --- |
| Portée | IGP, à l'intérieur d'un système autonome | RIP, OSPF, IS-IS |
| Portée | EGP, entre systèmes autonomes | BGP |
| Mécanisme | Vecteur de distance | RIP, BGP dans la classification du cours |
| Mécanisme | État de liens | OSPF, IS-IS |

Un protocole à vecteur de distance connaît surtout la direction et la distance annoncées par ses voisins. Un protocole à état de liens décrit les liaisons, construit une représentation de la topologie puis calcule ses propres chemins.

## OSPF en une chaîne logique

![De la relation de voisinage aux routes OSPF](<../../../assets/diagrammes/03 IP Technology Basics/ospf-link-state-cycle-fr.svg>)

1. Les routeurs adjacents établissent une relation de voisinage.
2. Ils diffusent des **LSA** décrivant leurs liens.
3. Chaque routeur rassemble ces LSA dans sa **LSDB**.
4. L'algorithme **SPF** construit un arbre des plus courts chemins dont le routeur local est la racine.
5. Les meilleurs chemins deviennent des routes installées dans la table IP.

OSPF échange donc des états de liens, pas directement des tables de routage complètes.

## Versions et portée

- **OSPFv2**, défini par RFC 2328, est utilisé pour IPv4 ;
- **OSPFv3**, présenté avec RFC 2740 dans le support, est utilisé pour IPv6 ;
- dans cette unité, « OSPF » désigne OSPFv2 sauf indication contraire ;
- OSPF est un IGP largement utilisé, notamment au cœur et à l'agrégation des réseaux campus.

OSPF prend en charge VLSM et une conception multi-aire adaptée aux réseaux plus vastes.

## Concepts fondamentaux

### Aire

Une aire regroupe logiquement des interfaces et des routeurs. Son identifiant peut s'écrire comme un entier ou en notation décimale pointée, par exemple `0` ou `0.0.0.0`.

### Router ID

Le Router ID identifie de façon unique un routeur dans le domaine OSPF. Huawei recommande une configuration manuelle, souvent avec l'adresse d'une loopback stable.

### Coût

Chaque interface OSPF possède un coût. Par défaut dans le cours :

```text
coût = bande passante de référence / bande passante de l'interface
```

La référence par défaut est 100 Mbit/s. Le coût d'une route est la somme des coûts des interfaces de sortie du chemin. Avec cette référence, Fast Ethernet et Gigabit Ethernet obtiennent tous deux 1 ; une référence adaptée peut être nécessaire pour les différencier.

## Cinq types de paquets OSPF

| Paquet | Fonction |
| --- | --- |
| Hello | Découvrir et maintenir les voisins |
| DD | Résumer la LSDB pendant la synchronisation |
| LSR | Demander des LSA précises |
| LSU | Transporter les LSA demandées ou mises à jour |
| LSAck | Accuser réception des LSA |

## Trois tables à distinguer

```text
<R1> display ospf peer
<R1> display ospf lsdb
<R1> display ospf routing
```

| Table | Contenu |
| --- | --- |
| Voisins | Router ID, adresse, état et informations du pair |
| LSDB | LSA locales et reçues, vision de la topologie de l'aire |
| Table OSPF | Routes calculées par OSPF, avec coût et next hop |

La table de routage OSPF est distincte de la table IP globale. Une route calculée par OSPF peut rester inactive si une source de meilleure préférence fournit la même destination.

## Pratique TianSemi : suivre une route

1. Affichez les voisins et relevez leur état.
2. Affichez la LSDB et identifiez le Router ID annonçant chaque entrée.
3. Affichez la table OSPF puis la table IP globale.
4. Repérez une route OSPF active et vérifiez destination, coût, next hop et interface.
5. Repérez, si possible, une route OSPF inactive parce qu'une route directe est prioritaire.

## À retenir pour l'examen HCIA-Datacom

- OSPF est un IGP à état de liens.
- OSPFv2 sert IPv4 ; OSPFv3 sert IPv6.
- La chaîne essentielle est voisinage → LSA → LSDB → SPF → routes.
- Le Router ID doit être unique.
- Le coût d'un chemin est cumulatif.
- Hello, DD, LSR, LSU et LSAck ont des rôles distincts.
- Voisins, LSDB, table OSPF et table IP ne doivent pas être confondus.

## Révision TianSemi

1. Pourquoi les routes statiques deviennent-elles difficiles à maintenir à grande échelle ?
2. Quelle différence fondamentale sépare vecteur de distance et état de liens ?
3. Quelles sont les cinq étapes de fonctionnement d'un protocole à état de liens ?
4. Quel est le rôle du Router ID et pourquoi utiliser une loopback ?
5. Comment OSPF calcule-t-il le coût d'un chemin ?
6. Quels sont les cinq types de paquets OSPF ?
7. Que contiennent la table de voisins, la LSDB et la table OSPF ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 447 à 468.
