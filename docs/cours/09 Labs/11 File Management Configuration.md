---
id: HUA-HCIA-LAB-011
title: "File Management Configuration"
official_title: "File Management Configuration"
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
source_pages: "PDF 143-150 (guide pages 141-148)"
lab_required: true
tags: [huawei, hcia, datacom, lab, ftp, file-management, aaa, configuration-backup]
devices: [R1, S1]
commands: [ftp server enable, local-user, ftp, get, put, dir]
publication_status: "not-started"
site_url:
---

# Lab 11 — Gestion de fichiers par FTP

## Objectif et topologie

Configurer R1 comme serveur FTP, S1 comme client, télécharger `test2.cfg` depuis R1, envoyer `test1.cfg` vers R1 et vérifier les fichiers aux deux extrémités.

![S1 client FTP relié à R1 serveur FTP](<../../assets/diagrammes/09 Labs/lab11-file-transfer-topology-fr.svg>)

S1 VLANIF2 utilise `10.0.12.1/24` ; R1 `GE0/0/1` utilise `10.0.12.2/24`.

> [!warning] FTP transmet identifiants et données sans chiffrement. Le paquet WEAKEA et FTP ne sont acceptables ici que dans un réseau de lab isolé. En production, privilégiez SFTP/SCP et une interface source précise.

## Préparation

```text
<S1> save test1.cfg
<R1> save test2.cfg
<S1> dir
<R1> dir
```

## Serveur R1 et compte AAA

```text
<R1> install feature-software WEAKEA
[R1] ftp server enable
[R1] ftp server source all-interface
[R1] aaa
[R1-aaa] local-user ftp-client password irreversible-cipher <SECRET-LAB>
[R1-aaa] local-user ftp-client service-type ftp
[R1-aaa] local-user ftp-client privilege level 3
[R1-aaa] local-user ftp-client ftp-directory flash:/
```

`source all-interface` élargit l’exposition ; il reproduit le guide mais doit être remplacé par une source de gestion dédiée en production.

## Transfert depuis S1

```text
<S1> ftp 10.0.12.2
[ftp] ascii
[ftp] get test2.cfg
[ftp] put test1.cfg
[ftp] bye
```

Le guide montre aussi `delete test2.cfg` sur le serveur. Exécutez cette suppression seulement après avoir prouvé le téléchargement et uniquement sur le fichier créé pour le lab.

## Vérifications attendues

```text
<S1> dir | include test
<R1> dir | include test
```

- S1 contient `test1.cfg` et la copie téléchargée `test2.cfg`.
- R1 contient `test1.cfg` envoyé par S1 ; `test2.cfg` dépend du test de suppression.
- La session se ferme proprement avec `bye`.

## Troubleshooting

- Connexion refusée : vérifier WEAKEA, `ftp server enable` et source serveur.
- Authentification refusée : contrôler service-type, secret et domaine AAA.
- `get/put` refusé : vérifier privilège, répertoire FTP et espace flash.
- Mauvais fichier : exécuter `pwd`, `dir` et confirmer le nom avant transfert ou suppression.

## Révision TianSemi

1. Quel équipement est serveur et lequel est client ?
2. À quoi sert `ftp-directory flash:/` ?
3. Comment prouver les deux sens du transfert ?
4. Pourquoi FTP et WEAKEA sont-ils limités au lab ?
5. Quel risque introduit `source all-interface` ?
6. Quelle solution chiffrée faut-il préférer en production ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 143 à 150 (pages 141 à 148 du guide).
