---
id: HUA-HCIA-DC-054
title: "Source NAT"
official_title: "Source NAT"
course_unit: "4.4.2"
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
source: "HCIA-Datacom V2.0 Training Material; HCIA-Datacom V2.0 Lab Guide"
source_pages: "Training PDF 702-722; Lab PDF 164-168"
tags: [huawei, hcia, datacom, source-nat, no-pat, napt, easy-ip, nat-policy]
prerequisites:
  - "[[Overview of NAT]]"
related:
  - "[[Destination NAT]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Source NAT

## Fonctionnement général

Source NAT remplace l'adresse source privée d'un paquet sortant par une adresse publique. La réponse reçue sur cette adresse est traduite en sens inverse et remise à l'hôte interne.

Une politique de source NAT contient :

- des conditions de correspondance, par exemple source, destination, interface, service ou période ;
- l'adresse après traduction, issue d'un pool ou de l'interface sortante ;
- l'action, par exemple NAPT ou Easy IP.

Les règles sont évaluées de haut en bas et la première correspondance arrête la recherche. Une nouvelle règle est placée à la fin, sauf réorganisation explicite.

## Trois variantes

![Comparaison de NAT No-PAT, NAPT et Easy IP](<../../../assets/diagrammes/04 Network Security and Services/source-nat-types-fr.svg>)

| Variante | Traduction | Adresse publique | Usage |
| --- | --- | --- | --- |
| No-PAT | Adresse uniquement | Une adresse du pool par hôte simultané | Ressources publiques abondantes, peu d'hôtes. |
| NAPT | Adresse et port | Une ou plusieurs adresses d'un pool partagées | Beaucoup d'hôtes, peu d'adresses publiques. |
| Easy IP | Adresse et port | Adresse de l'interface WAN | Adresse publique obtenue dynamiquement ou absence de pool. |

Dans le support, NAPT et Easy IP attribuent des ports traduits entre 2048 et 65535.

## Tables utilisées

La **server mapping table** conserve des associations prédéfinies ou semi-persistantes, notamment pour No-PAT et NAT Server. La **session table** décrit chaque flux par son cinq-tuple et les valeurs traduites ; elle vieillit à la fin de la session.

NAPT et Easy IP s'appuient sur la table de sessions pour distinguer plusieurs flux partageant la même adresse publique. Pour ICMP, qui n'a pas de ports TCP/UDP, l'identifiant ICMP joue un rôle comparable.

## Configuration NAPT du Lab Guide

```text
[R1] nat address-group test 1
[R1-address-group-test] section 1 100.1.23.1 100.1.23.254
[R1-address-group-test] mode pat
[R1-address-group-test] quit
[R1] nat-policy
[R1-policy-nat] rule name test
[R1-policy-nat-rule-test] source-address range 192.168.1.1 192.168.1.254
[R1-policy-nat-rule-test] action source-nat address-group test
[R1-policy-nat-rule-test] quit
[R1-policy-nat] quit
[R1] interface GE 0/0/2
[R1-GE0/0/2] nat enable
```

Le réseau amont doit disposer d'une route vers le pool public lorsque celui-ci n'est pas directement porté par l'interface. `route enable` peut générer des OPR comparables à des routes de rejet afin d'éviter une boucle vers un pool sans session correspondante.

## Passer à Easy IP

Dans la même règle :

```text
[R1] nat-policy
[R1-policy-nat] rule name test
[R1-policy-nat-rule-test] action source-nat easy-ip
```

L'adresse publique devient celle de l'interface de sortie au lieu d'une adresse du pool.

## Vérification

```text
<R1> display session all verbose
```

Pour NAPT, `New SrcAddr` doit appartenir au pool. Pour Easy IP, il doit correspondre à l'adresse WAN. Sur un flux TCP ou UDP, contrôler aussi le port traduit ; sur un ping, contrôler `New IcmpId`.

## Troubleshooting

| Symptôme | Contrôle |
| --- | --- |
| Pas de traduction | `nat enable`, ordre des règles et conditions de correspondance. |
| Traduction présente, pas de retour | Route amont vers le pool, route par défaut et filtrage. |
| Pool épuisé en No-PAT | Nombre d'adresses disponibles et sessions actives. |
| Mauvaise adresse publique | Action Easy IP ou pool réellement lié à la règle. |
| Boucle vers une adresse du pool | OPR générée avec `route enable` et routage amont. |

## À retenir pour l'examen HCIA-Datacom

- No-PAT conserve les ports et consomme une adresse publique par hôte simultané.
- NAPT partage les adresses publiques grâce à la traduction des ports.
- Easy IP utilise l'adresse de l'interface sortante sans pool.
- Les politiques NAT sont évaluées dans l'ordre, première correspondance gagnante.
- La table de sessions porte les associations dynamiques de flux.
- `nat enable` est requis sur l'interface concernée.

## Révision TianSemi

1. Quels éléments composent une règle de politique source NAT ?
2. Pourquoi NAPT économise-t-il davantage d'adresses que No-PAT ?
3. Quelle différence existe entre NAPT et Easy IP ?
4. Quelle table permet de restaurer adresse et port d'un flux NAPT ?
5. Quel champ remplace le port pour distinguer des sessions ICMP ?
6. Pourquoi le réseau amont doit-il connaître le pool public ?
7. Que vérifiez-vous si aucune entrée de session NAT n'apparaît ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 702 à 722.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 164 à 168, étapes NAPT et Easy IP du lab « NAT Configuration ».
