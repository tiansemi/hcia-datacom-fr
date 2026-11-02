---
id: HUA-HCIA-LAB-004
title: "Ethernet Link Aggregation Configuration"
official_title: "Ethernet Link Aggregation Configuration"
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
source_pages: "PDF 60-71 (guide pages 58-69)"
lab_required: true
tags: [huawei, hcia, datacom, lab, eth-trunk, lacp, aggregation, load-balancing, resilience]
devices: [S1, S2]
commands: [interface Eth-Trunk, mode lacp-static, eth-trunk, lacp priority, least active-linknumber, lacp max active-linknumber, load-balance]
publication_status: "not-started"
site_url:
---

# Lab 04 — Configuration de l’agrégation de liens Ethernet

## Objectif

Regrouper trois liaisons physiques entre S1 et S2 dans `Eth-Trunk1`, comparer le mode manuel au mode LACP statique, maîtriser la sélection des liens actifs, tester le secours et configurer l’algorithme de répartition.

## Topologie

![Trois liens physiques regroupés entre S1 et S2](<../../assets/diagrammes/09 Labs/lab04-eth-trunk-topology-fr.svg>)

| S1 | S2 | Rôle |
| --- | --- | --- |
| `GE1/0/10` | `GE1/0/10` | Membre, prévu en secours après réglage des priorités. |
| `GE1/0/11` | `GE1/0/11` | Membre actif prioritaire. |
| `GE1/0/12` | `GE1/0/12` | Membre actif prioritaire. |

Toutes les interfaces membres doivent présenter des caractéristiques compatibles et être ajoutées symétriquement aux Eth-Trunk des deux extrémités.

## Roadmap

1. Créer `Eth-Trunk1` en mode manuel et ajouter les trois membres.
2. Vérifier l’état agrégé.
3. Retirer les membres, passer l’Eth-Trunk en LACP statique, puis les réajouter.
4. Faire de S1 l’acteur LACP et préférer `GE1/0/11` et `GE1/0/12`.
5. Limiter à deux liens actifs, exiger au moins deux liens et activer la préemption.
6. Simuler une puis deux pannes de membres.
7. Rétablir les liens et changer le mode de répartition.

## 1 — Construire l’Eth-Trunk en mode manuel

```text
[S1] interface Eth-Trunk 1
[S1-Eth-Trunk1] mode manual load-balance
[S2] interface Eth-Trunk 1
[S2-Eth-Trunk1] mode manual load-balance
```

Ajoutez les membres un par un ou en lot après avoir supprimé une éventuelle configuration de type de lien incompatible :

```text
[S1] interface GE 1/0/10
[S1-GE1/0/10] undo port link-type
[S1-GE1/0/10] eth-trunk 1

[S1] interface Eth-Trunk 1
[S1-Eth-Trunk1] trunkport GE 1/0/11 to 1/0/12

[S2] interface Eth-Trunk 1
[S2-Eth-Trunk1] trunkport GE 1/0/10 to 1/0/12
```

```text
[S1] display eth-trunk 1
```

En mode manuel, la sortie indique `Working Mode: Normal`. Les trois ports doivent être Up.

## 2 — Migrer vers LACP statique

Un Eth-Trunk ne change pas de mode tant qu’il conserve des interfaces membres. Retirez-les d’abord des deux côtés :

```text
[S1] interface Eth-Trunk 1
[S1-Eth-Trunk1] undo trunkport GE 1/0/10 to 1/0/12
[S1-Eth-Trunk1] mode lacp-static
[S1-Eth-Trunk1] trunkport GE 1/0/10 to 1/0/12

[S2] interface Eth-Trunk 1
[S2-Eth-Trunk1] undo trunkport GE 1/0/10 to 1/0/12
[S2-Eth-Trunk1] mode lacp-static
[S2-Eth-Trunk1] trunkport GE 1/0/10 to 1/0/12
```

`display eth-trunk 1` doit maintenant indiquer `Working Mode: Static` et afficher les informations Actor et Partner.

## 3 — Sélectionner deux liens actifs

Le guide attribue à S1 une priorité système LACP de 100, inférieure à la valeur par défaut 32768, afin d’en faire l’Actor. Il dégrade ensuite la priorité de `GE1/0/10` à 40000 :

```text
[S1] lacp priority 100
[S1] interface GE 1/0/10
[S1-GE1/0/10] lacp priority 40000

[S1] interface Eth-Trunk 1
[S1-Eth-Trunk1] lacp max active-linknumber 2
[S1-Eth-Trunk1] least active-linknumber 2
[S1-Eth-Trunk1] lacp preempt enable
```

Une valeur de priorité plus petite est préférée. `GE1/0/11` et `GE1/0/12` deviennent donc Selected ; `GE1/0/10` reste Unselect en secours.

- `lacp max active-linknumber 2` limite le nombre de membres actifs à deux.
- `least active-linknumber 2` met l’Eth-Trunk Down s’il reste moins de deux membres actifs.
- `lacp preempt enable` permet aux liens de meilleure priorité de reprendre leur rôle après rétablissement ; le délai par défaut observé dans le guide est 30 secondes.

## 4 — Tester le secours et le seuil minimal

```text
[S1] interface GE 1/0/12
[S1-GE1/0/12] shutdown
[S1] display eth-trunk 1
```

`GE1/0/10` doit remplacer le lien défaillant et l’Eth-Trunk rester Up avec deux liens actifs.

```text
[S1] interface GE 1/0/11
[S1-GE1/0/11] shutdown
[S1] display eth-trunk 1
```

Il ne reste plus assez de membres pour satisfaire le seuil minimal : l’Eth-Trunk passe Down. Rétablissez ensuite `GE1/0/11` et `GE1/0/12`, attendez la préemption, puis confirmez que les deux liens prioritaires sont de nouveau Selected.

## 5 — Choisir la répartition

```text
[S1] interface Eth-Trunk 1
[S1-Eth-Trunk1] load-balance dst-ip
```

Le hachage par IP destination conserve les paquets d’un même flux sur un membre cohérent. La répartition s’applique au trafic sortant ; les deux extrémités peuvent donc employer des critères différents sans empêcher la formation de l’agrégation.

## Vérifications attendues

| État | Preuve dans `display eth-trunk 1` |
| --- | --- |
| Mode manuel | `Working Mode: Normal`, trois ports Up. |
| LACP initial | `Working Mode: Static`, Actor et Partner visibles. |
| Sélection nominale | `GE1/0/11` et `GE1/0/12` Selected, `GE1/0/10` Unselect. |
| Première panne | `GE1/0/10` devient Selected, agrégat Up. |
| Deuxième panne | Nombre actif inférieur à 2, agrégat Down. |
| Rétablissement | Après le délai de préemption, les liens prioritaires redeviennent Selected. |
| Répartition | `Hash Arithmetic` reflète le critère configuré. |

## Troubleshooting

| Symptôme | Cause probable | Correction |
| --- | --- | --- |
| Membre refusé | Interface déjà membre d’un autre Eth-Trunk ou configuration incompatible | Retirer l’ancienne appartenance et aligner les paramètres. |
| Agrégat Down d’un seul côté | Membre distant absent ou mauvais câblage | Vérifier la correspondance exacte des interfaces aux deux extrémités. |
| Mode LACP impossible à changer | Interfaces membres encore présentes | Exécuter `undo trunkport ...`, changer le mode, puis réajouter. |
| Lien de secours jamais sélectionné | Priorités ou seuils incohérents | Contrôler Actor, priorités et `max/least active-linknumber`. |
| Eth-Trunk Down malgré un port Up | Seuil minimal réglé à 2 | Rétablir un deuxième membre ou revoir le seuil justifié par la bande passante minimale. |

## Révision TianSemi

1. Quels bénéfices l’Eth-Trunk apporte-t-il par rapport à trois liens STP indépendants ?
2. Pourquoi faut-il retirer les membres avant de passer du mode manuel au mode LACP ?
3. Comment l’Actor LACP et les ports actifs sont-ils sélectionnés ?
4. Quelle différence existe entre `least active-linknumber` et `lacp max active-linknumber` ?
5. Pourquoi `GE1/0/10` remplace-t-il un lien défaillant mais ne suffit-il pas lorsque deux liens sont arrêtés ?
6. Quel effet produit la préemption après le rétablissement des liens prioritaires ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 60 à 71 (pages 58 à 69 du guide).
