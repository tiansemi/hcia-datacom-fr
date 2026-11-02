---
id: HUA-HCIA-LAB-013
title: "NAT Configuration"
official_title: "NAT Configuration"
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
source_pages: "PDF 164-171 (guide pages 162-169)"
lab_required: true
tags: [huawei, hcia, datacom, lab, nat, napt, easy-ip, nat-server]
devices: [S1, R1, S2]
commands: [nat address-group, nat-policy, source-nat, nat enable, nat server, display session]
publication_status: "not-started"
site_url:
---

# Lab 13 — Configuration de NAT

## Objectif et topologie

Comparer trois fonctions sur R1 : NAPT avec pool public, Easy IP utilisant l’adresse de sortie et NAT Server publiant un service interne.

![S1 privé traverse R1 NAT vers S2 public](<../../assets/diagrammes/09 Labs/lab13-nat-topology-fr.svg>)

- S1 privé : `192.168.1.1/24`, passerelle `192.168.1.254`.
- R1 inside : `192.168.1.254/24` ; outside : `100.0.23.2/24`.
- S2 public simulé : `100.0.23.3/24`.

## 1 — NAPT avec pool

```text
[R1] nat address-group test 1
[R1-address-group-test] section 1 100.1.23.1 100.1.23.254
[R1-address-group-test] mode pat
[R1] nat-policy
[R1-policy-nat] rule name test
[R1-policy-nat-rule-test] source-address range 192.168.1.1 192.168.1.254
[R1-policy-nat-rule-test] action source-nat address-group test
[R1-GE0/0/2] nat enable
[S2] ip route-static 100.1.23.0 24 100.0.23.2
```

Le routeur public simulé doit savoir retourner vers le pool. Lancez plusieurs pings puis lisez les sessions :

```text
[S1] ping -c 1000 100.0.23.3
[R1] display session all verbose
```

## 2 — Easy IP

```text
[R1] nat-policy
[R1-policy-nat] rule name test
[R1-policy-nat-rule-test] action source-nat easy-ip
```

Le même test doit maintenant montrer l’adresse traduite `100.0.23.2`, adresse de l’interface de sortie, au lieu d’une adresse du pool.

## 3 — NAT Server

Le guide publie le service Telnet interne uniquement pour vérifier la traduction de destination :

```text
[R1] nat server for_telnet protocol tcp global 100.0.23.2 telnet inside 192.168.1.1 telnet
<S2> telnet 100.0.23.2
[R1] display session all verbose
```

Telnet/WEAKEA restent limités au lab. En production, publiez un service chiffré, filtrez les sources et évitez de réutiliser un port déjà occupé par le routeur.

## Vérifications attendues

| Mode | Traduction attendue |
| --- | --- |
| NAPT pool | `192.168.1.1` devient une adresse de `100.1.23.0/24` avec PAT. |
| Easy IP | `192.168.1.1` devient `100.0.23.2` avec différenciation par ports. |
| NAT Server | Destination publique `100.0.23.2:23` traduite vers `192.168.1.1:23`. |

## Troubleshooting

- Aucune session : vérifier correspondance de la rule, direction du trafic et `nat enable` outside.
- Aller sans retour : vérifier la route de S2 vers le pool NAPT.
- Easy IP utilise une adresse inattendue : contrôler l’interface de sortie réellement sélectionnée.
- NAT Server inaccessible : vérifier service interne, route, port global et conflit local.

## Révision TianSemi

1. Quelle différence existe entre NAPT avec pool et Easy IP ?
2. Pourquoi S2 a-t-il besoin d’une route vers le pool ?
3. Quel équipement doit porter `nat enable` et sur quelle interface ?
4. Que prouve `display session all verbose` ?
5. Que traduit NAT Server par rapport au source NAT ?
6. Les ports global et interne doivent-ils obligatoirement être identiques ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 164 à 171 (pages 162 à 169 du guide).
