---
id: HUA-HCIA-DC-033
title: "Direct Routes"
official_title: "Direct Routes"
course_unit: "3.2.2"
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
source_pages: "PDF 414-426"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 72-78"
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 82-86"
tags: [huawei, hcia, datacom, direct-route, vlanif, dot1q-termination, inter-vlan]
prerequisites:
  - "[[Overview of IP Routing]]"
related:
  - "[[Static Routes]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Routes directes (Direct Routes)

## Génération d'une route directe

Une route directe est créée automatiquement lorsqu'une adresse IP est configurée sur une interface de couche 3 et que ses états **Physical** et **Protocol** sont tous deux `Up`.

```text
<R1> display ip interface brief
<R1> display ip routing-table protocol direct
```

Si l'interface tombe, la route associée disparaît de la table. Elle ne nécessite donc aucune commande `ip route-static`.

Sur le dernier routeur du chemin, la route directe indique que la destination se trouve sur un segment local. Le routeur résout alors l'adresse MAC de l'hôte par ARP et lui transmet directement la trame.

## Routes créées pour une interface

Le lab montre qu'après l'adressage d'une interface, la table contient notamment :

- la route du segment, par exemple `10.0.12.0/24` ;
- une route hôte /32 vers l'adresse de l'interface ;
- une route hôte /32 vers le broadcast dirigé du segment.

Une loopback configurée produit également une route directe /32. Comme elle reste généralement `Up` tant qu'elle n'est pas arrêtée administrativement, elle convient à une adresse de gestion ou à un Router ID.

## Application : communication inter-VLAN

![Trois façons d'utiliser des routes directes entre VLAN](<../../../assets/diagrammes/03 IP Technology Basics/direct-routes-inter-vlan-fr.svg>)

Huawei présente trois solutions :

| Solution | Principe | Limite ou avantage |
| --- | --- | --- |
| Interfaces physiques du routeur | Une interface de couche 3 par VLAN | Simple mais peu évolutif |
| Sous-interfaces dot1q | Plusieurs passerelles logiques sur une liaison trunk | Économise les interfaces physiques |
| Interfaces VLANIF | Routage interne sur un switch de couche 3 | Adapté à la centralisation des passerelles |

### Sous-interfaces dot1q

Le port du switch vers le routeur transporte les VLAN en trunk. La sous-interface retire le tag reçu pour le traitement de couche 3 et ajoute le tag lors de l'émission.

```text
[R1] interface GE 0/0/1.10
[R1-GE0/0/1.10] dot1q termination vid 10
[R1-GE0/0/1.10] ip address 192.168.10.254 24
```

Par défaut, une sous-interface de couche 3 n'accepte pas les trames taguées sans configuration de terminaison dot1q.

### Interfaces VLANIF

```text
[SW1] vlan batch 10 20
[SW1] interface Vlanif 10
[SW1-Vlanif10] ip address 192.168.10.254 24
[SW1] interface Vlanif 20
[SW1-Vlanif20] ip address 192.168.20.254 24
```

L'adresse VLANIF sert de passerelle aux hôtes du VLAN correspondant. La VLANIF passe `Up` lorsqu'au moins une interface physique autorisant ce VLAN est opérationnelle.

## Mini-lab TianSemi

1. Créez les VLAN 10 et 20 et placez un terminal dans chacun.
2. Testez d'abord l'isolation de couche 2.
3. Implémentez une solution par sous-interfaces dot1q et vérifiez `ping` puis `tracert`.
4. Supprimez cette solution et créez deux VLANIF sur le switch de couche 3.
5. Comparez les routes directes avant et après l'adressage.
6. Arrêtez le dernier port actif d'un VLAN et observez l'état de la VLANIF et de sa route.

## Troubleshooting

| Symptôme | Contrôle prioritaire |
| --- | --- |
| Route directe absente | États Physical/Protocol et adresse/masque |
| Sous-interface ne reçoit rien | Trunk, VLAN autorisé et `dot1q termination vid` |
| VLANIF Down | VLAN créé et au moins un port de ce VLAN Up |
| Un VLAN fonctionne, l'autre non | Adressage de la passerelle et association VLAN/port |
| Dernier saut inaccessible | ARP vers l'hôte et masque du segment |

## À retenir pour l'examen HCIA-Datacom

- Une route directe dépend d'une interface de couche 3 Physical/Protocol Up.
- Une route directe a une préférence par défaut de 0.
- Le dernier saut utilise ARP pour joindre l'hôte.
- Les sous-interfaces nécessitent la terminaison dot1q.
- Une VLANIF correspond à un VLAN et sert de passerelle de couche 3.
- Interfaces physiques, sous-interfaces et VLANIF peuvent assurer l'inter-VLAN.

## Révision TianSemi

1. Quelles conditions créent et maintiennent une route directe ?
2. Quelles routes la configuration d'une adresse d'interface génère-t-elle dans le lab ?
3. Pourquoi une route directe disparaît-elle lorsque l'interface tombe ?
4. Quel est le rôle de `dot1q termination vid` ?
5. Quand une interface VLANIF passe-t-elle Up ?
6. Quelle solution inter-VLAN utilise le moins d'interfaces physiques de routeur ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 414 à 426.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 72 à 78 pour l'inter-VLAN et pages PDF 82 à 86 pour la génération des routes directes.
