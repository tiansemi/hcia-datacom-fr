---
id: HUA-HCIA-LAB-012
title: "Remote Login Configuration"
official_title: "Remote Login Configuration"
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
source_pages: "PDF 151-163 (guide pages 149-161)"
lab_required: true
tags: [huawei, hcia, datacom, lab, aaa, telnet, stelnet, ssh, remote-management]
devices: [R1, S1]
commands: [authentication-scheme, authorization-scheme, domain, local-user, user-interface vty, stelnet]
publication_status: "not-started"
site_url:
---

# Lab 12 — Connexion distante avec AAA, Telnet et STelnet

## Objectif et topologie

Mettre en place une authentification/autorisation AAA locale sur R1, tester Telnet pour observer ses limites, puis configurer STelnet/SSH et administrer les utilisateurs distants.

![S1 administre R1 par Telnet puis STelnet](<../../assets/diagrammes/09 Labs/lab12-remote-login-topology-fr.svg>)

S1 `10.0.12.1/24` se connecte à R1 `10.0.12.2/24`.

## 1 — Construire AAA local et le domaine

```text
[R1] aaa
[R1-aaa] authentication-scheme datacom
[R1-aaa-authen-datacom] authentication-mode local
[R1-aaa] authorization-scheme datacom
[R1-aaa-author-datacom] authorization-mode local
[R1-aaa] domain datacom
[R1-aaa-domain-datacom] authentication-scheme datacom
[R1-aaa-domain-datacom] authorization-scheme datacom
[R1-aaa] local-user hcia@datacom password irreversible-cipher <SECRET-LAB>
[R1-aaa] local-user hcia@datacom service-type telnet
[R1-aaa] local-user hcia@datacom privilege level 3
```

Le suffixe `@datacom` associe l’utilisateur au domaine. Le niveau 3 autorise les opérations prévues par le lab ; appliquez le moindre privilège en production.

## 2 — Observer Telnet uniquement en lab

```text
<R1> install feature-software WEAKEA
[R1] telnet server enable
[R1] telnet server-source all-interface
[R1] user-interface vty 0 4
[R1-ui-vty0-4] authentication-mode aaa
[R1-ui-vty0-4] protocol inbound telnet
<S1> telnet 10.0.12.2
```

Telnet ne chiffre pas la session. Cette étape sert seulement de comparaison et doit être supprimée après le lab.

## 3 — Configurer STelnet

```text
[R1] ssh user sshclient@datacom
[R1] ssh user sshclient@datacom authentication-type password
[R1] ssh user sshclient@datacom service-type stelnet
[R1] aaa
[R1-aaa] local-user sshclient@datacom password irreversible-cipher <SECRET-LAB>
[R1-aaa] local-user sshclient@datacom privilege level 3
[R1-aaa] local-user sshclient@datacom service-type ssh
[R1] ssh server-source all-interface
[R1] stelnet server enable
[R1] user-interface vty 5 10
[R1-ui-vty5-10] authentication-mode aaa
[R1-ui-vty5-10] protocol inbound ssh

[S1] ssh client first-time enable
<S1> stelnet 10.0.12.2
```

En production, validez l’empreinte du serveur plutôt que d’accepter automatiquement la première clé.

## Vérification et gestion

```text
<R1> display users
<R1> display aaa configuration
<R1> display local-user username hcia@datacom
<R1> display vty ip-block all
<R1> display ssh server ip-block all
```

Vérifiez le protocole de chaque session, le domaine, le niveau de privilège et les blocages issus d’échecs répétés.

## Troubleshooting

- Login refusé : vérifier domaine, utilisateur, service-type et schéma local.
- Connexion SSH refusée : contrôler utilisateur SSH, compte AAA, STelnet et VTY `protocol inbound ssh`.
- IP bloquée : lire les tables de blocage, corriger la cause, puis déverrouiller de façon contrôlée.
- Accès trop large : remplacer `all-interface` par une source de gestion et ajouter une ACL.

## Révision TianSemi

1. Quels rôles assurent authentification et autorisation ?
2. Comment le suffixe `@datacom` sélectionne-t-il le domaine ?
3. Pourquoi le compte SSH existe-t-il à la fois dans SSH et AAA ?
4. Pourquoi STelnet est-il préférable à Telnet ?
5. Que contrôlent les vues VTY ?
6. Comment diagnostiquer un blocage après plusieurs échecs ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 151 à 163 (pages 149 à 161 du guide).
