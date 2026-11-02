---
id: HUA-HCIA-DC-050
title: "FTP for File Transfer"
official_title: "FTP for File Transfer"
course_unit: "4.3.2"
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
source_pages: "Training PDF 638-654; Lab PDF 143-150"
tags: [huawei, hcia, datacom, ftp, file-transfer, active-mode, passive-mode, aaa]
prerequisites:
  - "[[Overview of Network Services]]"
related:
  - "[[SSH]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# FTP pour le transfert de fichiers (FTP for File Transfer)

## Rôle et architecture

FTP est un protocole applicatif client/serveur destiné au transfert et à la gestion de fichiers. Sur un équipement Huawei :

- le client se connecte au serveur et exécute des opérations sur les fichiers et répertoires ;
- le serveur stocke les fichiers et attend les demandes de connexion.

Le support cite notamment la mise à niveau logicielle et la sauvegarde de configuration. FTP transmet cependant les identifiants et les données en clair ; **SFTP est le choix recommandé sur un réseau réel**.

## Modes de transfert

| Mode | Usage |
| --- | --- |
| ASCII | Fichiers texte nécessitant éventuellement une adaptation des caractères de fin de ligne. |
| Binaire | Logiciels système, archives, images, vidéos, bases de données et tout contenu qui ne doit pas être transformé. |

Le Lab Guide indique ASCII comme mode par défaut et l'utilise pour les fichiers de configuration. Le Training Material utilise le mode binaire pour un paquet logiciel et une archive de configuration. Il faut donc choisir selon le fichier et vérifier son intégrité après transfert.

## Deux connexions TCP

![Connexions de contrôle et de données en FTP actif et passif](<../../../assets/diagrammes/04 Network Security and Services/ftp-active-passive-fr.svg>)

FTP maintient :

- une **connexion de contrôle** vers TCP 21, ouverte pendant la session ;
- une **connexion de données** distincte, créée pour les transferts ou listes.

### Mode actif

Le client ouvre la connexion de contrôle, annonce un port de données avec `PORT`, puis le serveur initie la connexion de données depuis TCP 20 vers le client. C'est le mode par défaut indiqué dans le Lab Guide.

### Mode passif

Le client envoie `PASV`. Le serveur annonce un port disponible et le client initie également la connexion de données. Ce fonctionnement traverse généralement mieux les filtrages placés devant le client.

## Configuration du serveur Huawei

Dans un lab isolé :

```text
[R1] ftp server enable
[R1] ftp server source -i GE 0/0/1
[R1] aaa
[R1-aaa] local-user ftp-client password irreversible-cipher <mot-de-passe-robuste>
[R1-aaa] local-user ftp-client service-type ftp
[R1-aaa] local-user ftp-client privilege level 3
[R1-aaa] local-user ftp-client ftp-directory flash:/
```

Spécifier une interface ou une adresse source réduit les points d'entrée. Le Training Material exige un niveau de gestion 3 pour l'utilisateur FTP présenté.

Une ACL peut limiter les clients autorisés :

```text
[R1] acl number 2001
[R1-acl4-basic-2001] rule 5 permit source 10.0.12.1 0
[R1-acl4-basic-2001] rule 10 deny
[R1-acl4-basic-2001] quit
[R1] ftp server acl 2001
```

## Mini-lab TianSemi : gérer une configuration

Topologie du Lab Guide : S1 client `10.0.12.1/24`, R1 serveur `10.0.12.2/24`.

1. Sauvegardez `test1.cfg` sur S1 et `test2.cfg` sur R1.
2. Configurez R1 comme serveur FTP et créez l'utilisateur limité au répertoire prévu.
3. Depuis S1, connectez-vous :

```text
<S1> ftp 10.0.12.2
```

4. Dans la vue FTP, exercez les opérations :

```text
[ftp] ascii
[ftp] get test2.cfg
[ftp] put test1.cfg
[ftp] bye
```

5. Vérifiez sur chaque équipement :

```text
<S1> dir | include test
<R1> dir | include test
```

Ne supprimez un fichier qu'après sauvegarde et validation du chemin. Pour une mise en pratique moderne, reproduisez ensuite l'objectif avec SFTP.

## Troubleshooting

| Symptôme | Vérification |
| --- | --- |
| TCP 21 inaccessible | Adressage, routage, serveur FTP, interface source et ACL. |
| Authentification refusée | Compte AAA, mot de passe, `service-type ftp` et état du compte. |
| Connexion acceptée mais accès impossible | Répertoire FTP autorisé et niveau de privilège. |
| Liste ou transfert bloqué | Mode actif/passif et filtrage de la connexion de données. |
| Fichier corrompu | Mode ASCII/binaire, espace disponible et intégrité du fichier. |

## À retenir pour l'examen HCIA-Datacom

- FTP utilise une connexion de contrôle TCP 21 et une connexion de données séparée.
- En mode actif, le serveur initie la connexion de données ; en mode passif, le client l'initie.
- Le mode actif est le mode par défaut mentionné dans le Lab Guide.
- ASCII convient aux textes ; binaire préserve les octets des autres fichiers.
- L'utilisateur FTP local doit être associé au service FTP et à un répertoire autorisé.
- FTP n'est pas sécurisé ; SFTP est recommandé.

## Révision TianSemi

1. Pourquoi FTP utilise-t-il deux connexions TCP ?
2. Quel port porte la connexion de contrôle ?
3. Qui initie la connexion de données en mode actif ?
4. Pourquoi le mode passif traverse-t-il souvent mieux un pare-feu côté client ?
5. Quand faut-il sélectionner le mode binaire ?
6. Quelles propriétés AAA sont indispensables à un utilisateur FTP Huawei ?
7. Pourquoi faut-il préférer SFTP sur un réseau de production ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 638 à 654.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 143 à 150, lab « File Management Configuration ».
