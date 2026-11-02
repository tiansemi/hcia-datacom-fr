---
id: HUA-HCIA-DC-048
title: "Configuration Examples for AAA"
official_title: "Configuration Examples for AAA"
course_unit: "4.2.3"
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
source_pages: "Training PDF 609-622; Lab PDF 151-163"
tags: [huawei, hcia, datacom, aaa, local-user, radius, stelnet, vty, remote-login]
prerequisites:
  - "[[Deploying AAA]]"
related:
  - "[[Overview of Network Services]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Exemples de configuration AAA (Configuration Examples for AAA)

## Configuration locale d'un administrateur

La configuration minimale associe trois propriétés : une identité locale, un type de service autorisé et un niveau de privilège.

```text
[SW1] aaa
[SW1-aaa] local-user huawei password irreversible-cipher <mot-de-passe-robuste>
[SW1-aaa] local-user huawei service-type ssh
[SW1-aaa] local-user huawei privilege level 3
```

`irreversible-cipher` évite de conserver le secret en clair. Le type de service limite les modes de connexion acceptés. Le niveau, compris entre 0 et 3 dans le support, borne les commandes disponibles.

Vérification :

```text
[SW1] display local-user username huawei
```

La sortie doit notamment confirmer l'état actif, le masque de service et le niveau de privilège.

## Configuration RADIUS

![Chaîne de configuration d'un accès AAA avec RADIUS](<../../../assets/diagrammes/04 Network Security and Services/aaa-radius-configuration-chain-fr.svg>)

Le cours suit quatre associations : serveur RADIUS, schémas AAA, domaine, puis utilisateur se connectant à ce domaine.

### 1. Déclarer le serveur

```text
[SW1] radius-server template radius1
[SW1-radius-radius1] radius-server authentication 192.168.1.1 1812
[SW1-radius-radius1] radius-server accounting 192.168.1.1 1813
[SW1-radius-radius1] radius-server shared-key cipher <cle-partagee-robuste>
[SW1-radius-radius1] quit
```

La même clé doit être configurée côté équipement et côté serveur. Elle doit être protégée et renouvelée périodiquement.

### 2. Créer les schémas

```text
[SW1] aaa
[SW1-aaa] authentication-scheme auth1
[SW1-aaa-authen-auth1] authentication-mode radius
[SW1-aaa-authen-auth1] quit
[SW1-aaa] accounting-scheme acc1
[SW1-aaa-accounting-acc1] accounting-mode radius
[SW1-aaa-accounting-acc1] quit
```

### 3. Lier le tout au domaine

```text
[SW1-aaa] domain huawei.com
[SW1-aaa-domain-huawei.com] authentication-scheme auth1
[SW1-aaa-domain-huawei.com] accounting-scheme acc1
[SW1-aaa-domain-huawei.com] radius-server radius1
[SW1-aaa-domain-huawei.com] quit
```

### 4. Vérifier les associations

```text
[SW1] display radius-server configuration template radius1
[SW1] display domain name huawei.com
```

Contrôler l'adresse, les ports, l'état du serveur, les noms de schémas et le modèle RADIUS lié au domaine.

## Mini-lab TianSemi : AAA local et accès distant

Le Lab Guide utilise S1 comme client et R1 comme équipement administré. Les interfaces de liaison reçoivent `10.0.12.1/24` et `10.0.12.2/24`.

### Objectifs

- créer des schémas locaux ;
- les appliquer au domaine `datacom` ;
- créer un utilisateur `hcia@datacom` ;
- valider l'accès, puis observer la session et le compte.

### Configuration AAA sur R1

```text
[R1] aaa
[R1-aaa] authentication-scheme datacom
[R1-aaa-authen-datacom] authentication-mode local
[R1-aaa-authen-datacom] quit
[R1-aaa] authorization-scheme datacom
[R1-aaa-author-datacom] authorization-mode local
[R1-aaa-author-datacom] quit
[R1-aaa] domain datacom
[R1-aaa-domain-datacom] authentication-scheme datacom
[R1-aaa-domain-datacom] authorization-scheme datacom
[R1-aaa-domain-datacom] quit
[R1-aaa] local-user hcia@datacom password irreversible-cipher <mot-de-passe-robuste>
[R1-aaa] local-user hcia@datacom service-type ssh
[R1-aaa] local-user hcia@datacom privilege level 3
```

### Accès distant sécurisé

Le lab officiel commence par Telnet pour illustrer le mécanisme, puis demande de passer à **STelnet**, fondé sur SSH. En pratique, ne déployez pas Telnet : il transmet les comptes, mots de passe et commandes sans chiffrement.

```text
[R1] ssh user hcia@datacom
[R1] ssh user hcia@datacom authentication-type password
[R1] ssh user hcia@datacom service-type stelnet
[R1] stelnet server enable
[R1] user-interface vty 0 4
[R1-ui-vty0-4] authentication-mode aaa
[R1-ui-vty0-4] protocol inbound ssh
```

Adaptez la commande de source du serveur SSH à une interface ou une adresse de gestion maîtrisée. L'option `all-interface` du lab élargit la surface d'écoute et n'est pas un choix de production par défaut.

### Vérifications

Depuis S1 :

```text
<S1> stelnet 10.0.12.2
```

Sur R1 :

```text
<R1> display users
<R1> display aaa configuration
<R1> display local-user username hcia@datacom
<R1> display ssh server ip-block all
```

La session doit apparaître avec le protocole SSH, l'adresse du client et le nom complet de l'utilisateur.

## Troubleshooting

| Symptôme | Contrôle prioritaire |
| --- | --- |
| Compte refusé | Domaine du nom, état du compte, mot de passe et méthode d'authentification. |
| Connexion impossible | Adressage, routage, serveur STelnet, source d'écoute et VTY. |
| Authentification réussie mais commande refusée | Niveau de privilège ou autorisation renvoyée. |
| RADIUS sans réponse | Joignabilité, ports 1812/1813, clé partagée et état du serveur. |
| Compte bloqué | `display local-user state block` et mécanismes de blocage VTY/SSH. |

Ne désactivez pas durablement les protections de blocage pour contourner un diagnostic. Corrigez la cause, puis réactivez explicitement le compte ou l'adresse si nécessaire.

## À retenir pour l'examen HCIA-Datacom

- Un utilisateur local doit disposer d'un mot de passe, d'un type de service adapté et d'un niveau de privilège.
- Un déploiement RADIUS associe modèle de serveur, schémas AAA et domaine.
- `display local-user`, `display domain` et `display radius-server configuration` valident des couches différentes.
- L'authentification VTY doit être réglée sur AAA pour utiliser les comptes AAA.
- STelnet chiffre la session ; Telnet ne doit servir que dans un lab isolé lorsque le support l'exige.
- Un nom contenant `@` permet de sélectionner explicitement le domaine.

## Révision TianSemi

1. Quelles trois propriétés faut-il associer à un administrateur local ?
2. Quel objet contient l'adresse et la clé partagée du serveur RADIUS ?
3. Où les schémas et le modèle RADIUS sont-ils liés ?
4. Quelles commandes permettent de vérifier le modèle RADIUS et le domaine ?
5. Pourquoi `authentication-mode aaa` est-il requis sous les lignes VTY ?
6. Pourquoi STelnet doit-il remplacer Telnet en production ?
7. Quels éléments faut-il vérifier lorsqu'un serveur RADIUS ne répond pas ?
8. Quelle information de `display users` confirme que la connexion utilise SSH ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 609 à 622.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 151 à 163, lab « Remote Login Configuration ».
