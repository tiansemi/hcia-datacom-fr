---
id: HUA-HCIA-DC-028
title: "Introduction to IPv4 Addresses"
official_title: "Introduction to IPv4 Addresses"
course_unit: "3.1.2"
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
source_pages: "PDF 346-360"
tags: [huawei, hcia, datacom, ipv4-address, subnet-mask, private-address, gateway]
prerequisites:
  - "[[Network Layer Protocols]]"
related:
  - "[[Subnetting]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Introduction aux adresses IPv4 (Introduction to IPv4 Addresses)

## Format et fonction

Une adresse IPv4 identifie une interface sur un réseau IP. Elle contient 32 bits, regroupés en quatre octets et écrits en notation décimale pointée, par exemple `192.168.10.1`.

La plage complète va de `0.0.0.0` à `255.255.255.255`, mais toutes les valeurs ne sont pas assignables à une interface.

## Partie réseau, partie hôte et masque

![Une adresse IPv4 divisée en partie réseau et partie hôte par son masque](<../../../assets/diagrammes/03 IP Technology Basics/ipv4-network-host-mask-fr.svg>)

- la **partie réseau** identifie le segment ;
- la **partie hôte** identifie l'interface dans ce segment ;
- le **masque** contient des 1 consécutifs pour la partie réseau puis des 0 pour la partie hôte.

`192.168.10.1 255.255.255.0` s'écrit aussi `192.168.10.1/24`.

Si la destination appartient à un autre réseau, l'hôte remet le paquet à sa passerelle. L'adresse de l'interface de passerelle située sur le segment local constitue la gateway configurée sur l'hôte.

## Classification historique

| Classe | Plage | Masque historique | Usage présenté |
| --- | --- | --- | --- |
| A | `0.0.0.0`–`127.255.255.255` | /8 | Unicast hôte, sauf adresses spéciales |
| B | `128.0.0.0`–`191.255.255.255` | /16 | Unicast hôte |
| C | `192.0.0.0`–`223.255.255.255` | /24 | Unicast hôte |
| D | `224.0.0.0`–`239.255.255.255` | — | Multicast |
| E | `240.0.0.0`–`255.255.255.255` | — | Recherche/expérimentation |

Cette classification explique des notions d'examen, mais la planification moderne s'appuie sur des longueurs de préfixe et le subnetting.

## Adresse réseau, broadcast et hôtes

Pour un préfixe comportant `n` bits hôte :

- tous les bits hôte à 0 donnent l'adresse réseau ;
- tous les bits hôte à 1 donnent l'adresse de broadcast ;
- les autres valeurs sont assignables ;
- le nombre d'hôtes utilisables présenté est `2^n - 2`.

Exemple `192.168.10.0/24` : réseau `.0`, broadcast `.255`, hôtes `.1` à `.254`.

## Adresses privées et publiques

| Plage privée | Préfixe |
| --- | --- |
| `10.0.0.0`–`10.255.255.255` | `10.0.0.0/8` |
| `172.16.0.0`–`172.31.255.255` | `172.16.0.0/12` |
| `192.168.0.0`–`192.168.255.255` | `192.168.0.0/16` |

Les adresses privées peuvent être réutilisées dans des réseaux internes. Elles ne sont pas routées directement sur Internet ; NAT traduit leurs communications vers une adresse publique.

## Adresses spéciales

| Adresse | Fonction |
| --- | --- |
| `255.255.255.255` | Broadcast limité au segment local |
| `0.0.0.0` | Source non configurée ; `0.0.0.0/0` représente la route par défaut |
| `127.0.0.0/8` | Loopback, test du système local |
| `169.254.0.0/16` | Adresse link-local temporaire si l'obtention automatique échoue |

Un paquet destiné à une adresse loopback ne quitte pas l'équipement.

## Pratique TianSemi : calcul élémentaire

Pour `172.16.10.1/16` :

- réseau : `172.16.0.0` ;
- broadcast : `172.16.255.255` ;
- plage utilisable : `172.16.0.1` à `172.16.255.254` ;
- hôtes utilisables : `2^16 - 2 = 65534`.

## À retenir pour l'examen HCIA-Datacom

- IPv4 mesure 32 bits et s'écrit sur quatre octets.
- Le masque sépare réseau et hôte.
- Réseau = bits hôte à 0 ; broadcast = bits hôte à 1.
- Les plages privées sont 10/8, 172.16/12 et 192.168/16.
- `127.0.0.0/8` est loopback ; `169.254.0.0/16` est link-local.
- Une passerelle transfère vers les autres segments.

## Révision TianSemi

1. Que représente une adresse IPv4 sur un équipement ?
2. Comment le masque distingue-t-il partie réseau et partie hôte ?
3. Quelles classes historiques sont assignables aux hôtes ?
4. Comment calculer réseau, broadcast et nombre d'hôtes ?
5. Quelles sont les trois plages privées ?
6. Quelle différence existe entre `0.0.0.0`, `127.0.0.0/8` et `169.254.0.0/16` ?
7. Quel est le rôle d'une passerelle ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 346 à 360.
