---
id: HUA-HCIA-DC-052
title: "SSH"
official_title: "SSH"
course_unit: "4.3.4"
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
source_pages: "Training PDF 667-690; Lab PDF 157-163"
tags: [huawei, hcia, datacom, ssh, stelnet, sftp, encryption, authentication, vty]
prerequisites:
  - "[[Telnet for Remote Login]]"
related:
  - "[[NAT Overview]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# SSH

## Canal sécurisé sur un réseau non fiable

SSH fournit un accès distant et un transfert de fichiers sécurisés grâce au chiffrement et à l'authentification. Il établit un canal au-dessus de TCP, généralement sur le port 22.

Huawei présente deux services fondés sur SSH 2.0 :

- **STelnet** pour l'administration distante en ligne de commande ;
- **SFTP** pour la gestion et le transfert sécurisés de fichiers.

SFTP n'est pas FTP « avec une option de chiffrement » : c'est un protocole distinct transporté comme sous-système SSH et utilisant une seule connexion TCP pour commandes et données.

## Trois composants du protocole

| Composant | Rôle |
| --- | --- |
| Protocole de transport SSH | Négociation de version et d'algorithmes, échange de clés, chiffrement et intégrité. |
| Protocole d'authentification utilisateur | Vérification du client par mot de passe, clé publique ou combinaison. |
| Protocole de connexion | Création de canaux logiques pour shell, commandes, SFTP ou redirection TCP. |

## Étapes d'une session

![Étapes de l'établissement et de l'utilisation d'une session SSH](<../../../assets/diagrammes/04 Network Security and Services/ssh-session-stages-fr.svg>)

1. Le client ouvre une connexion TCP vers le serveur.
2. Les deux extrémités négocient la version SSH.
3. Elles choisissent les algorithmes compatibles : échange de clés, chiffrement, clé publique et HMAC.
4. L'échange Diffie-Hellman produit une clé de session et le client authentifie le serveur.
5. Le serveur authentifie l'utilisateur.
6. Le client demande un service, par exemple STelnet ou SFTP.
7. Les données circulent dans le canal chiffré.

## Configuration d'un serveur STelnet

### VTY et compte AAA

```text
[R2] user-interface vty 0 4
[R2-ui-vty0-4] authentication-mode aaa
[R2-ui-vty0-4] protocol inbound ssh
[R2-ui-vty0-4] quit
[R2] aaa
[R2-aaa] local-user admin1 password irreversible-cipher <mot-de-passe-robuste>
[R2-aaa] local-user admin1 service-type ssh
[R2-aaa] local-user admin1 privilege level 3
[R2-aaa] quit
```

Sans `authentication-mode aaa`, la commande `protocol inbound ssh` ne produit pas l'effet attendu dans le scénario du cours.

### Utilisateur et service SSH

```text
[R2] ssh user admin1
[R2] ssh user admin1 authentication-type password
[R2] ssh user admin1 service-type stelnet
[R2] stelnet server enable
[R2] ssh server-source -i GE 0/0/1
```

L'authentification SSH par mot de passe repose sur un utilisateur AAA du même nom. L'authentification par clé exige les paires de clés et l'enregistrement de la clé publique du pair.

### Algorithmes

Le Training Material présente des listes modernes incluant AES-GCM/AES-CTR, SHA-2, `curve25519_sha256`, `dh_group16_sha512` et RSA-SHA2/ECC. N'activez pas les anciens algorithmes simplement parce qu'ils apparaissent dans la syntaxe disponible ; conservez seulement les choix compatibles avec la politique de sécurité et la version logicielle.

## Première connexion du client

```text
[R1] ssh client first-time enable
<R1> stelnet 192.168.1.2
```

Le mode « first-time » accepte et enregistre la clé présentée lors de la première connexion. Il facilite le lab, mais l'administrateur doit idéalement vérifier l'empreinte du serveur par un canal fiable avant de l'accepter.

Si le serveur utilise un port non standard :

```text
<R1> stelnet 192.168.1.2 1025
```

## Mini-lab TianSemi : migrer de Telnet à STelnet

1. Reprenez la topologie R1 `10.0.12.2/24`, S1 `10.0.12.1/24` du Lab Guide.
2. Créez un compte AAA de service `ssh` et un utilisateur SSH de service `stelnet`.
3. Autorisez uniquement SSH sur les VTY.
4. Limitez la source d'écoute à l'interface de gestion.
5. Connectez-vous depuis S1, contrôlez l'empreinte puis changez le mot de passe initial.
6. Vérifiez :

```text
<R1> display ssh server status
<R1> display ssh user-information
<R1> display users
<R1> display ssh server ip-block all
```

7. Confirmez que Telnet n'est plus accepté et que la session observée utilise SSH.

## Troubleshooting

| Symptôme | Contrôle |
| --- | --- |
| TCP inaccessible | Adressage, routage, port SSH, source d'écoute et ACL. |
| Aucun algorithme commun | Listes de chiffrement, HMAC, échange de clés et clé publique des deux côtés. |
| Serveur non authentifié | Empreinte et clé publique enregistrée côté client. |
| Mot de passe rejeté | Utilisateur SSH, compte AAA homonyme, service `ssh` et état du compte. |
| Session ouverte mais STelnet refusé | `stelnet server enable` et `service-type stelnet`. |
| VTY ne prend pas SSH | `authentication-mode aaa` et `protocol inbound ssh`. |

## À retenir pour l'examen HCIA-Datacom

- SSH chiffre la communication et authentifie les pairs.
- Le port SSH par défaut est TCP 22.
- SSH comporte protocoles de transport, d'authentification utilisateur et de connexion.
- SFTP et STelnet reposent sur SSH 2.0.
- Une authentification SSH par mot de passe nécessite un utilisateur AAA correspondant.
- Les VTY doivent utiliser AAA et accepter SSH.
- La compatibilité cryptographique exige au moins un algorithme commun par catégorie.

## Révision TianSemi

1. Quels risques de Telnet et FTP SSH corrige-t-il ?
2. Quels sont les trois composants de SSH ?
3. À quel moment la clé de session est-elle produite ?
4. Quelle différence fondamentale existe entre FTP et SFTP ?
5. Pourquoi faut-il un compte AAA homonyme avec l'authentification SSH par mot de passe ?
6. Quelles commandes activent STelnet et l'autorisent sur les VTY ?
7. Pourquoi faut-il vérifier l'empreinte lors de la première connexion ?
8. Quelles familles d'algorithmes le client et le serveur doivent-ils négocier ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 667 à 690.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 157 à 163, fin du lab « Remote Login Configuration ».
