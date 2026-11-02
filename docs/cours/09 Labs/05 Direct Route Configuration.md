---
id: HUA-HCIA-LAB-005
title: "Direct Route Configuration"
official_title: "Direct Route Configuration"
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
source_pages: "PDF 72-78 (guide pages 70-76)"
lab_required: true
tags: [huawei, hcia, datacom, lab, direct-route, vlanif, dot1q, inter-vlan]
devices: [R1, S1, S3, S4]
commands: [interface Vlanif, dot1q termination vid, ip route-static, display ip routing-table, tracert]
publication_status: "not-started"
site_url:
---

# Lab 05 — Routes directes et communication inter-VLAN

## Objectif

Faire communiquer les VLAN 2 et 3 par deux méthodes, puis comparer leur chemin de traitement : sous-interfaces dot1q sur R1 et interfaces VLANIF sur S1. Dans les deux cas, l’adresse de passerelle crée une route directe lorsque l’interface de couche 3 est opérationnelle.

## Topologie et adressage

![S3 et S4 reliés à S1, lui-même relié à R1](<../../assets/diagrammes/09 Labs/lab05-direct-route-topology-fr.svg>)

| Terminal simulé | Adresse | Passerelle | Port sur S1 | VLAN |
| --- | --- | --- | --- | --- |
| S3 `GE1/0/1` | `192.168.2.1/24` | `192.168.2.254` | `GE1/0/13` | 2 |
| S4 `GE1/0/1` | `192.168.3.1/24` | `192.168.3.254` | `GE1/0/14` | 3 |

## 1 — Préparer les terminaux et les VLAN

```text
[S3-GE1/0/1] undo portswitch
[S3-GE1/0/1] ip address 192.168.2.1 24
[S3] ip route-static 0.0.0.0 0 192.168.2.254
[S4-GE1/0/1] undo portswitch
[S4-GE1/0/1] ip address 192.168.3.1 24
[S4] ip route-static 0.0.0.0 0 192.168.3.254

[S1] vlan batch 2 3
[S1-GE1/0/13] port link-type access
[S1-GE1/0/13] port default vlan 2
[S1-GE1/0/14] port link-type access
[S1-GE1/0/14] port default vlan 3
```

Avant l’ajout des passerelles, S3 et S4 ne doivent pas communiquer : les VLAN isolent leurs domaines de broadcast.

## 2 — Méthode A : sous-interfaces dot1q sur R1

Le lien S1–R1 transporte les deux VLAN :

```text
[S1-GE1/0/1] port link-type trunk
[S1-GE1/0/1] port trunk allow-pass vlan 2 3

[R1-GE0/0/1] undo portswitch
[R1] interface GE 0/0/1.2
[R1-GE0/0/1.2] dot1q termination vid 2
[R1-GE0/0/1.2] ip address 192.168.2.254 24
[R1] interface GE 0/0/1.3
[R1-GE0/0/1.3] dot1q termination vid 3
[R1-GE0/0/1.3] ip address 192.168.3.254 24
```

Chaque sous-interface termine le tag de son VLAN et sert de passerelle. Vérifiez depuis S3 :

```text
<S3> ping 192.168.3.1
<S3> tracert 192.168.3.1
```

Le premier saut doit être `192.168.2.254`, puis S4.

## 3 — Méthode B : interfaces VLANIF sur S1

Supprimez proprement les sous-interfaces et le transport devenu inutile vers R1, puis créez les passerelles sur le commutateur :

```text
[R1] undo interface GE 0/0/1.2
[R1] undo interface GE 0/0/1.3
[S1-GE1/0/1] undo port trunk allow-pass vlan 2 3

[S1] interface Vlanif 2
[S1-Vlanif2] ip address 192.168.2.254 24
[S1] interface Vlanif 3
[S1-Vlanif3] ip address 192.168.3.254 24
```

Une VLANIF ne peut être créée qu’après le VLAN correspondant. Elle passe Up lorsqu’au moins une interface physique autorisant ce VLAN est Up.

## Vérifications attendues

```text
[S1] display ip interface brief
[S1] display ip routing-table
<S3> ping 192.168.3.1
<S3> tracert 192.168.3.1
```

- Deux routes directes `/24` doivent exister sur l’équipement qui porte les passerelles.
- Le ping inter-VLAN doit réussir dans chacune des deux méthodes.
- Le premier saut du tracert est la passerelle du VLAN source.

## Troubleshooting

| Symptôme | Cause probable | Correction |
| --- | --- | --- |
| Sous-interface sans trafic | Tag dot1q ou trunk absent | Aligner `termination vid` et VLAN autorisés sur S1. |
| VLANIF Down | Aucun port actif dans le VLAN | Vérifier VLAN, port access et état physique. |
| Ping de la passerelle réussi, destination échoue | Passerelle/route retour absente sur le terminal | Contrôler les routes par défaut de S3 et S4. |
| Conflit de passerelle | Ancienne méthode encore active | Retirer les sous-interfaces avant de créer les VLANIF avec les mêmes adresses. |

## Révision TianSemi

1. Quand une route directe est-elle générée ?
2. Quel est le rôle de `dot1q termination vid` ?
3. Pourquoi le lien S1–R1 doit-il être trunk dans la première méthode ?
4. Dans quelles conditions une VLANIF passe-t-elle Up ?
5. Quelle différence de placement existe entre les deux méthodes ?
6. Pourquoi les terminaux ont-ils besoin d’une route par défaut ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 72 à 78 (pages 70 à 76 du guide).
