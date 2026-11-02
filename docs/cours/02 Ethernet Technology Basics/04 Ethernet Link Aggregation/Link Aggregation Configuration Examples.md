---
id: HUA-HCIA-DC-026
title: "Link Aggregation Configuration Examples"
official_title: "Link Aggregation Configuration Examples"
course_unit: "2.4.4"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "intermediate"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-10
updated: 2026-08-10
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 325-333"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 60-71"
tags: [huawei, hcia, datacom, eth-trunk, lacp, cli, link-aggregation]
prerequisites:
  - "[[Link Aggregation in LACP Mode]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Exemples de configuration de l'agrégation de liens (Link Aggregation Configuration Examples)

## Créer un Eth-Trunk

```text
[HUAWEI] interface eth-trunk 1
```

## Choisir le mode

Le Training Material présente :

```text
[HUAWEI-Eth-Trunk1] mode manual load-balance
[HUAWEI-Eth-Trunk1] mode lacp
```

Le Lab Guide utilise, sur sa plateforme, la syntaxe :

```text
[HUAWEI-Eth-Trunk1] mode lacp-static
```

Cette différence reflète la plateforme et la version logicielle. Vérifiez l'aide contextuelle `?` et la documentation du modèle avant de choisir la syntaxe. Le mode doit être identique aux deux extrémités.

## Ajouter les membres

Depuis l'interface physique :

```text
[HUAWEI-GE1/0/1] eth-trunk 1
```

Ou depuis la vue Eth-Trunk :

```text
[HUAWEI-Eth-Trunk1] trunkport GE 1/0/1 to 1/0/3
```

Une interface Ethernet ne peut appartenir qu'à un seul Eth-Trunk.

## Seuils et priorités LACP

```text
[HUAWEI] lacp priority 30000
[HUAWEI-GE1/0/1] lacp priority 30000
[HUAWEI-Eth-Trunk1] lacp max active-linknumber 2
[HUAWEI-Eth-Trunk1] least active-linknumber 2
```

- plus petite priorité = plus haute priorité ;
- le maximum d'actifs doit être le même sur les deux pairs ;
- si le nombre d'actifs passe sous le minimum, l'Eth-Trunk tombe Down afin de garantir la bande passante minimale.

## Exemple manuel avec trunk VLAN

Deux liens transportent les VLAN 10 et 20 :

```text
[SW1] interface eth-trunk 1
[SW1-Eth-Trunk1] mode manual load-balance
[SW1-Eth-Trunk1] trunkport GE 1/0/1 to 1/0/2
[SW1-Eth-Trunk1] port link-type trunk
[SW1-Eth-Trunk1] port trunk allow-pass vlan 10 20
```

Appliquez la même logique sur `SW2` avec les interfaces homologues.

## Exemple LACP avec deux actifs et un secours

```text
[SW1] interface eth-trunk 1
[SW1-Eth-Trunk1] mode lacp
[SW1-Eth-Trunk1] lacp max active-linknumber 2
[SW1-Eth-Trunk1] trunkport GE 1/0/1 to 1/0/3
[SW1-Eth-Trunk1] port link-type trunk
[SW1-Eth-Trunk1] port trunk allow-pass vlan 10 20
[SW1-Eth-Trunk1] quit
[SW1] lacp priority 30000
```

Sur `SW2`, utilisez le même mode, le même maximum d'actifs, les trois ports homologues et les mêmes VLAN. La priorité système inférieure de `SW1` le rend Actor par rapport à la valeur par défaut 32768 de `SW2`.

## Répartition et préemption dans le Lab Guide

Le lab officiel utilise aussi :

```text
[S1-Eth-Trunk1] lacp preempt enable
[S1-Eth-Trunk1] load-balance dst-ip
```

La préemption permet à un membre de meilleure priorité redevenu disponible de reprendre son rôle après le délai prévu. Le mode `dst-ip` répartit les flux sortants selon l'adresse IP destination.

## Vérification obligatoire

```text
<S1> display eth-trunk 1
```

Contrôlez :

- état global de l'Eth-Trunk ;
- mode de fonctionnement ;
- membres Selected et Unselected ;
- nombre maximal et minimal de liens actifs ;
- cohérence des interfaces avec l'équipement pair.

## Mini-lab TianSemi

1. Reliez deux switchs avec trois liens.
2. Créez Eth-Trunk 1 en LACP statique selon la syntaxe disponible.
3. Limitez le groupe à deux liens actifs et gardez le troisième en secours.
4. Configurez l'interface logique comme trunk pour les VLAN 10 et 20.
5. Vérifiez `display eth-trunk 1` aux deux extrémités.
6. Déconnectez un membre actif et confirmez la sélection du secours.
7. Vérifiez que l'Eth-Trunk reste Up tant que le minimum d'actifs est respecté.
8. Déconnectez un second membre et observez l'effet du seuil minimal.

## Troubleshooting

| Symptôme | Contrôle prioritaire |
| --- | --- |
| Eth-Trunk Down | Mode, minimum d'actifs, membres et configuration du pair |
| Membres Unselected inattendus | Maximum actif, priorités, numéros et débits |
| VLAN ne traverse pas | Configurer le trunk sur l'Eth-Trunk logique, pas séparément sur les membres |
| Commande LACP rejetée | Vérifier si la plateforme attend `mode lacp` ou `mode lacp-static` |
| Perte après retour d'un meilleur lien | État et délai de préemption |
| Charge concentrée sur un membre | Algorithme de hachage inadapté aux flux |

## À retenir pour l'examen HCIA-Datacom

- `interface eth-trunk` crée l'interface logique.
- Les membres peuvent être ajoutés depuis leur vue ou avec `trunkport`.
- Le mode doit être identique aux deux extrémités.
- `lacp max active-linknumber` définit le nombre maximal d'actifs.
- `least active-linknumber` protège la bande passante minimale.
- La configuration VLAN s'applique à l'Eth-Trunk logique.
- `display eth-trunk` est la vérification principale.

## Révision TianSemi

1. Quelles sont les deux méthodes pour ajouter un membre à un Eth-Trunk ?
2. Pourquoi les syntaxes `mode lacp` et `mode lacp-static` peuvent-elles différer ?
3. Quel paramètre doit être identique aux deux extrémités en LACP ?
4. À quoi sert `least active-linknumber` ?
5. Où configure-t-on le trunk VLAN : membres ou interface logique ?
6. Quelle commande affiche l'état et les membres du groupe ?
7. Quel effet attend-on après la panne d'un membre actif avec un secours disponible ?
8. Pourquoi le choix de l'algorithme de charge doit-il suivre la diversité des flux ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 325 à 333.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 60 à 71 pour le lab officiel de configuration Ethernet Link Aggregation.
