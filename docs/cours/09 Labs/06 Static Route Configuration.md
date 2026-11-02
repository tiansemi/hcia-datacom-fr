---
id: HUA-HCIA-LAB-006
title: "Static Route Configuration"
official_title: "Static Route Configuration"
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
source_pages: "PDF 79-94 (guide pages 77-92)"
lab_required: true
tags: [huawei, hcia, datacom, lab, static-route, floating-route, default-route, loopback]
devices: [R1, S1, S2]
commands: [ip route-static, display ip routing-table, ping, tracert]
publication_status: "not-started"
site_url:
---

# Lab 06 — Configuration des routes statiques

## Objectif

Construire le routage IPv4 d’un triangle R1–S1–S2, observer les routes directes, joindre les loopbacks par routes statiques, ajouter une route flottante de secours puis remplacer une route spécifique par une route par défaut.

## Topologie

![Triangle de routage statique R1, S1 et S2](<../../assets/diagrammes/09 Labs/lab06-static-routing-topology-fr.svg>)

| Lien | Extrémité A | Extrémité B |
| --- | --- | --- |
| R1–S1 | R1 `10.0.12.2/24` | S1 `10.0.12.1/24` |
| R1–S2 | R1 `10.0.23.2/24` | S2 `10.0.23.3/24` |
| S1–S2 | S1 `10.0.13.1/24` | S2 `10.0.13.3/24` |

Loopbacks : S1 `10.0.1.1/32`, R1 `10.0.2.2/32`, S2 `10.0.3.3/32`.

## 1 — Adresser et observer les routes directes

Passez les six interfaces en couche 3, appliquez le tableau puis vérifiez :

```text
[R1] display ip interface brief
[R1] display ip routing-table
[R1] ping 10.0.12.1
[R1] ping 10.0.23.3
```

Une interface Up/Up avec une adresse produit une route vers son réseau et une route hôte vers son adresse locale.

Créez ensuite les loopbacks :

```text
[R1] interface LoopBack0
[R1-LoopBack0] ip address 10.0.2.2 32
[S1-LoopBack0] ip address 10.0.1.1 32
[S2-LoopBack0] ip address 10.0.3.3 32
```

Un ping sourcé depuis R1 vers `10.0.1.1` échoue encore : le chemin aller et le chemin retour doivent tous deux être connus.

## 2 — Ajouter les routes statiques spécifiques

```text
[R1] ip route-static 10.0.1.1 32 10.0.12.1
[R1] ip route-static 10.0.3.3 32 10.0.23.3
[S1] ip route-static 10.0.2.2 32 10.0.12.2
[S1] ip route-static 10.0.3.3 32 10.0.13.3
[S2] ip route-static 10.0.1.1 32 10.0.13.1
[S2] ip route-static 10.0.2.2 32 10.0.23.2
```

```text
[R1] display ip routing-table
[R1] ping -a 10.0.2.2 10.0.1.1
```

Une route configurée n’entre dans la table que si son next-hop est joignable et si elle constitue le meilleur chemin connu.

## 3 — Créer une route flottante de secours

Le chemin direct R1–S1 reste préféré. Le chemin R1–S2–S1 reçoit une préférence 100, donc moins prioritaire que la route statique normale :

```text
[R1] ip route-static 10.0.1.1 32 10.0.23.3 preference 100
[S1] ip route-static 10.0.2.2 32 10.0.13.3 preference 100
```

Arrêtez R1 `GE0/0/1`, puis contrôlez table, ping sourcé et tracert. La route via S2 doit devenir active. Rétablissez ensuite l’interface.

## 4 — Tester une route par défaut

Supprimez sur R1 les deux routes vers `10.0.1.1/32`, puis configurez :

```text
[R1] ip route-static 0.0.0.0 0 10.0.12.1
[R1] display ip routing-table
[R1] ping -a 10.0.2.2 10.0.1.1
```

La route par défaut sert lorsque aucune route plus spécifique ne correspond à la destination.

## Vérification et pannes contrôlées

| Test | Résultat attendu |
| --- | --- |
| `display ip routing-table` après adressage | Trois réseaux de transit apparaissent comme Direct sur leurs équipements. |
| Ping loopback après routes spécifiques | Réussite dans les deux sens. |
| Arrêt du lien R1–S1 | Activation de la route préférence 100 via S2. |
| Tracert pendant la panne | Passage par S2 avant S1. |
| Route par défaut | `0.0.0.0/0` utilisée en l’absence de route plus spécifique. |

## Troubleshooting

- Route absente : vérifier la joignabilité du next-hop et l’état Up/Up de l’interface.
- Ping aller sans réponse : rechercher la route de retour depuis la destination.
- Route de secours active en permanence : comparer les préférences et supprimer une route concurrente involontaire.
- Test trompeur : préciser la source avec `ping -a` et `tracert -a` pour tester les loopbacks.

## Révision TianSemi

1. Quelles routes sont générées par une interface IPv4 Up/Up ?
2. Pourquoi une route statique configurée peut-elle rester absente de la table ?
3. Quel est le rôle d’une loopback dans ce lab ?
4. Comment une préférence 100 transforme-t-elle une route en secours ?
5. Pourquoi faut-il tester le chemin retour ?
6. Quand la route par défaut est-elle sélectionnée ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 79 à 94 (pages 77 à 92 du guide).
