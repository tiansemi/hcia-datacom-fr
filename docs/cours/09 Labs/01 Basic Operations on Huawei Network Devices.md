---
id: HUA-HCIA-LAB-001
title: "Basic Operations on Huawei Network Devices"
official_title: "Basic Operations on Huawei Network Devices"
category: "Huawei HCIA-Datacom Lab"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-06
updated: 2026-08-11
module: "09 Labs"
type: "lab"
source: "HCIA-Datacom V2.0 Lab Guide"
source_pages: "PDF 14-26 (guide pages 12-24)"
lab_required: true
tags: [huawei, hcia, datacom, lab, yunshan-os, cli, console, configuration]
devices: [PC, AR5710-S8T2XE]
commands: [system-view, sysname, display version, display current-configuration, save, display startup]
publication_status: "not-started"
site_url:
---

# Lab 01 — Opérations de base sur un équipement Huawei

## Objectif

Initialiser un routeur Huawei YunShan OS depuis la console, naviguer entre les vues de commandes, utiliser l’aide contextuelle, configurer une interface, sauvegarder la configuration et distinguer l’état courant de l’état chargé au prochain démarrage.

À la fin du lab, vous devez pouvoir expliquer chaque changement d’invite et prouver qu’une configuration survivra — ou non — à un redémarrage.

## Topologie et prérequis

![PC relié au port console du routeur](<../../assets/diagrammes/09 Labs/lab01-console-topology-fr.svg>)

| Élément | Rôle |
| --- | --- |
| PC | Terminal d’administration avec client série. |
| Routeur AR5710-S8T2XE | Équipement à initialiser via son port console. |
| Câble console | Liaison hors bande entre le PC et le routeur. |

Le guide recommande YunShan OS `V600R024C00SPC100`. Les invites et certaines commandes peuvent varier selon le modèle ou la version.

> [!danger] Commandes destructives
> `reset saved-configuration`, `reboot` et `reset factory-configuration` interrompent le service ou effacent des données. Exécutez-les uniquement sur un équipement de lab, après avoir compris la configuration qui sera chargée au prochain démarrage.

## Roadmap

1. Se connecter par console et observer l’équipement.
2. Naviguer entre vue utilisateur, vue système et vue d’interface.
3. Configurer le nom et l’adresse de `GE0/0/2`.
4. Vérifier puis sauvegarder la configuration.
5. Manipuler les fichiers de configuration de démarrage.
6. Tester le comportement au redémarrage et, en fin de lab, la restauration usine.

## 1 — Prendre en main la CLI

À la première connexion, définissez un mot de passe robuste propre au lab. Ne réutilisez pas l’exemple du support officiel dans un environnement réel.

```text
<HUAWEI> display version
<HUAWEI> system-view
[HUAWEI] sysname Datacom-Router
[Datacom-Router] interface GE 0/0/2
[Datacom-Router-GE0/0/2] quit
[Datacom-Router] return
<Datacom-Router>
```

Repères :

- `<...>` correspond à la vue utilisateur ;
- `[...]` correspond à une vue de configuration ;
- `quit` remonte d’un niveau ;
- `return` ou `Ctrl+Z` revient directement à la vue utilisateur ;
- `?` affiche l’aide disponible dans le contexte courant ;
- `Tab` complète un mot-clé lorsqu’il est identifiable sans ambiguïté ;
- `undo` annule une configuration ou rétablit sa valeur par défaut.

## 2 — Configurer et contrôler une interface

Sur l’AR5710-S8T2XE du guide, les interfaces LAN sont initialement en couche 2. `undo portswitch` fait passer `GE0/0/2` en couche 3 afin de lui attribuer une adresse IP.

```text
[Datacom-Router] interface GE 0/0/2
[Datacom-Router-GE0/0/2] undo portswitch
[Datacom-Router-GE0/0/2] ip address 192.168.1.1 24
[Datacom-Router-GE0/0/2] display this
[Datacom-Router-GE0/0/2] quit
[Datacom-Router] display current-configuration
```

`display this` limite la lecture à la vue courante. `display current-configuration` montre la configuration active de l’équipement.

## 3 — Sauvegarder et préparer le prochain démarrage

```text
<Datacom-Router> save
<Datacom-Router> compare configuration
<Datacom-Router> dir
<Datacom-Router> save test.cfg
<Datacom-Router> startup saved-configuration test.cfg
<Datacom-Router> display startup
```

Interprétez les résultats :

- `save` écrit la configuration courante dans un fichier persistant ;
- `compare configuration` compare l’état actif et le fichier prévu au démarrage ;
- `dir` affiche le système de fichiers ;
- `startup saved-configuration` désigne le fichier à charger au prochain démarrage ;
- `display startup` distingue les fichiers actuels et ceux du prochain démarrage.

Un fichier `.cc` contient le logiciel système ; un fichier `.cfg` ou `.zip` peut contenir une configuration.

## 4 — Observer les effets d’un redémarrage

Avant `reboot`, relevez la valeur **Next startup saved-configuration file** dans `display startup`. Répondez ensuite avec attention aux demandes de sauvegarde : accepter de sauvegarder peut recréer une configuration de démarrage même après `reset saved-configuration`.

```text
<Datacom-Router> reset saved-configuration
<Datacom-Router> display startup
<Datacom-Router> reboot
```

Pour terminer le lab sur l’équipement dédié :

```text
<Datacom-Router> reset factory-configuration
```

La réapparition de l’assistant de mot de passe initial après redémarrage confirme la restauration usine.

## Vérifications attendues

| Contrôle | Preuve attendue |
| --- | --- |
| Version et modèle | `display version` identifie le logiciel et l’AR5710-S8T2XE. |
| Vue courante | L’invite change avec `system-view`, `interface`, `quit` et `return`. |
| Interface | `display this` montre `192.168.1.1/24` sur `GE0/0/2`. |
| Persistance | `compare configuration` ne signale plus de différence après `save`. |
| Prochain démarrage | `display startup` pointe vers le fichier choisi. |
| Restauration | L’équipement redemande l’initialisation après la remise à zéro usine. |

## Troubleshooting

| Symptôme | Cause probable | Action |
| --- | --- | --- |
| Aucun affichage console | Mauvais port série, câble ou paramètres du terminal | Vérifier port COM, câble et paramètres recommandés par le matériel. |
| `ip address` refusé | Interface encore en couche 2 | Exécuter `undo portswitch` dans la vue de l’interface. |
| La commande abrégée est ambiguë | Plusieurs mots-clés partagent le préfixe | Utiliser `?`, saisir davantage de caractères, puis `Tab`. |
| Configuration perdue | `save` non exécuté ou mauvais fichier de démarrage | Contrôler `compare configuration` et `display startup`. |
| Configuration conservée après effacement | Sauvegarde acceptée pendant `reboot` | Relire les invites et vérifier le fichier du prochain démarrage avant de confirmer. |

## Révision TianSemi

1. Quelle différence existe entre vue utilisateur, vue système et vue d’interface ?
2. Comment l’aide `?` et la touche `Tab` réduisent-elles les erreurs de saisie ?
3. Pourquoi `undo portswitch` est-il nécessaire avant l’adresse IP dans ce lab ?
4. Quelle différence existe entre configuration courante et configuration de prochain démarrage ?
5. Pourquoi `reset saved-configuration` peut-il sembler inefficace si l’on accepte ensuite la sauvegarde proposée par `reboot` ?
6. Quelles preuves faut-il collecter avant une restauration usine ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 14 à 26 (pages 12 à 24 du guide).
