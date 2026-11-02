---
id: HUA-HCIA-DC-039
title: "Overview of IPv6"
official_title: "Overview of IPv6"
course_unit: "3.4.1"
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
source_pages: "PDF 496-527"
tags: [huawei, hcia, datacom, ipv6, gua, ula, link-local, multicast, anycast, extension-header]
prerequisites:
  - "[[Introduction to IPv4 Addresses]]"
related:
  - "[[ICMPv6]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble d'IPv6 (Overview of IPv6)

## Pourquoi IPv6 ?

L'épuisement de l'espace IPv4 a conduit à des mécanismes de prolongation comme CIDR et NAT, sans supprimer la limite fondamentale des 32 bits. IPv6 utilise 128 bits et apporte aussi une structure hiérarchique, SLAAC, un en-tête fixe extensible et le champ Flow Label.

IPv6 restaure plus facilement une communication de bout en bout sans rendre la sécurité automatique : IPsec est supporté, mais une politique de sécurité reste nécessaire.

## Écriture d'une adresse IPv6

Une adresse comporte huit groupes de 16 bits en hexadécimal :

```text
2001:0DB8:0000:0000:0008:0800:200C:417A
```

Règles d'abréviation :

- supprimer les zéros de tête d'un groupe ;
- remplacer une seule séquence de groupes entièrement nuls par `::` ;
- ne jamais utiliser `::` deux fois dans une même adresse ;
- ajouter `/longueur` pour le préfixe.

Exemple : `2001:0DB8:0000:0000:FB00:1400:5000:45FF` devient `2001:DB8::FB00:1400:5000:45FF`.

## Types d'adresses

![Principaux types et portées d'adresses IPv6](<../../../assets/diagrammes/03 IP Technology Basics/ipv6-address-types-fr.svg>)

| Type | Préfixe ou exemple | Usage |
| --- | --- | --- |
| Unspecified | `::/128` | Source avant attribution d'une adresse |
| Loopback | `::1/128` | Test local |
| Link-local | `FE80::/10` | Communication et découverte sur le lien |
| Global unicast | `2000::/3` | Adresse globalement routable |
| Unique local | `FC00::/7`, avec `FD00::/8` utilisé | Réseau interne non routé publiquement |
| Multicast | `FF00::/8` | Un groupe d'interfaces |
| Anycast | Format unicast | Interface la plus proche selon le routage |

IPv6 ne définit pas de broadcast. Le multicast remplit les fonctions de diffusion ciblée.

Pour les adresses unicast courantes présentées, le préfixe et l'Interface ID mesurent généralement 64 bits chacun. L'Interface ID peut être manuel, aléatoire ou dérivé d'une MAC selon EUI-64. EUI-64 insère `FFFE` au milieu de la MAC et inverse le bit U/L ; il peut toutefois révéler des informations sur la MAC.

## Multicast sollicité

Une adresse unicast ou anycast entraîne l'adhésion au groupe **solicited-node** correspondant. Le préfixe fixe est `FF02::1:FF00:0/104`, complété par les 24 derniers bits de l'adresse cible. NDP l'utilise pour limiter une sollicitation aux nœuds concernés, sans broadcast.

Sur Ethernet, une adresse multicast IPv6 se mappe vers une MAC commençant par `33:33`, suivie des 32 derniers bits de l'adresse IPv6 multicast.

## En-tête et extensions

![En-tête IPv6 fixe et chaîne des extensions](<../../../assets/diagrammes/03 IP Technology Basics/ipv6-basic-header-fr.svg>)

L'en-tête de base mesure toujours 40 octets et contient huit champs : Version, Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, Source Address et Destination Address.

Différences essentielles avec IPv4 :

- pas de checksum de couche 3 ;
- pas de fragmentation par les routeurs intermédiaires ;
- options déplacées vers des en-têtes d'extension ;
- Flow Label ajouté ;
- adresses source et destination de 128 bits.

`Next Header` désigne soit le premier en-tête d'extension, soit le protocole supérieur. Chaque extension pointe à son tour vers la suivante. La plupart sont traitées par la destination ; Hop-by-Hop Options fait exception et doit être examiné sur le chemin.

## Pratique TianSemi

1. Développez `2001:DB8::32A:0:0:2D70` sur huit groupes.
2. Compressez `FE80:0000:0000:0000:0212:34FF:FE56:789A`.
3. Classez `::`, `::1`, `FE80::1`, `FD12::1`, `2001:DB8::1` et `FF02::1`.
4. Pour une adresse finissant par `ABCD:EF12`, déduisez les 24 derniers bits de son groupe solicited-node.
5. Comparez les champs TTL/Protocol IPv4 aux champs Hop Limit/Next Header IPv6.

## À retenir pour l'examen HCIA-Datacom

- IPv6 mesure 128 bits et utilise l'hexadécimal séparé par des deux-points.
- `::` ne peut apparaître qu'une fois dans une adresse abrégée.
- IPv6 possède unicast, multicast et anycast, mais pas broadcast.
- Link-local vaut `FE80::/10`, multicast `FF00::/8`, global unicast `2000::/3`.
- L'en-tête de base mesure 40 octets.
- Les routeurs intermédiaires ne fragmentent pas les paquets IPv6.
- Next Header chaîne extensions et protocole supérieur.

## Révision TianSemi

1. Quelles limites d'IPv4 motivent le passage à IPv6 ?
2. Quelles règles permettent de compresser une adresse IPv6 ?
3. Quelles sont les portées de `FE80::/10`, `FD00::/8` et `2000::/3` ?
4. Pourquoi IPv6 n'a-t-il pas besoin de broadcast pour NDP ?
5. Comment une MAC peut-elle produire un Interface ID EUI-64 ?
6. Quels sont les huit champs de l'en-tête de base IPv6 ?
7. Quelles fonctions IPv4 ont été supprimées ou déplacées dans IPv6 ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 496 à 527.
