---
id: HUA-HCIA-DC-031
title: "Fundamentals and Configuration of DHCP"
official_title: "Fundamentals and Configuration of DHCP"
course_unit: "3.1.5"
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
source_pages: "PDF 377-392"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 95-108"
tags: [huawei, hcia, datacom, dhcp, dhcp-relay, address-pool, lease]
prerequisites:
  - "[[Introduction to IPv4 Addresses]]"
  - "[[Subnetting]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Fondamentaux et configuration de DHCP (Fundamentals and Configuration of DHCP)

## Pourquoi DHCP ?

Une configuration statique sur chaque terminal augmente la charge d'administration, favorise les erreurs et immobilise des adresses lorsque des utilisateurs sont absents.

**DHCP** adopte un modèle client/serveur pour attribuer et gérer centralement adresse IP, masque, passerelle, DNS et durée de bail. Il utilise UDP : port 68 côté client et 67 côté serveur.

## Messages DHCPv4

| Message | Rôle |
| --- | --- |
| DISCOVER | Découvrir les serveurs disponibles |
| OFFER | Proposer une adresse et des paramètres |
| REQUEST | Choisir une offre, confirmer ou renouveler |
| ACK | Confirmer l'attribution |
| NAK | Refuser la demande |
| DECLINE | Signaler un conflit d'adresse |
| RELEASE | Libérer l'adresse |
| INFORM | Obtenir d'autres paramètres après acquisition d'une adresse |

## Première attribution : DORA

![Échange DHCP DORA avec extension par un relais](<../../../assets/diagrammes/03 IP Technology Basics/dhcp-dora-relay-fr.svg>)

1. Le client diffuse **DISCOVER** depuis `0.0.0.0` vers `255.255.255.255`.
2. Le serveur répond par **OFFER** avec une adresse disponible.
3. Le client diffuse **REQUEST**, indiquant l'offre et le serveur choisis.
4. Le serveur confirme par **ACK**.

Si plusieurs serveurs répondent, le client choisit généralement la première offre reçue selon le cours.

## DHCP relay

Les broadcasts ne traversent normalement pas les routeurs. Un agent relais reçoit les messages clients dans un domaine de broadcast et les transmet en unicast au serveur distant. Les réponses suivent le chemin inverse.

Le relais permet donc à un serveur central de desservir plusieurs segments, à condition que le routage fonctionne dans les deux sens.

## Renouvellement du bail

- à 50 % du bail, le client envoie un REQUEST unicast au serveur ;
- sans réponse, à 87,5 %, il diffuse un REQUEST ;
- à expiration sans ACK, il cesse d'utiliser l'adresse et recommence par DISCOVER.

Un NAK oblige également le client à demander une nouvelle adresse.

## Configuration Huawei : prérequis et pool global

```text
[HUAWEI] dhcp enable
[HUAWEI] ip pool USERS
[HUAWEI-ip-pool-USERS] network 192.168.10.0 mask 24
[HUAWEI-ip-pool-USERS] gateway-list 192.168.10.254
[HUAWEI-ip-pool-USERS] lease day 1
[HUAWEI-ip-pool-USERS] quit
[HUAWEI] interface GE 0/0/1
[HUAWEI-GE0/0/1] dhcp select global
```

Le bail par défaut présenté est d'un jour. Une liaison statique peut associer une IP à une MAC :

```text
[HUAWEI-ip-pool-USERS] static-bind ip-address 192.168.10.10 mac-address 001e-10dd-dd01
```

## Pool d'interface et client DHCP

```text
[HUAWEI-GE0/0/1] dhcp select interface
[CLIENT-GE0/0/1] ip address dhcp-alloc
```

Le pool d'interface sert les clients du même segment que l'interface. Un pool global peut desservir plusieurs segments, avec un relais lorsque serveur et client sont séparés.

## Configurer un relais

```text
[RELAY-GE0/0/1] dhcp select relay
[RELAY-GE0/0/1] dhcp relay server-ip 10.0.12.254
```

Le serveur doit disposer d'un pool correspondant au segment client et d'une route de retour vers le relais.

## Mini-lab TianSemi

1. Configurez un serveur DHCP avec pool global, passerelle et DNS.
2. Activez `dhcp select global` sur l'interface serveur.
3. Configurez un client avec `ip address dhcp-alloc`.
4. Vérifiez l'adresse, la passerelle et la route par défaut obtenues.
5. Placez ensuite le client sur un autre segment et configurez un relais.
6. Vérifiez le routage de retour et l'allocation à travers le relais.
7. Créez une liaison statique IP–MAC et confirmez l'adresse attribuée.

## Troubleshooting

| Symptôme | Contrôle prioritaire |
| --- | --- |
| Aucun bail | `dhcp enable`, pool valide et fonction serveur sur l'interface |
| Client distant non servi | Relais, adresse serveur et routes aller-retour |
| Mauvaise passerelle | `gateway-list` du pool sélectionné |
| Pool global non choisi | Réseau du pool correspondant au segment client ou à l'adresse relayée |
| Conflit signalé | Message DECLINE, doublon statique ou réservation incorrecte |
| Adresse perdue à terme | Renouvellement à 50/87,5 %, disponibilité du serveur et durée du bail |

## À retenir pour l'examen HCIA-Datacom

- DHCP utilise UDP 67 serveur et 68 client.
- DORA : Discover, Offer, Request, Ack.
- Le relais transporte les échanges entre domaines de broadcast.
- Renouvellement à 50 %, puis 87,5 % en broadcast.
- `dhcp enable` est le prérequis global.
- `dhcp select interface`, `global` et `relay` ont des rôles distincts.

## Révision TianSemi

1. Quels défauts de la configuration statique DHCP corrige-t-il ?
2. Quels ports UDP le client et le serveur utilisent-ils ?
3. Quel est l'ordre DORA ?
4. À quoi servent NAK, DECLINE et RELEASE ?
5. Pourquoi un relay est-il nécessaire entre deux segments ?
6. Que se passe-t-il à 50 % et 87,5 % du bail ?
7. Quelle différence existe entre pool d'interface et pool global ?
8. Quelles routes sont nécessaires dans un scénario relayé ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 377 à 392.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 95 à 108 pour le lab officiel d'autoconfiguration IPv4.
