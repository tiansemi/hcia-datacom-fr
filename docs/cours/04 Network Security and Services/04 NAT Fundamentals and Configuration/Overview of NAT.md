---
id: HUA-HCIA-DC-053
title: "Overview of NAT"
official_title: "Overview of NAT"
course_unit: "4.4.1"
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
source_pages: "PDF 692-701"
tags: [huawei, hcia, datacom, nat, private-ip, public-ip, address-translation]
prerequisites:
  - "[[SSH]]"
related:
  - "[[Source NAT]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble de NAT (Overview of NAT)

## Pourquoi NAT existe

Les adresses IPv4 publiques sont limitées, alors que les réseaux privés réutilisent les mêmes blocs d'adresses :

- `10.0.0.0/8` ;
- `172.16.0.0/12` ;
- `192.168.0.0/16`.

Ces adresses ne sont pas routées directement sur Internet. La **Network Address Translation** modifie les adresses IP des paquets à la frontière entre réseau privé et réseau public afin de permettre les communications.

## Traduction à l'aller et au retour

![NAT traduit l'adresse privée à l'aller puis restaure la destination au retour](<../../../assets/diagrammes/04 Network Security and Services/nat-round-trip-fr.svg>)

Lorsqu'un poste privé initie une connexion :

1. le routeur de sortie remplace l'adresse source privée par une adresse publique ;
2. le serveur Internet répond vers cette adresse publique ;
3. le routeur retrouve l'association créée ;
4. il restaure l'adresse privée comme destination du paquet de retour.

NAT est généralement déployé sur un routeur ou un pare-feu d'egress.

## Bénéfices et limites

### Bénéfices

- réutilisation des adresses privées ;
- partage d'un petit nombre d'adresses publiques ;
- communication entre réseaux privés et publics ;
- masquage de l'adressage interne ;
- conception interne indépendante de l'adresse publique exposée.

### Limites

- les extrémités ne voient plus leurs adresses réelles respectives ;
- les journaux nécessitent une corrélation avec les associations NAT ;
- la traduction consomme des ressources et ajoute du traitement ;
- certains protocoles portant des adresses dans leurs données exigent un ALG ;
- NAT ne remplace ni un pare-feu ni une politique de sécurité.

## Classification

| Type | Champ traduit | Traduction de port |
| --- | --- | --- |
| NAT No-PAT | Source | Non |
| NAPT | Source | Oui |
| Easy IP | Source | Oui |
| NAT Server | Destination | Facultative selon la règle |
| NAT bidirectionnelle | Source et destination | Facultative selon la combinaison |

## Ordre de traitement Huawei

Pour un paquet reçu, le mécanisme présenté suit cette logique :

1. si NAT est activé sur l'interface entrante, rechercher une association de **destination NAT** ;
2. traduire la destination en cas de correspondance ;
3. rechercher une route vers la destination obtenue ;
4. si NAT est activé sur l'interface sortante, rechercher une politique de **source NAT** ;
5. traduire la source en cas de correspondance ;
6. transmettre le paquet, ou le supprimer si aucune route n'existe.

La présence d'une configuration NAT globale ne suffit pas : la fonction doit être activée sur l'interface traversée concernée.

## Pratique TianSemi : raisonner sur un paquet

Pour le flux `192.168.1.10:49152 → 200.1.2.3:443` traduit en `122.1.2.1:2048` :

1. écrivez le cinq-tuple avant et après NAT ;
2. écrivez le paquet de réponse reçu sur l'interface publique ;
3. identifiez les champs restaurés avant livraison au poste ;
4. expliquez quelle table permet la traduction inverse.

## À retenir pour l'examen HCIA-Datacom

- NAT répond principalement à la pénurie d'adresses IPv4 publiques.
- Les adresses privées ne sont pas routées directement sur Internet.
- Source NAT permet aux hôtes internes de sortir ; destination NAT publie un service interne.
- NAPT et Easy IP traduisent également les ports.
- NAT bidirectionnelle combine source NAT et destination NAT.
- L'ordre de traitement examine la destination avant le routage, puis la source sur l'interface sortante.

## Révision TianSemi

1. Pourquoi un hôte utilisant une adresse privée a-t-il besoin de NAT pour accéder à Internet ?
2. Quels sont les trois blocs IPv4 privés ?
3. Quelle différence existe entre source NAT et destination NAT ?
4. Pourquoi les journaux NAT sont-ils importants pour le dépannage ?
5. À quel moment la route est-elle recherchée dans le flux de traitement Huawei ?
6. Pourquoi NAT ne doit-il pas être confondu avec un pare-feu ?
7. Quels types de NAT traduisent les ports ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 692 à 701.
