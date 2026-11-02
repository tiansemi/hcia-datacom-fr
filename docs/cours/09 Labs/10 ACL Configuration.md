---
id: HUA-HCIA-LAB-010
title: "ACL Configuration"
official_title: "ACL Configuration"
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
source_pages: "PDF 136-142 (guide pages 134-140)"
lab_required: true
tags: [huawei, hcia, datacom, lab, acl, mqc, traffic-policy, filtering]
devices: [R1, S1, S2]
commands: [acl, rule deny, traffic classifier, traffic behavior, traffic policy, traffic-policy]
publication_status: "not-started"
site_url:
---

# Lab 10 — Configuration d’une ACL et filtrage du trafic

## Objectif et topologie

Filtrer sur R1 les paquets émis par `10.1.1.1` tout en autorisant ceux provenant de `10.0.1.1`, puis prouver le résultat avec deux pings sourcés.

![S1 envoie deux flux à travers R1 vers S2](<../../assets/diagrammes/09 Labs/lab10-acl-topology-fr.svg>)

- S1–R1 : `10.0.12.1/24` ↔ `10.0.12.2/24`.
- R1–S2 : `10.0.23.2/24` ↔ `10.0.23.3/24`.
- S1 LoopBack0 : `10.0.1.1/32`, flux autorisé.
- S1 LoopBack1 : `10.1.1.1/32`, flux refusé.

## Roadmap

1. Adresser les liens et loopbacks.
2. Déployer OSPF aire 0 et valider les deux pings.
3. Créer l’ACL avancée 3000.
4. Lier l’ACL à une policy MQC.
5. Appliquer la policy en sortie de R1 `GE0/0/2`.
6. Répéter les deux tests et lire les compteurs.

## Configuration

Après la connectivité OSPF complète :

```text
[R1] acl 3000
[R1-acl4-advance-3000] rule deny ip source 10.1.1.1 0.0.0.0
[R1-acl4-advance-3000] rule permit ip

[R1] traffic classifier test
[R1-classifier-test] if-match acl 3000
[R1] traffic behavior test
[R1-behavior-test] permit
[R1] traffic policy test
[R1-trafficpolicy-test] classifier test behavior test

[R1] interface GE 0/0/2
[R1-GE0/0/2] traffic-policy test outbound
```

L’ACL classe les paquets ; la policy MQC applique le comportement au trafic correspondant. La direction outbound est choisie car les deux flux quittent R1 vers S2 par `GE0/0/2`.

## Vérifications attendues

```text
[S1] ping -a 10.0.1.1 10.0.23.3
[S1] ping -a 10.1.1.1 10.0.23.3
[R1] display acl 3000
[R1] display traffic policy user-defined test
```

- Le premier ping réussit.
- Le second échoue.
- Les compteurs de la règle deny augmentent uniquement pour le flux `10.1.1.1`.

## Troubleshooting

| Symptôme | Cause | Correction |
| --- | --- | --- |
| Les deux pings passent | Policy absente, mauvaise interface ou direction | Vérifier l’application outbound sur `GE0/0/2`. |
| Les deux échouent | Règle permit absente ou problème de routage | Ajouter l’autorisation explicite et valider OSPF hors policy. |
| Compteur deny nul | Source du test différente | Utiliser `ping -a 10.1.1.1`. |
| ACL correcte mais sans effet | ACL non liée au classifier | Contrôler classifier, behavior et policy. |

## Révision TianSemi

1. Pourquoi une ACL seule ne filtre-t-elle pas encore le trafic dans ce lab ?
2. Quel rôle joue le classifier MQC ?
3. Pourquoi la policy est-elle appliquée en sortie de `GE0/0/2` ?
4. Comment prouver que la règle deny correspond au bon flux ?
5. Pourquoi conserver une règle permit explicite après le deny ?
6. Quelle autre interface/direction pourrait produire un filtrage équivalent ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 136 à 142 (pages 134 à 140 du guide).
