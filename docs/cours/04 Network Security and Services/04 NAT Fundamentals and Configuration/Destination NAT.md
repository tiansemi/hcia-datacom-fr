---
id: HUA-HCIA-DC-055
title: "Destination NAT"
official_title: "Destination NAT"
course_unit: "4.4.3"
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
source_pages: "Training PDF 723-731; Lab PDF 169-171"
tags: [huawei, hcia, datacom, destination-nat, nat-server, port-forwarding, server-mapping]
prerequisites:
  - "[[Source NAT]]"
related:
  - "[[Bidirectional NAT]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Destination NAT

## Publier un service interne

Destination NAT remplace l'adresse destination publique d'un paquet entrant par l'adresse privée du serveur interne. Dans le sens retour, le routeur remplace la source privée par la valeur publique attendue par le client.

Huawei appelle **NAT Server** la forme statique utilisée pour publier une correspondance fixe.

![NAT Server publie un service interne avec une association adresse et port](<../../../assets/diagrammes/04 Network Security and Services/nat-server-flow-fr.svg>)

Exemple :

```text
1.1.1.1:8080  ⇄  192.168.1.1:80/TCP
```

Un utilisateur Internet contacte `1.1.1.1:8080`. Le routeur transmet la session au serveur `192.168.1.1:80`, puis réécrit la réponse dans le sens inverse.

## Association et session

La configuration NAT Server génère une entrée persistante dans la **server mapping table**. Le premier paquet correspondant crée ensuite une entrée dans la **session table**. Les paquets suivants utilisent directement la session sans rechercher de nouveau l'association statique.

Contrairement à source NAT dynamique, l'initiative vient ici d'Internet. La publication doit donc être volontaire, limitée au protocole et au port nécessaires, et accompagnée des contrôles de sécurité appropriés.

## Configuration du Training Material

```text
[R1] interface 10GE 0/0/2
[R1-10GE0/0/2] nat enable
[R1-10GE0/0/2] quit
[R1] nat server policy_web protocol tcp global 1.1.1.1 8080 inside 192.168.1.1 www route
[R1] ip route-static 0.0.0.0 0.0.0.0 1.1.1.254
```

La règle limite la publication à TCP et traduit le port public 8080 vers HTTP 80. Le paramètre `route` demande la génération de la route OPR prévue par le mécanisme.

Le Lab Guide fournit aussi un exemple utilisant l'adresse de l'interface :

```text
[R1] nat server for_telnet protocol tcp global 100.0.23.2 telnet inside 192.168.1.1 telnet
```

Telnet sert ici uniquement à valider la traduction dans le lab. Pour une publication réelle, choisissez un service chiffré et ne publiez que le port indispensable.

## Vérification

Pendant une connexion depuis l'extérieur :

```text
<R1> display session all verbose
```

Contrôlez :

- la source externe et son port ;
- l'adresse et le port publics demandés ;
- `New DestAddr` et `New DestPort` correspondant au serveur interne.

## Troubleshooting

| Symptôme | Contrôle |
| --- | --- |
| Aucun paquet n'atteint le routeur | Route du fournisseur vers l'adresse publique et filtrage amont. |
| Pas de traduction | `nat enable`, protocole, adresse et port publics de la règle. |
| Traduction correcte, serveur muet | Route interne, service en écoute et pare-feu du serveur. |
| Réponse avec mauvaise source | Session NAT et symétrie du chemin retour. |
| Application à ports dynamiques en échec | Besoin éventuel d'un ALG, selon le protocole. |

## À retenir pour l'examen HCIA-Datacom

- Destination NAT traduit la destination des paquets entrants.
- NAT Server maintient une association statique publique-privée.
- Le premier paquet crée une session ; les suivants utilisent cette session.
- Une règle peut traduire à la fois l'adresse et le port.
- Spécifier protocole et port limite l'exposition du serveur interne.
- La route amont vers l'adresse publique et le chemin retour sont indispensables.

## Révision TianSemi

1. Quel besoin distingue destination NAT de source NAT ?
2. Qu'est-ce qu'une association NAT Server ?
3. Quelle différence existe entre server mapping table et session table ?
4. Pourquoi faut-il limiter une publication au protocole et au port requis ?
5. Quels champs la règle d'exemple `1.1.1.1:8080 → 192.168.1.1:80` modifie-t-elle ?
6. Que faut-il vérifier si la traduction est visible mais que le serveur ne répond pas ?
7. Pourquoi le chemin retour doit-il repasser par le même équipement NAT ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 723 à 731.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 169 à 171, étape NAT Server du lab « NAT Configuration ».
