---
id: HUA-HCIA-DC-010
title: "Configuring Network Devices"
official_title: "Configuring Network Devices"
course_unit: "1.3.3"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-13
updated: 2026-08-10
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 106-151"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 14-25"
tags: [huawei, hcia, datacom, cli, file-system, configuration-files]
prerequisites:
  - "[[Operating System Overview]]"
  - "[[Logging In to Network Devices]]"
lab_required: true
labs:
  - "[[01 Basic Operations on Huawei Network Devices]]"
publication_status: "not-started"
site_url:
---

# Configuration des équipements réseau (Configuring Network Devices)

## Organisation de l'unité

Huawei divise cette unité en trois parties :

1. CLI Overview ;
2. File System Management ;
3. Configuration File Management.

Les commandes ci-dessous font partie du contenu officiel. Leur disponibilité exacte dépend du modèle et de la version logicielle.

## 1. CLI Overview

### Structure d'une commande

Une commande Huawei suit une structure unifiée :

```text
mot-de-commande [mot-clé] [paramètre valeur]
```

- le **mot de commande** indique l'action, comme `display` ou `reboot` ;
- le **mot-clé** précise la fonction ;
- la **liste de paramètres** limite la commande avec des noms et des valeurs.

Exemple Huawei :

```text
display ip interface GE1/0/1
```

Les différents éléments sont séparés par des espaces.

### Vues de commande

Les commandes sont organisées par vues.

| Vue | Prompt | Fonction |
| --- | --- | --- |
| Utilisateur | `<HUAWEI>` | état, statistiques, commandes d'affichage et outils comme ping ou telnet |
| Système | `[HUAWEI]` | paramètres globaux et accès aux vues de configuration |
| Interface | `[HUAWEI-GE1/0/1]` | paramètres physiques, liaison et IP d'une interface |
| Protocole | `[HUAWEI-ospf-1]` | paramètres du protocole concerné |
| Zone OSPF | `[HUAWEI-ospf-1-area-0.0.0.0]` | paramètres d'une zone OSPF |

Navigation présentée :

```text
<HUAWEI> system-view
[HUAWEI] interface GE1/0/1
[HUAWEI-GE1/0/1] ip address 192.168.1.1 24
[HUAWEI-GE1/0/1] quit
[HUAWEI] ospf 1
[HUAWEI-ospf-1] area 0
[HUAWEI-ospf-1-area-0.0.0.0] return
<HUAWEI>
```

- `quit` revient à la vue précédente ;
- `return` revient directement à la vue utilisateur.

### Niveaux de privilège

Le système associe un niveau aux utilisateurs et aux commandes.

| Niveau | Catégorie | Portée générale |
| --- | --- | --- |
| 0 | Visit | diagnostics comme ping/tracert et certaines commandes d'affichage |
| 1 | Monitoring | maintenance et commandes d'affichage |
| 2 | Configuration | configuration des services réseau |
| 3 | Management | opérations système, fichiers, transferts et debugging |

Huawei déconseille de modifier les niveaux par défaut sans nécessité, afin d'éviter les risques de sécurité. Certaines commandes `display`, notamment celles qui affichent les configurations courante ou sauvegardée, peuvent exiger le niveau 3.

### Édition de la ligne de commande

#### Touches de base

| Touche | Fonction |
| --- | --- |
| Backspace | supprime le caractère à gauche du curseur |
| Flèche gauche ou `Ctrl+B` | recule d'un caractère |
| Flèche droite ou `Ctrl+F` | avance d'un caractère |

#### Mot-clé incomplet

Un mot-clé peut être abrégé si les caractères saisis identifient une commande unique.

```text
display current-configuration
d cu
di cu
dis cu
```

Une abréviation ambiguë produit une erreur et doit être complétée.

#### Touche Tab

- si l'entrée correspond à un seul mot-clé, Tab le complète ;
- si plusieurs mots-clés correspondent, des pressions successives parcourent les possibilités ;
- si aucun mot-clé ne correspond, l'entrée reste inchangée.

### Aide en ligne

Le caractère `?` fournit une aide adaptée à la vue et à la position courante.

```text
<HUAWEI> ?
[HUAWEI-ui-vty0-4] authentication-mode ?
[HUAWEI] ssh server timeout ?
<HUAWEI> d?
<HUAWEI> display s?
```

- `?` seul affiche les commandes de la vue ;
- un espace suivi de `?` affiche les mots-clés ou paramètres possibles ;
- une chaîne directement suivie de `?` affiche les mots-clés qui commencent par cette chaîne ;
- `<cr>` indique que la commande peut être exécutée à cet emplacement.

### Messages d'erreur

| Message | Signification |
| --- | --- |
| `Unrecognized command` | commande ou mot-clé non reconnu |
| `Wrong parameter` | type ou valeur du paramètre incorrect |
| `Incomplete command` | commande incomplète |
| `Too many parameters` | trop de paramètres fournis |
| `Ambiguous command` | saisie insuffisamment précise |

Le symbole `^` indique la position où l'analyse de la commande échoue.

### Commande undo

Une commande précédée de `undo` sert généralement à restaurer une valeur par défaut, désactiver une fonction ou supprimer une configuration.

```text
[HUAWEI] sysname Server
[Server] undo sysname

[HUAWEI] sftp server enable
[HUAWEI] undo sftp server enable

[HUAWEI-GE1/0/1] ip address 192.168.1.1 24
[HUAWEI-GE1/0/1] undo ip address
```

### Historique des commandes

Par défaut, le support indique que les dix dernières commandes sont conservées pour chaque utilisateur. La taille maximale configurable est 256.

| Action | Commande ou touche |
| --- | --- |
| afficher l'historique | `display history-command [ all-users ]` |
| commande précédente | flèche haut ou `Ctrl+P` |
| commande suivante | flèche bas ou `Ctrl+N` |
| effacer son historique | `reset history-command` |

Les commandes sont conservées telles qu'elles ont été saisies, y compris sous forme abrégée.

### Raccourcis

Huawei distingue les raccourcis personnalisables et les raccourcis système.

Raccourcis personnalisables : `Ctrl+G`, `Ctrl+L`, `Ctrl+O` et `Ctrl+U`.

Valeurs par défaut citées :

- `Ctrl+G` : `display current-configuration` ;
- `Ctrl+L` : `display ip routing-table` ;
- `Ctrl+O` : `undo debugging all` ;
- `Ctrl+U` : aucune commande.

Raccourcis système importants :

| Raccourci | Fonction |
| --- | --- |
| `Ctrl+A` / `Ctrl+E` | début / fin de ligne |
| `Ctrl+C` | arrête l'opération courante |
| `Ctrl+D` | supprime le caractère courant |
| `Ctrl+H` | supprime le caractère à gauche |
| `Ctrl+I` | même fonction que Tab |
| `Ctrl+Z` | revient à la vue utilisateur |
| `Ctrl+]` | arrête ou redirige une connexion entrante |

### Commandes de configuration de base

#### Messages de connexion

```text
[HUAWEI] header login { information text | file file-name }
[HUAWEI] header shell { information text | file file-name }
[HUAWEI] system login information disable
```

`header login` définit le message présenté pendant la tentative de connexion ; `header shell` définit celui présenté après une connexion réussie.

#### Heure et nom de l'équipement

```text
<HUAWEI> clock timezone time-zone-name { add | minus } offset
<HUAWEI> clock datetime [ utc ] time date
[HUAWEI] sysname host-name
```

L'heure correcte est nécessaire pour que les journaux et alarmes possèdent des timestamps fiables. Chaque équipement doit également avoir un nom distinctif.

#### Adresse de management

Par une interface de management :

```text
<HUAWEI> system-view
[HUAWEI] interface MEth0/0/0
[HUAWEI-MEth0/0/0] ip address ip-address { mask | mask-length }
```

Par une interface de service utilisée pour le management :

```text
[HUAWEI] interface GE1/0/1
[HUAWEI-GE1/0/1] undo portswitch
[HUAWEI-GE1/0/1] ip address ip-address { mask | mask-length }
```

La commande `undo portswitch` et les noms d'interfaces dépendent des capacités du modèle.

## 2. File System Management

### Fichiers et supports de stockage

Le système de fichiers gère les fichiers et répertoires des supports de stockage.

| Type de fichier | Rôle | Extensions citées |
| --- | --- | --- |
| Logiciel système | démarrage et fonctions de l'équipement | `.cc` |
| Configuration | commandes restaurées au démarrage | `.cfg`, `.zip`, `.dat` |
| Correctif | correction compatible avec le logiciel | `.pat` |

Supports cités : SDRAM, NVRAM, flash, carte SD et support USB. La flash et les cartes SD sont non volatiles ; la SDRAM contient les informations de fonctionnement en mémoire.

### Méthodes de gestion

- gestion locale après connexion par console, Telnet ou STelnet ;
- TFTP pour des transferts simples en environnement adapté ;
- FTP lorsque les exigences de sécurité sont faibles ;
- SFTP pour les sauvegardes et transferts sécurisés ;
- SCP pour un transfert sécurisé et efficace.

Le cours se concentre sur la gestion locale.

### Commandes locales

#### Répertoires

| Opération | Commande |
| --- | --- |
| afficher le répertoire courant | `pwd` |
| changer de répertoire | `cd [ directory ]` |
| lister les fichiers | `dir [ /all ] [ filename | /all-filesystems ]` |
| créer un répertoire | `mkdir directory` |
| supprimer un répertoire vide | `rmdir directory` |

#### Fichiers

| Opération | Commande |
| --- | --- |
| afficher le contenu | `more file-name [ offset ] [ hex ]` |
| afficher les dernières lignes | `tail file-name [ line ]` |
| copier | `copy source-filename destination-filename [ all ]` |
| déplacer | `move source-filename destination-filename` |
| renommer | `rename old-name new-name` |
| compresser | `zip source-filename destination-filename` |
| décompresser | `unzip source-filename destination-filename` |
| supprimer | `delete [ /unreserved ] [ /quiet ] filename [ all ]` |
| restaurer depuis la corbeille | `undelete filename` |
| vider la corbeille | `reset recycle-bin [ /f | filename ]` |

`/unreserved` rend la suppression non restaurable. Les suppressions et écrasements doivent donc être confirmés avec prudence.

### Exemple officiel de gestion locale

```text
<HUAWEI> dir
<HUAWEI> mkdir test
<HUAWEI> copy vrpcfg.zip flash:/test/backup.zip
<HUAWEI> cd test
<HUAWEI> pwd
<HUAWEI> dir
```

L'exemple crée `test`, copie `vrpcfg.zip` sous le nom `backup.zip`, puis vérifie le résultat.

## 3. Configuration File Management

### Fichiers de configuration

Un fichier de configuration est un ensemble de commandes exécutables par l'équipement. Sa gestion permet de consulter, sauvegarder, comparer, sauvegarder à l'extérieur, restaurer, compresser, supprimer ou restaurer des configurations, et de choisir le fichier du prochain démarrage.

Huawei distingue :

| Catégorie | Description |
| --- | --- |
| Paramètres usine | configuration par défaut fournie avec l'équipement |
| Configuration prédéfinie | contenu d'un fichier `.defcfg` importé au démarrage |
| Configuration courante | commandes actuellement actives |
| Configuration du prochain démarrage | fichier chargé lors du prochain boot |

Formats présentés :

- `.cfg` : texte contenant les commandes ;
- `.zip` : version compressée d'un fichier `.cfg` ;
- `.dat` : base binaire, fichier texte et informations matérielles, uniquement valide lorsqu'elle est exportée par un équipement Huawei ;
- `.defcfg` : fichier texte de configuration prédéfinie.

Le fichier doit respecter l'ordre, les dépendances et les commandes de changement de vue nécessaires.

### Vérification des configurations

| Vérification | Commande |
| --- | --- |
| fichiers présents | `dir` |
| fichiers du démarrage courant et suivant | `display startup` |
| contenu d'un fichier précis | `display configuration configuration-file` |
| configuration du prochain démarrage | `display saved-configuration` |
| dernière configuration sauvegardée | `display saved-configuration last` |
| heure de la dernière sauvegarde | `display saved-configuration time` |
| configuration active complète | `display current-configuration [ include-default ]` |
| configuration active de la vue courante | `display this [ include-default ]` |

### Sauvegarde

Une configuration modifiée est perdue au redémarrage si elle n'est pas sauvegardée.

```text
[HUAWEI] configuration file auto-save [ interval interval | delay delay-interval | cpu-limit cpu-usage ]
<HUAWEI> save [ configuration-file ]
```

Huawei distingue la sauvegarde automatique selon intervalle ou délai et la sauvegarde manuelle avec `save`.

### Fichier du prochain démarrage

```text
<HUAWEI> startup saved-configuration configuration-file
<HUAWEI> display startup
<HUAWEI> reboot
<HUAWEI> reboot fast
```

`reboot` demande si la configuration doit être sauvegardée ; `reboot fast` ne présente pas cette demande. Un redémarrage interrompt les services.

### Effacement des configurations

```text
<HUAWEI> reset saved-configuration
<HUAWEI> reset factory-configuration
```

- `reset saved-configuration` efface les configurations de service du fichier de démarrage ;
- `reset factory-configuration` restaure les paramètres usine et efface un périmètre plus large, notamment des données et certaines configurations système.

Ces commandes sont destructives et le support les réserve à des scénarios maîtrisés : incompatibilité après mise à niveau, fichier endommagé, migration ou retour usine.

### Exemple officiel de mise à niveau logicielle

Le scénario Huawei suit cette séquence :

1. vérifier l'état de démarrage avec `display startup` ;
2. transférer le nouveau logiciel et le correctif, par exemple avec SFTP ;
3. vérifier les fichiers et l'espace avec `dir` ;
4. sauvegarder la configuration avec `save` ;
5. définir le logiciel du prochain démarrage avec `startup system-software` ;
6. définir la configuration avec `startup saved-configuration` ;
7. définir le correctif avec `startup patch` ;
8. contrôler de nouveau `display startup` ;
9. redémarrer et vérifier la version avec `display version`.

Ce scénario modifie le logiciel de l'équipement et ne doit être reproduit que dans un lab ou selon une procédure de changement validée.

## Quiz officiel

1. Combien d'utilisateurs peuvent se connecter simultanément par le port console ?
2. Comment spécifier le fichier à charger au prochain démarrage lorsque plusieurs fichiers de configuration existent ?

### Réponses

1. Un seul utilisateur ; l'identifiant de l'interface console est fixé à 0.
2. Avec `startup saved-configuration configuration-file`, en indiquant le nom et l'extension du fichier.

## Pratique officielle : Basic Operations on Huawei Network Devices

Le Lab Guide fait appliquer les opérations essentielles de cette unité :

- `display version` ;
- `system-view` et `sysname` ;
- navigation vers une interface ;
- aide en ligne et complétion Tab ;
- `display this` et `display current-configuration` ;
- `save` et sauvegarde vers un fichier nommé ;
- `dir`, `mkdir`, `copy`, `cd` et `pwd` ;
- `startup saved-configuration` et `display startup` ;
- redémarrage et restauration usine dans l'environnement contrôlé du lab.

## Diagnostic des erreurs courantes

| Problème | Méthode de vérification |
| --- | --- |
| commande refusée | vérifier la vue courante, le niveau de privilège et utiliser `?` |
| commande ambiguë | saisir davantage de caractères ou utiliser Tab |
| configuration apparemment absente | comparer `display this` et `display current-configuration` |
| configuration perdue après redémarrage | vérifier l'exécution de `save` et contrôler `display startup` |
| mauvais fichier chargé au démarrage | vérifier puis corriger `startup saved-configuration` |
| espace de stockage insuffisant | utiliser `dir`, supprimer ou sauvegarder les fichiers selon une procédure maîtrisée |
| mise à niveau non appliquée | vérifier le logiciel et le correctif du prochain démarrage avec `display startup` |

Les commandes de redémarrage, d'effacement de configuration et de remplacement logiciel ont un impact élevé. Elles doivent rester limitées au lab ou à une procédure de changement validée.

## À retenir pour l'examen HCIA-Datacom

- Le prompt permet d'identifier la vue de commande courante.
- `quit` remonte d'une vue ; `return` ou `Ctrl+Z` revient à la vue utilisateur.
- `?` et Tab réduisent les erreurs de syntaxe et évitent la mémorisation aveugle.
- `undo` restaure généralement une valeur par défaut, désactive une fonction ou retire une configuration.
- La configuration courante n'est pas automatiquement persistante.
- `save` enregistre la configuration ; `display startup` contrôle les fichiers du prochain démarrage.
- `.cfg`, `.zip`, `.dat` et `.defcfg` n'ont pas le même rôle.
- `reset saved-configuration`, `reset factory-configuration`, `reboot` et `reboot fast` doivent être utilisés avec prudence.

## Révision TianSemi

1. Comment reconnaître les vues utilisateur, système et interface à partir de leur prompt ?
2. Quelle différence existe entre `quit`, `return` et `Ctrl+Z` ?
3. Comment utiliser l'aide complète, l'aide partielle et la touche Tab ?
4. Que signifient les erreurs `Incomplete command`, `Ambiguous command` et `Wrong parameter` ?
5. Quelle différence faut-il établir entre configuration courante et configuration du prochain démarrage ?
6. Pourquoi faut-il exécuter `save` avant un redémarrage planifié ?
7. Quelles commandes permettent de vérifier la configuration active et les fichiers de démarrage ?
8. Pourquoi les opérations d'effacement et de mise à niveau doivent-elles être réalisées dans un environnement contrôlé ?

## Résumé fidèle du cours

- YunShan OS et VRP fournissent CLI et web UI pour configurer et administrer les équipements.
- La CLI organise les commandes par vues et niveaux de privilège.
- L'aide, l'historique, les raccourcis et `undo` facilitent l'utilisation correcte des commandes.
- Le système de fichiers stocke logiciels, correctifs et configurations.
- Une configuration courante doit être sauvegardée pour survivre au redémarrage.
- `display startup` permet de vérifier les logiciels et fichiers du prochain démarrage.

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 106 à 151.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 14-25.
- Huawei Talent, HCIA-Datacom V2.0 : https://e.huawei.com/en/talent/#/cert/product-details?certifiedProductId=1316&authenticationLevel=CTYPE_CARE_HCIA&technicalField=IIC&version=2.0
