---
id: HUA-HCIA-LAB-003
title: "Spanning Tree Protocol Configuration"
official_title: "Spanning Tree Protocol Configuration"
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
source_pages: "PDF 47-59 (guide pages 45-57)"
lab_required: true
tags: [huawei, hcia, datacom, lab, stp, rstp, root-bridge, port-cost, edge-port]
devices: [S1, S2, S3, S4]
commands: [stp enable, stp mode, stp root primary, stp root secondary, stp cost, stp edged-port enable]
publication_status: "not-started"
site_url:
---

# Lab 03 — Configuration de STP et RSTP

## Objectif

Déployer un spanning tree sur une topologie de couche 2 redondante, maîtriser l’élection du pont racine, influencer le rôle d’un port par son coût, migrer vers RSTP et déclarer correctement les ports de bordure.

Le résultat précis de l’élection initiale dépend des adresses MAC de vos commutateurs. Il faut donc interpréter les sorties locales au lieu de recopier les rôles observés dans le guide.

## Topologie

![Topologie maillée de quatre commutateurs pour le lab STP](<../../assets/diagrammes/09 Labs/lab03-stp-topology-fr.svg>)

La topologie comporte deux liens entre S1 et S2, deux entre S3 et S4 et quatre liaisons verticales ou diagonales. Elle est volontairement redondante : sans mécanisme de prévention, plusieurs chemins de couche 2 formeraient des boucles.

## Roadmap

1. Isoler les ports non utilisés et observer l’état initial.
2. Activer STP et forcer le mode STP sur les quatre commutateurs.
3. Définir S1 comme racine principale et S2 comme racine secondaire.
4. Modifier le coût d’un port de S4 pour changer son port racine.
5. Migrer les quatre équipements vers RSTP.
6. Configurer les ports uniquement reliés aux terminaux comme ports edge.
7. Tester un défaut de lien et la continuité par le chemin de secours.

## 1 — Observer la convergence STP

Après avoir vérifié le câblage et arrêté les ports inutilisés du scénario, configurez chaque commutateur :

```text
[S1] stp enable
[S1] stp mode stp
[S2] stp mode stp
[S3] stp mode stp
[S4] stp mode stp
```

Sur les plateformes du guide, STP est activé par défaut et le mode par défaut est MSTP. Les commandes ci-dessus rendent l’exercice explicite.

```text
[S1] display stp
[S1] display stp brief
[S2] display stp brief
[S3] display stp brief
[S4] display stp brief
```

Pour chaque commutateur, consignez : Bridge ID, Root ID, coût vers la racine, port racine, ports désignés, ports alternatifs et état forwarding/discarding.

## 2 — Maîtriser le choix de la racine

```text
[S1] stp root primary
[S2] stp root secondary
```

Dans l’instance 0, `stp root primary` attribue à S1 une priorité de 0 ; `stp root secondary` attribue à S2 une priorité de 4096. Vérifiez que S1 présente le même Bridge ID local et Root ID, avec un coût racine de 0.

```text
[S1] display stp
[S2] display stp
```

Le pont racine doit être choisi selon sa capacité et sa position dans l’architecture, pas laissé au hasard des adresses MAC.

## 3 — Influencer le port racine de S4

Le guide augmente le coût de S4 `GE1/0/1` afin que `GE1/0/2` devienne le meilleur chemin :

```text
[S4] interface GE 1/0/1
[S4-GE1/0/1] stp cost 50000
[S4-GE1/0/1] quit
[S4] display stp brief
[S4] display stp
```

Résultat attendu : `GE1/0/1` devient alternatif/discarding et `GE1/0/2` devient root/forwarding. Le coût total vers S1 doit refléter le nouveau chemin.

## 4 — Passer à RSTP et déclarer les ports edge

```text
[S1] stp mode rstp
[S2] stp mode rstp
[S3] stp mode rstp
[S4] stp mode rstp
```

Le passage à RSTP ne doit pas modifier la topologie logique voulue. Sur S3, le guide traite `GE1/0/10` à `GE1/0/24` comme ports exclusivement reliés à des terminaux :

```text
[S3] interface range GE 1/0/10 to GE 1/0/24
[S3-port-group] stp edged-port enable
```

Un port edge ne doit pas relier un autre commutateur. S’il reçoit une BPDU, le système peut le requalifier et recalculer l’arbre.

## Vérification et test de résilience

1. Dessinez l’arbre convergé à partir de `display stp brief`.
2. Identifiez le lien bloqué qui protège chaque boucle.
3. Choisissez un port forwarding non critique et relevez son rôle.
4. Arrêtez-le pour simuler une panne :

```text
[Sx] interface GE x/x/x
[Sx-GEx/x/x] shutdown
```

5. Attendez la convergence, vérifiez les nouveaux rôles et testez la continuité.
6. Rétablissez le port avec `undo shutdown` et confirmez une nouvelle convergence stable.

Ne désactivez pas STP pour créer la panne : vous risqueriez une vraie boucle et une tempête de broadcast.

## Troubleshooting

| Symptôme | Cause probable | Vérification / correction |
| --- | --- | --- |
| Racine inattendue | Priorité non appliquée ou mauvaise instance | `display stp`, puis contrôler `stp root primary/secondary`. |
| Mauvais port racine | Coût, priorité de port ou topologie différents | Comparer coûts cumulés et rôles avec `display stp brief`. |
| Tous les liens ne transmettent pas | Fonctionnement normal d’un arbre sans boucle | Identifier le port alternate/discarding ; ne pas le forcer en forwarding. |
| Recalculs fréquents | Port terminal non edge ou liaison instable | Vérifier Last TC, câblage et configuration edge. |
| Perte après panne unique | Chemin de secours absent ou également down | Contrôler états physiques et nouvelle convergence sur chaque commutateur. |

## Révision TianSemi

1. Pourquoi deux liens parallèles de couche 2 ne peuvent-ils pas tous transmettre indépendamment sans agrégation ?
2. Comment prouver qu’un commutateur est la racine ?
3. Quel effet produit l’augmentation du coût de `GE1/0/1` sur S4 ?
4. Pourquoi le résultat initial peut-il différer de celui du guide ?
5. Quelle différence opérationnelle observe-t-on entre STP et RSTP dans ce lab ?
6. Quels ports peuvent être déclarés edge et quel risque existe si un commutateur y est branché ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 47 à 59 (pages 45 à 57 du guide).
