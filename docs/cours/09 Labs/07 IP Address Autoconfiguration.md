---
id: HUA-HCIA-LAB-007
title: "IP Address Autoconfiguration"
official_title: "IP Address Autoconfiguration"
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
source_pages: "PDF 95-107 (guide pages 93-105)"
lab_required: true
tags: [huawei, hcia, datacom, lab, dhcp, address-pool, static-bind, relay]
devices: [S1, R1, S2]
commands: [dhcp enable, ip pool, dhcp select global, ip address dhcp-alloc, dhcp select relay, dhcp relay server-ip]
publication_status: "not-started"
site_url:
---

# Lab 07 — Autoconfiguration d’adresses IPv4 avec DHCP

## Objectif

Configurer S2 comme serveur DHCP, attribuer dynamiquement une adresse à R1, réserver une adresse à S1 selon sa MAC, puis placer R1 comme relais entre le client S1 et le serveur distant.

## Topologie et rôles

![Chaîne DHCP entre S1, R1 et S2](<../../assets/diagrammes/09 Labs/lab07-dhcp-topology-fr.svg>)

- S1 `GE1/0/1` : client DHCP sur `192.168.1.0/24`.
- R1 `GE0/0/1` : relais et passerelle `192.168.1.254/24`.
- R1 `GE0/0/2` : client DHCP sur le réseau du serveur.
- S2 `GE1/0/1` : serveur DHCP `10.0.12.254/24`.

## 1 — Activer DHCP et créer les pools

Après l’adressage de S2 et le passage des interfaces utiles en couche 3 :

```text
[S2] dhcp enable
[S1] dhcp enable
[R1] dhcp enable

[S2] ip pool for_r1
[S2-ip-pool-for_r1] network 10.0.12.0 mask 24
[S2-ip-pool-for_r1] gateway-list 10.0.12.254
[S2-ip-pool-for_r1] dns-list 10.0.12.254
[S2-ip-pool-for_r1] lease day 2 hour 2
```

Pour S1, relevez d’abord la MAC réelle de `GE1/0/1`, puis créez la réservation :

```text
[S1] display interface GE 1/0/1
[S2] ip pool for_s1
[S2-ip-pool-for_s1] network 192.168.1.0 mask 24
[S2-ip-pool-for_s1] gateway-list 192.168.1.254
[S2-ip-pool-for_s1] static-bind ip-address 192.168.1.253 mac-address <MAC-S1>
```

## 2 — Servir le réseau directement connecté

```text
[S2] interface GE 1/0/1
[S2-GE1/0/1] dhcp select global
[R1-GE0/0/2] ip address dhcp-alloc
```

Vérifiez l’adresse, le bail, la route par défaut et l’utilisation du pool :

```text
[R1] display ip interface GE 0/0/2
[R1] display dhcp client interface GE 0/0/2
[R1] display ip routing-table
[S2] display ip pool name for_r1 used
```

## 3 — Relayer les requêtes de S1

```text
[R1] interface GE 0/0/1
[R1-GE0/0/1] ip address 192.168.1.254 24
[R1-GE0/0/1] dhcp select relay
[R1-GE0/0/1] dhcp relay server-ip 10.0.12.254

[S1-GE1/0/1] ip address dhcp-alloc
```

Le serveur doit posséder une route de retour vers le réseau du client. Utilisez comme next-hop l’adresse réellement obtenue par R1 sur `GE0/0/2` :

```text
[S2] ip route-static 192.168.1.0 24 <IP-DHCP-DE-R1>
```

Ne recopiez pas l’exemple `10.0.12.105` si votre pool a attribué une autre adresse.

## Vérifications attendues

```text
[S1] display ip interface GE 1/0/1
[S1] display ip routing-table
[S2] display ip pool name for_s1 used
[S1] ping 10.0.12.254
```

- R1 obtient une adresse dynamique de `10.0.12.0/24`.
- S1 obtient exactement `192.168.1.253` grâce à la liaison MAC/IP.
- S1 reçoit la passerelle `192.168.1.254`.
- Le ping S1–S2 réussit à travers le relais et la route de retour.

## Troubleshooting

| Symptôme | Vérification | Correction |
| --- | --- | --- |
| Aucun bail | DHCP global et service d’interface | `dhcp enable`, puis `dhcp select global` ou `relay`. |
| S1 reçoit une adresse différente | MAC de réservation | Relever la MAC réelle et corriger `static-bind`. |
| Requête relayée sans réponse | Route retour sur S2 | Ajouter la route vers `192.168.1.0/24` via l’IP réelle de R1. |
| Passerelle absente | Paramètre du pool | Vérifier `gateway-list`. |
| Pool inattendu choisi | Réseau de l’interface de réception/relais | Contrôler les préfixes des pools et l’adresse du relais. |

## Révision TianSemi

1. Quelle différence existe entre allocation dynamique et réservation DHCP ?
2. Pourquoi `dhcp enable` ne suffit-il pas à servir une interface ?
3. Comment le serveur choisit-il le pool applicable ?
4. Pourquoi le relais est-il nécessaire pour S1 ?
5. Quelle route doit exister sur S2 et pourquoi ?
6. Pourquoi la MAC et l’adresse obtenue par R1 doivent-elles être relevées dans votre propre lab ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 95 à 107 (pages 93 à 105 du guide).
