---
id: HUA-HCIA-LAB-009
title: "IPv6 Address Configuration"
official_title: "IPv6 Address Configuration"
category: "Huawei HCIA-Datacom Lab"
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
module: "09 Labs"
type: "lab"
source: "HCIA-Datacom V2.0 Lab Guide"
source_pages: "PDF 125-135 (guide pages 123-133)"
lab_required: true
tags: [huawei, hcia, datacom, lab, ipv6, link-local, slaac, router-advertisement, static-route]
devices: [S1, R1, S2]
commands: [ipv6 enable, ipv6 address, ipv6 address auto, ipv6 nd ra, ipv6 route-static, ping ipv6]
publication_status: "not-started"
site_url:
---

# Lab 09 — Configuration des adresses IPv6

## Objectif

Activer IPv6 sur S1–R1–S2, tester les adresses link-local, configurer des adresses globales statiques, faire obtenir à S1 une adresse et une route par défaut par SLAAC, puis joindre la loopback de S2 grâce aux routes statiques IPv6.

## Topologie

![Chaîne IPv6 entre S1, R1 et S2](<../../assets/diagrammes/09 Labs/lab09-ipv6-topology-fr.svg>)

| Segment | Adresse côté gauche | Adresse côté droit |
| --- | --- | --- |
| S1–R1 | S1 par SLAAC dans `2000:12::/64` | R1 `2000:12::2/64` |
| R1–S2 | R1 `2000:23::1/64` | S2 `2000:23::2/64` |
| Loopback S2 | — | `2000::2/128` |

## 1 — Activer IPv6 et générer les link-local

Sur les quatre interfaces physiques :

```text
[S1-GE1/0/1] ipv6 enable
[R1-GE0/0/1] ipv6 enable
[R1-GE0/0/2] ipv6 enable
[S2-GE1/0/1] ipv6 enable
```

```text
[S1-GE1/0/1] ipv6 address auto link-local
[R1-GE0/0/1] ipv6 address auto link-local
[R1-GE0/0/2] ipv6 address auto link-local
[S2-GE1/0/1] ipv6 address auto link-local
```

Relevez les adresses réelles avec `display ipv6 interface`. Pour joindre une link-local, précisez l’interface de sortie car la même portée `FE80::/10` existe sur chaque lien :

```text
[S1] ping ipv6 <LINK-LOCAL-R1> -i GE 1/0/1
```

## 2 — Configurer les adresses globales

```text
[R1-GE0/0/1] ipv6 address 2000:12::2 64
[R1-GE0/0/2] ipv6 address 2000:23::1 64
[S2-GE1/0/1] ipv6 address 2000:23::2 64
[S2] interface LoopBack 0
[S2-LoopBack0] ipv6 enable
[S2-LoopBack0] ipv6 address 2000::2 128
```

```text
[R1] ping ipv6 2000:23::2
[R1] display ipv6 neighbors GE 0/0/2
```

## 3 — Activer SLAAC entre R1 et S1

```text
[S1-GE1/0/1] ipv6 address auto global default
[R1-GE0/0/1] ipv6 nd ra halt disable
[R1-GE0/0/1] ipv6 nd ra prefix 2000:12::/64 3600 1200
```

R1 envoie des Router Advertisements. S1 construit une adresse globale avec le préfixe reçu et installe une route par défaut dont le next-hop est la link-local de R1.

```text
[S1] display ipv6 interface GE 1/0/1
[S1] display ipv6 routing-table
```

## 4 — Compléter le routage statique

La route par défaut de S1 envoie déjà le trafic à R1. Il manque la route R1 vers la loopback et le retour S2 vers le préfixe de S1 :

```text
[R1] ipv6 route-static 2000::2 128 2000:23::2
[S2] ipv6 route-static 2000:12:: 64 2000:23::1
[S1] ping ipv6 2000::2
```

## Vérifications attendues

- Chaque interface possède une link-local et un état IPv6 Up.
- Le ping link-local réussit uniquement avec la bonne interface de sortie.
- S1 obtient une adresse dans `2000:12::/64` et une route `::/0`.
- Les voisins IPv6 sont visibles sur R1–S2.
- Le ping de S1 vers `2000::2` réussit après les deux routes statiques.

## Troubleshooting

| Symptôme | Cause probable | Correction |
| --- | --- | --- |
| Link-local injoignable | Interface de sortie non précisée | Utiliser `ping ipv6 ... -i <interface>`. |
| S1 sans adresse globale | RA désactivé ou préfixe absent | Vérifier `ipv6 nd ra halt disable` et le préfixe RA. |
| Adresse SLAAC présente, destination distante absente | Routes statiques incomplètes | Ajouter route aller sur R1 et route retour sur S2. |
| Voisin absent | IPv6 non activé ou lien Down | Vérifier `ipv6 enable`, état physique et préfixe du lien. |

## Révision TianSemi

1. Pourquoi faut-il préciser l’interface lors d’un ping vers une link-local ?
2. Quelle différence existe entre link-local et adresse globale dans ce lab ?
3. Quel message permet à S1 de réaliser SLAAC ?
4. Comment S1 obtient-il sa route par défaut ?
5. Quelles routes statiques rendent la loopback S2 joignable depuis S1 ?
6. Pourquoi une adresse globale correcte ne garantit-elle pas à elle seule le chemin retour ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 125 à 135 (pages 123 à 133 du guide).
