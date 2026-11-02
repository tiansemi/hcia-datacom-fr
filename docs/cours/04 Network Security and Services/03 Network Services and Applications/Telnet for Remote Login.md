---
id: HUA-HCIA-DC-051
title: "Telnet for Remote Login"
official_title: "Telnet for Remote Login"
course_unit: "4.3.3"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-11
updated: 2026-08-11
source: "HCIA-Datacom V2.0 Training Material; HCIA-Datacom V2.0 Lab Guide"
source_pages: "Training PDF 655-666; Lab PDF 151-157"
tags: [huawei, hcia, datacom, telnet, remote-login, vty, aaa, insecure-protocol]
prerequisites:
  - "[[FTP for File Transfer]]"
related:
  - "[[SSH]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Telnet pour l'accès distant (Telnet for Remote Login)

## Ce que Telnet apporte

Telnet permet d'utiliser à distance la ligne de commande d'un équipement sans câble console. Il suit un modèle client/serveur et utilise TCP 23 par défaut.

La connexion nécessite :

- une connectivité IP entre client et serveur ;
- un service Telnet actif et joignable ;
- une interface utilisateur virtuelle disponible ;
- un mécanisme d'authentification ;
- des droits suffisants après connexion.

## La VTY représente la session distante

![Une connexion Telnet est associée à une interface VTY mais reste en clair](<../../../assets/diagrammes/04 Network Security and Services/telnet-vty-security-fr.svg>)

Lorsqu'un utilisateur se connecte par Telnet ou SSH, le système lui attribue la VTY disponible ayant le plus petit numéro. Selon le modèle et la version, jusqu'à 21 VTY numérotées de 0 à 20 peuvent exister.

Les paramètres de la VTY déterminent notamment :

- le mode d'authentification ;
- les protocoles entrants acceptés ;
- le niveau de privilège dans le cas d'une authentification par simple mot de passe.

Avec `authentication-mode aaa`, le niveau configuré pour l'utilisateur AAA prévaut.

## Configuration étudiée dans le cours

Le Training Material présente la séquence suivante à des fins d'apprentissage :

```text
[R2] telnet server enable
[R2] telnet server source -i GE 0/0/1
[R2] user-interface vty 0 4
[R2-ui-vty0-4] authentication-mode aaa
[R2-ui-vty0-4] protocol inbound telnet
[R2] aaa
[R2-aaa] local-user admin1 password irreversible-cipher <mot-de-passe-robuste>
[R2-aaa] local-user admin1 service-type telnet
[R2-aaa] local-user admin1 privilege level 3
```

Une ACL appliquée en entrée sur les VTY peut restreindre l'adresse du client :

```text
[R2] user-interface vty 0 4
[R2-ui-vty0-4] acl 2000 inbound
```

Le client se connecte avec :

```text
<R1> telnet 192.168.1.2
```

Si le serveur écoute sur un port différent de 23, le client doit préciser ce port.

## Limite de sécurité déterminante

Telnet ne chiffre ni les identifiants ni les commandes. Un observateur placé sur le chemin peut lire ou modifier la session. Une ACL réduit les sources admises mais ne fournit aucun chiffrement.

Le Lab Guide et le Training Material signalent explicitement ce risque et recommandent **STelnet**. Le paquet WEAKEA requis par certaines versions sert à reproduire le lab ; son installation ne rend pas Telnet sûr.

## Pratique encadrée

Si l'objectif pédagogique impose Telnet, utilisez uniquement un environnement isolé sans données réelles :

1. configurez R1 client et R2 serveur sur un lien dédié ;
2. limitez l'accès à l'adresse de R1 avec une ACL ;
3. capturez la session pour constater le trafic en clair ;
4. remplacez immédiatement Telnet par STelnet ;
5. comparez les deux captures.

## Troubleshooting

| Symptôme | Contrôle |
| --- | --- |
| Pas de connexion TCP | IP, routage, port, service et interface source. |
| Connexion rejetée | ACL appliquée aux VTY et plage VTY disponible. |
| Identifiants refusés | Mode AAA, compte, type de service et domaine. |
| Commandes refusées | Niveau du compte AAA. |
| Lab impossible sur une version récente | Présence éventuelle du paquet WEAKEA, uniquement en environnement isolé. |

## À retenir pour l'examen HCIA-Datacom

- Telnet est un protocole applicatif client/serveur utilisant TCP 23 par défaut.
- Une session distante utilise une interface VTY.
- AAA exige un nom d'utilisateur et un mot de passe.
- Avec AAA, le privilège du compte local prévaut sur celui de la VTY.
- Une ACL VTY peut limiter les sources mais ne chiffre pas la session.
- Telnet est non sûr ; STelnet est la solution recommandée.

## Révision TianSemi

1. Quel port TCP Telnet utilise-t-il par défaut ?
2. Quel est le rôle d'une interface VTY ?
3. Comment le système choisit-il une VTY lors d'une nouvelle connexion ?
4. Quelle configuration relie les VTY aux comptes AAA ?
5. Que protège une ACL appliquée en entrée sur une VTY ?
6. Pourquoi cette ACL ne suffit-elle pas à sécuriser Telnet ?
7. Quel service doit remplacer Telnet sur un réseau réel ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 655 à 666.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 151 à 157, début du lab « Remote Login Configuration ».
