---
id: HUA-HCIA-DC-027
title: "Network Layer Protocols"
official_title: "Network Layer Protocols"
course_unit: "3.1.1"
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
source_pages: "PDF 335-345"
tags: [huawei, hcia, datacom, ipv4, network-layer, ttl, fragmentation, ip-header]
prerequisites:
  - "[[Network Reference Models and Standard Protocols]]"
related:
  - "[[Introduction to IPv4 Addresses]]"
  - "[[ICMP]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Protocoles de la couche réseau (Network Layer Protocols)

## Rôle de la couche réseau

La couche réseau est souvent appelée couche IP, mais elle ne se limite pas à IP. Huawei y situe notamment :

- **IP**, pour l'adressage logique, le routage et le transfert des paquets ;
- **ICMP**, pour les informations de contrôle, les erreurs et le diagnostic ;
- **IGMP**, pour la gestion des groupes multicast, étudié plus loin dans le parcours HCIP.

IP existe en deux versions : IPv4 et IPv6. Dans cette unité, « IP » désigne IPv4 sauf indication contraire.

## En-tête IPv4

![Principaux champs de l'en-tête IPv4 et leur rôle](<../../../assets/diagrammes/03 IP Technology Basics/ipv4-header-key-fields-fr.svg>)

L'en-tête IPv4 fixe mesure 20 octets. Les options peuvent ajouter jusqu'à 40 octets, pour un en-tête total maximal de 60 octets.

| Champ | Taille | Fonction essentielle |
| --- | ---: | --- |
| Version | 4 bits | Vaut 4 pour IPv4 |
| Header Length | 4 bits | Longueur de l'en-tête |
| Type of Service | 8 bits | Type de service demandé |
| Total Length | 16 bits | Taille totale du paquet |
| Identification | 16 bits | Associe les fragments d'un même paquet |
| Flags | 3 bits | Contrôle notamment la fragmentation |
| Fragment Offset | 13 bits | Position du fragment dans le paquet original |
| TTL | 8 bits | Limite le nombre de routeurs traversés |
| Protocol | 8 bits | Identifie le protocole qui traite la charge utile |
| Header Checksum | 16 bits | Vérifie l'en-tête |
| Source/Destination | 32 bits chacun | Adresses IPv4 des extrémités |
| Options et Padding | 0 à 40 octets | Options et alignement par remplissage à zéro |

## Fragmentation

Si un paquet est plus grand que ce qu'un lien peut transporter, il peut être découpé en fragments.

- **Identification** reste commun aux fragments du paquet initial ;
- **DF** (*Don't Fragment*) à 1 interdit la fragmentation ;
- **MF** (*More Fragments*) à 1 indique qu'un autre fragment suit ;
- **Fragment Offset** localise les données du fragment dans le paquet initial.

La fragmentation ajoute du traitement et la perte d'un fragment empêche la reconstitution complète. Il est donc préférable que les tailles de paquets respectent la MTU du chemin.

## TTL

Le TTL diminue de 1 à chaque passage par un routeur. Lorsqu'il atteint 0, le routeur supprime le paquet et peut envoyer une erreur ICMP à la source.

Ce champ évite qu'un paquet reste indéfiniment dans une boucle de routage. Il représente aujourd'hui une limite de sauts, même si son nom historique signifie *Time to Live*.

## Champ Protocol

Le destinataire utilise ce champ pour remettre la charge utile au bon protocole :

| Valeur décimale | Protocole |
| ---: | --- |
| 1 | ICMP |
| 2 | IGMP |
| 6 | TCP |
| 17 | UDP |

## Pratique TianSemi : lire un en-tête

Un paquet arrive avec `TTL=1`, `Protocol=6`, `DF=1` et une taille supérieure à la MTU du prochain lien.

1. `Protocol=6` indique TCP.
2. Le routeur décrémente le TTL à 0 et supprime le paquet.
3. Même sans ce problème, `DF=1` interdirait sa fragmentation.
4. Le diagnostic peut recevoir une erreur ICMP selon la configuration du routeur.

## Troubleshooting

| Symptôme | Champ ou mécanisme à examiner |
| --- | --- |
| Boucle ou chemin anormal | TTL et résultats de `tracert` |
| Gros paquets perdus | MTU, Total Length et drapeau DF |
| Fragments impossibles à réassembler | Identification, MF et Fragment Offset |
| Charge remise au mauvais traitement | Champ Protocol |
| En-tête altéré | Header Checksum |

## À retenir pour l'examen HCIA-Datacom

- IP fournit adressage logique et transfert inter-réseaux.
- L'en-tête IPv4 fixe mesure 20 octets, jusqu'à 60 avec options.
- Identification, Flags et Fragment Offset servent à la fragmentation.
- TTL diminue à chaque routeur et évite les boucles infinies.
- Protocol vaut notamment 1 pour ICMP, 6 pour TCP et 17 pour UDP.

## Révision TianSemi

1. Quels protocoles de couche réseau Huawei cite-t-il avec IP ?
2. Quelle est la taille minimale et maximale d'un en-tête IPv4 ?
3. Quels champs permettent de fragmenter et réassembler un paquet ?
4. Quel effet produit le drapeau DF ?
5. Que fait un routeur lorsque le TTL atteint 0 ?
6. À quels protocoles correspondent les valeurs 1, 6 et 17 ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 335 à 345.
