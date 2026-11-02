---
id: HUA-HCIA-DC-042
title: "IPv6 Address Configuration Example"
official_title: "IPv6 Address Configuration Example"
course_unit: "3.4.4"
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
source_pages: "PDF 551-560"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 125-135"
tags: [huawei, hcia, datacom, ipv6-configuration, slaac, static-route, ra, link-local]
prerequisites:
  - "[[IPv6 Address Autoconfiguration]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Exemple de configuration d'adresses IPv6 (IPv6 Address Configuration Example)

## Topologie et objectifs

![Topologie du lab IPv6 : SLAAC, adresses statiques et routes statiques](<../../../assets/diagrammes/03 IP Technology Basics/ipv6-config-topology-fr.svg>)

Le lab officiel fait obtenir une adresse globale à S1 par SLAAC, utilise des adresses statiques entre R1 et S2, puis ajoute des routes statiques pour joindre la loopback `2000::2/128`.

## Activer IPv6 et la link-local

```text
[S1] interface GE 1/0/1
[S1-GE1/0/1] undo portswitch
[S1-GE1/0/1] ipv6 enable
[S1-GE1/0/1] ipv6 address auto link-local
```

Sur une destination link-local, précisez le lien :

```text
<S1> ping ipv6 FE80::B2C7:87FF:FE33:D660 -i GE 1/0/1
```

## Adresses globales statiques

```text
[R1-GE0/0/1] ipv6 enable
[R1-GE0/0/1] ipv6 address 2000:12::2 64
[R1-GE0/0/2] ipv6 enable
[R1-GE0/0/2] ipv6 address 2000:23::1 64
[S2-GE1/0/1] ipv6 enable
[S2-GE1/0/1] ipv6 address 2000:23::2 64
[S2-LoopBack0] ipv6 enable
[S2-LoopBack0] ipv6 address 2000::2 128
```

## SLAAC sur S1

Client :

```text
[S1-GE1/0/1] ipv6 address auto global default
```

Routeur :

```text
[R1-GE0/0/1] ipv6 nd ra halt disable
[R1-GE0/0/1] ipv6 nd ra prefix 2000:12::/64 3600 1200
```

`default` autorise l'installation d'une route par défaut apprise depuis le RA. Huawei désactive par défaut l'émission de RA sur l'interface routeur ; `ipv6 nd ra halt disable` l'active.

## Routes statiques IPv6

```text
[R1] ipv6 route-static 2000::2 128 2000:23::2
[S2] ipv6 route-static 2000:12:: 64 2000:23::1
```

Comme en IPv4, la route retour est indispensable. S1 possède sa route par défaut via la link-local de R1 ; R1 doit joindre la loopback de S2 et S2 doit retourner vers le préfixe SLAAC de S1.

## Vérifications

```text
<S1> display ipv6 interface GE 1/0/1
<S1> display ipv6 routing-table
<R1> display ipv6 neighbors GE 0/0/2
<S1> ping ipv6 2000::2
```

Vérifiez successivement :

1. IPv6 `UP` et link-local présente ;
2. adresse globale SLAAC présente et non tentative ;
3. route par défaut via la link-local du routeur ;
4. voisin NDP appris ;
5. routes aller et retour ;
6. ping de bout en bout.

## Variante du Training Material

Le support montre aussi :

```text
[R3-GE0/0/0] ipv6 address auto dhcp
[R2-GE0/0/1] ipv6 nd ra halt disable
[R4-GE0/0/0] ipv6 address auto global
```

Cette variante compare DHCPv6 stateful pour R3 et SLAAC pour R4. Les commandes DHCPv6 serveur dépendent de la plateforme et de la version VRP ; vérifiez la compatibilité du modèle utilisé.

## Mini-lab TianSemi

1. Reproduisez la topologie S1–R1–S2 du Lab Guide.
2. Activez IPv6 et générez automatiquement les link-local.
3. Testez les link-local avec l'interface `-i`.
4. Configurez les adresses globales statiques de R1 et S2.
5. Activez RA et SLAAC pour S1.
6. Observez l'échec vers `2000::2` avant routage.
7. Ajoutez les deux routes statiques, vérifiez la table puis recommencez le ping.

## Troubleshooting

| Symptôme | Contrôle prioritaire |
| --- | --- |
| IPv6 absent | `ipv6 enable` sur chaque interface concernée |
| Ping link-local échoue | Interface `-i`, état du lien et adresse cible |
| S1 ne reçoit pas de globale | Émission RA et préfixe RA |
| Globale présente sans défaut | Mot-clé `default` et RA du routeur |
| Voisin non appris | NS/NA et couche 2 |
| Ping distant échoue | Route statique aller et route retour |

## À retenir pour l'examen HCIA-Datacom

- `ipv6 enable` active IPv6 par interface.
- Une link-local peut être générée avec `ipv6 address auto link-local`.
- Le routeur Huawei n'envoie pas de RA par défaut dans le scénario présenté.
- `ipv6 address auto global default` combine SLAAC et apprentissage de la défaut.
- `display ipv6 interface`, `neighbors` et `routing-table` couvrent les vérifications clés.
- Une route statique IPv6 suit la même logique aller/retour qu'en IPv4.

## Révision TianSemi

1. Quelles commandes activent IPv6 et génèrent une link-local ?
2. Pourquoi un ping link-local doit-il préciser l'interface ?
3. Comment activer l'émission RA sur une interface Huawei ?
4. Que change le mot-clé `default` dans l'autoconfiguration globale ?
5. Quelles routes permettent à S1 de joindre `2000::2/128` et de recevoir la réponse ?
6. Dans quel ordre faut-il diagnostiquer une panne IPv6 dans ce lab ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 551 à 560.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 125 à 135 pour les adresses statiques, SLAAC, NDP et les routes statiques IPv6.
