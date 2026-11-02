---
id: HUA-HCIA-LAB-002
title: "VLAN Configuration"
official_title: "VLAN Configuration"
category: "Huawei HCIA-Datacom Lab"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "intermediate"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-06
updated: 2026-08-11
module: "09 Labs"
type: "lab"
source: "HCIA-Datacom V2.0 Lab Guide"
source_pages: "PDF 27-46 (guide pages 25-44)"
lab_required: true
tags: [huawei, hcia, datacom, lab, vlan, access, trunk, hybrid, mac-vlan]
devices: [R1, S1, S2, S3, S4]
commands: [vlan batch, port link-type, port default vlan, port trunk allow-pass vlan, port hybrid untagged vlan, mac-vlan]
publication_status: "not-started"
site_url:
---

# Lab 02 — Configuration des VLAN

## Objectif

Segmenter un réseau de couche 2 avec les VLAN 3 et 10, configurer des interfaces access, trunk et hybrid, puis associer un terminal mobile à un VLAN selon son adresse MAC. Le lab montre aussi, à partir d’ARP et de la table MAC, pourquoi deux équipements placés dans des VLAN différents ne communiquent pas directement.

## Topologie officielle simplifiée

![Topologie du lab VLAN avec S1, S2, S3, S4 et R1](<../../assets/diagrammes/09 Labs/lab02-vlan-topology-fr.svg>)

| Équipement | Interface | Adresse | VLAN / rôle |
| --- | --- | --- | --- |
| S3 | `GE1/0/1` | `192.168.1.3/24` | Terminal simulé dans le VLAN 3 via S1 `GE1/0/13`. |
| S4 | `GE1/0/2` | `192.168.1.4/24` | Terminal mobile simulé dans le VLAN 3 via S2 `GE1/0/14`. |
| R1 | `GE0/0/2` | `192.168.1.251/24` | PC spécial simulé, VLAN 10 via S2 `GE1/0/1`. |
| S1–S2 | `GE1/0/10` | — | Trunk transportant uniquement les VLAN 3 et 10. |

Les adresses appartiennent volontairement au même préfixe IPv4. La séparation observée provient donc du domaine de broadcast VLAN, pas du masque IP.

## Roadmap

1. Préparer les équipements, adresses et interfaces utiles.
2. Observer la connectivité initiale dans le VLAN 1 par défaut.
3. Créer les VLAN 3 et 10.
4. Affecter les interfaces par port et configurer le trunk S1–S2.
5. Étudier un port hybrid et l’asymétrie ARP obtenue.
6. Associer la MAC de S4 au VLAN 3 sur plusieurs ports mobiles.
7. Vérifier VLAN, MAC, ARP et connectivité attendue.

## 1 — Préparer et mesurer l’état initial

Configurez les noms, arrêtez les interfaces inutilisées comme dans votre câblage, passez les interfaces de S3, S4 et R1 en couche 3 lorsque nécessaire, puis appliquez :

```text
[R1-GE0/0/2] ip address 192.168.1.251 24
[S3-GE1/0/1] ip address 192.168.1.3 24
[S4-GE1/0/2] ip address 192.168.1.4 24
```

Avant toute segmentation, vérifiez que R1 peut joindre S3 et S4. Tous les ports appartiennent alors au VLAN 1 par défaut.

```text
[R1] ping 192.168.1.3
[R1] ping 192.168.1.4
[R1] display arp
[S2] display mac-address
```

Relevez la MAC de S4 : elle sera utilisée pour l’affectation par adresse MAC. Ne recopiez pas la valeur du guide si votre matériel en présente une autre.

## 2 — Créer et transporter les VLAN

```text
[S1] vlan batch 3 10
[S2] vlan batch 3 10

[S1] interface GE 1/0/13
[S1-GE1/0/13] port link-type access
[S1-GE1/0/13] port default vlan 3

[S2] interface GE 1/0/14
[S2-GE1/0/14] port link-type access
[S2-GE1/0/14] port default vlan 3

[S2] interface GE 1/0/1
[S2-GE1/0/1] port link-type access
[S2-GE1/0/1] port default vlan 10
```

Configurez ensuite les deux extrémités du lien S1–S2 de manière symétrique :

```text
[S1] interface GE 1/0/10
[S1-GE1/0/10] port link-type trunk
[S1-GE1/0/10] port trunk allow-pass vlan 3 10
[S1-GE1/0/10] undo port trunk allow-pass vlan 1

[S2] interface GE 1/0/10
[S2-GE1/0/10] port link-type trunk
[S2-GE1/0/10] port trunk allow-pass vlan 3 10
[S2-GE1/0/10] undo port trunk allow-pass vlan 1
```

Résultat attendu : S3 et S4 se joignent dans le VLAN 3 ; R1, dans le VLAN 10, ne les joint plus en l’absence de routage inter-VLAN.

## 3 — Comprendre le port hybrid avec ARP

Le guide transforme S2 `GE1/0/1` en hybrid, PVID 10, avec sortie non taguée pour les VLAN 3 et 10 :

```text
[S2] interface GE 1/0/1
[S2-GE1/0/1] port link-type hybrid
[S2-GE1/0/1] port hybrid pvid vlan 10
[S2-GE1/0/1] port hybrid untagged vlan 3 10
```

Après purge contrôlée des entrées ARP en lab, un test S4 vers R1 produit une situation asymétrique : la requête ARP de S4, classée VLAN 3, peut sortir non taguée vers R1 ; la réponse de R1 entre avec le PVID 10 et ne retourne pas vers S4 dans le VLAN 3.

```text
<S4> reset arp all
<R1> reset arp all
<S4> ping 192.168.1.251
<S4> display arp
<R1> display arp
[S2] display vlan
[S2] display mac-address
```

> [!warning] La purge ARP peut interrompre temporairement des services. Ne l’utilisez que dans ce lab ou dans une fenêtre autorisée.

## 4 — Affecter S4 par adresse MAC

Sur S2, associez la MAC réellement observée de S4 au VLAN 3 :

```text
[S2] vlan 3
[S2-vlan3] mac-vlan mac-address <MAC-DE-S4>

[S2] interface GE 1/0/14
[S2-GE1/0/14] port link-type hybrid
[S2-GE1/0/14] port hybrid untagged vlan 3 10
[S2-GE1/0/14] mac-vlan enable
```

Répétez la configuration hybrid et `mac-vlan enable` sur `GE1/0/15` et `GE1/0/16`. S4 doit conserver son appartenance au VLAN 3 lorsqu’il est déplacé entre ces ports, sans nouvelle configuration du commutateur.

## Vérifications attendues

```text
[S1] display vlan
[S2] display vlan
[S2] display mac-vlan vlan 3
[S1] display mac-address
[S2] display mac-address
<S4> ping 192.168.1.3
<S4> ping 192.168.1.251
<R1> ping 192.168.1.3
```

| Test final | Résultat attendu |
| --- | --- |
| S4 → S3 | Réussite : VLAN 3 de bout en bout. |
| S4 → R1 | Réussite après configuration hybrid/MAC-VLAN prévue par le guide. |
| R1 → S3 | Échec : R1 est VLAN 10, S3 VLAN 3, sans routage ni hybrid adapté côté S1. |
| Déplacement de S4 vers `GE1/0/15` ou `GE1/0/16` | S4 reste classé dans le VLAN 3. |

## Troubleshooting

| Symptôme | Vérification | Correction |
| --- | --- | --- |
| S3 et S4 ne se joignent pas | VLAN 3 présent et autorisé sur les deux extrémités du trunk | Corriger `vlan batch`, PVID access et liste `allow-pass`. |
| Le trafic traverse un VLAN inattendu | `display vlan` et `display mac-address` | Contrôler PVID, sortie tagged/untagged et apprentissage MAC. |
| MAC-VLAN sans effet | `display mac-vlan vlan 3` et interface | Utiliser la MAC réelle, interface hybrid, VLAN autorisé et `mac-vlan enable`. |
| Comportement ARP asymétrique | Entrées ARP des deux extrémités | Suivre le VLAN attribué à la requête puis à la réponse. |

## Révision TianSemi

1. Pourquoi R1, S3 et S4 communiquent-ils avant la création des VLAN ?
2. Quelle différence existe entre port access, trunk et hybrid dans ce lab ?
3. Pourquoi S3 et S4 communiquent-ils alors que R1 et S3 ne le peuvent plus ?
4. Comment le PVID de `GE1/0/1` explique-t-il l’asymétrie ARP ?
5. Quelles conditions doivent être réunies pour que l’affectation MAC-VLAN fonctionne ?
6. Pourquoi faut-il relever la MAC réelle de S4 plutôt que recopier celle du guide ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 27 à 46 (pages 25 à 44 du guide).
