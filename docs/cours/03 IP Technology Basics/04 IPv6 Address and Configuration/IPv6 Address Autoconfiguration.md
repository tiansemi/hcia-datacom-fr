---
id: HUA-HCIA-DC-041
title: "IPv6 Address Autoconfiguration"
official_title: "IPv6 Address Autoconfiguration"
course_unit: "3.4.3"
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
source_pages: "PDF 542-550"
tags: [huawei, hcia, datacom, ipv6, slaac, dhcpv6, ra, managed-flag, other-flag]
prerequisites:
  - "[[ICMPv6]]"
related:
  - "[[IPv6 Address Configuration Example]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Autoconfiguration des adresses IPv6 (IPv6 Address Autoconfiguration)

## Trois modèles à distinguer

| Modèle | Adresse | Autres paramètres |
| --- | --- | --- |
| SLAAC | Générée à partir du préfixe RA et d'un Interface ID | Principalement RA |
| DHCPv6 stateful | Fournie par le serveur DHCPv6 | Fournis par DHCPv6 |
| DHCPv6 stateless | Générée par SLAAC | DNS et autres paramètres par DHCPv6 |

Le système d'exploitation du client et les bits **M** et **O** du RA guident le choix :

- `M=1` : obtenir l'adresse et les autres informations par DHCPv6 ;
- `M=0, O=1` : générer l'adresse par SLAAC et demander les autres paramètres à DHCPv6 ;
- `M=0, O=0` : utiliser SLAAC sans complément DHCPv6 indiqué par ces bits.

## Processus SLAAC

![Séquence SLAAC de la link-local à l'adresse globale utilisable](<../../../assets/diagrammes/03 IP Technology Basics/ipv6-slaac-sequence-fr.svg>)

1. L'hôte génère une adresse link-local.
2. Il exécute DAD sur cette adresse.
3. Il envoie un RS aux routeurs du lien.
4. Le routeur répond par RA avec un préfixe utilisable pour SLAAC.
5. L'hôte combine préfixe et Interface ID.
6. Il exécute DAD sur la nouvelle adresse globale.
7. En l'absence de conflit, il utilise l'adresse et la route par défaut apprise.

Le routeur peut envoyer périodiquement des RA même sans RS.

## DHCPv6 à quatre messages

Quand plusieurs serveurs peuvent répondre :

```text
Solicit → Advertise → Request → Reply
```

Le client découvre les serveurs, choisit une proposition, demande les paramètres puis reçoit la confirmation.

## DHCPv6 à deux messages

Avec Rapid Commit et un environnement contrôlé :

```text
Solicit → Reply
```

Ce mode accélère l'attribution mais doit éviter que plusieurs serveurs répondent simultanément avec des paramètres concurrents.

## Relais DHCPv6

Le client diffuse Solicit vers `FF02::1:2`, groupe des serveurs et relais DHCPv6 du lien. Le relais encapsule et transmet la demande au serveur distant, puis remet la réponse au client. Il est nécessaire lorsque client et serveur se trouvent sur des segments différents.

## Choisir le bon modèle

- terminaux utilisateurs simples : SLAAC, éventuellement DHCPv6 stateless pour DNS ;
- besoin de contrôle central de l'adresse : DHCPv6 stateful ;
- serveur distant : relais DHCPv6 ;
- besoin de traçabilité : évaluer les capacités du système d'exploitation et de la plateforme, pas seulement les bits RA.

## Pratique TianSemi

1. Capturez RS puis RA sur un segment SLAAC.
2. Relevez préfixe, route par défaut et bits M/O.
3. Vérifiez DAD avant que l'adresse ne devienne utilisable.
4. Passez à `M=0, O=1` et confirmez que l'adresse reste issue de SLAAC.
5. Comparez ensuite un échange DHCPv6 à quatre messages et un Rapid Commit.

## Troubleshooting

| Symptôme | Contrôle |
| --- | --- |
| Link-local présente, pas de globale | RA absent ou préfixe non autonome |
| Globale présente, pas de DNS | Bit O et service DHCPv6 stateless |
| DHCPv6 stateful non utilisé | Bit M, client compatible et serveur joignable |
| Client distant sans réponse | Relais et routage vers le serveur |
| Adresse générée mais inutilisable | DAD, durée de vie du préfixe et route par défaut |

## À retenir pour l'examen HCIA-Datacom

- SLAAC repose sur NDP et RA.
- M sélectionne la gestion d'adresse par DHCPv6 ; O demande d'autres informations.
- SLAAC exécute DAD sur link-local puis sur l'adresse globale.
- DHCPv6 stateful attribue l'adresse ; stateless complète SLAAC.
- Le modèle à quatre messages est Solicit, Advertise, Request, Reply.
- Un relais dessert les clients d'autres segments.

## Révision TianSemi

1. Quelle différence existe entre SLAAC, DHCPv6 stateful et DHCPv6 stateless ?
2. Que signifient les bits M et O d'un RA ?
3. Quelles sont les étapes complètes de SLAAC ?
4. Quels sont les quatre messages de l'attribution DHCPv6 standard ?
5. Dans quel cas l'échange DHCPv6 peut-il se réduire à deux messages ?
6. Pourquoi un relais DHCPv6 est-il nécessaire entre segments ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 542 à 550.
