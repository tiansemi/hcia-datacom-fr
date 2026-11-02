---
id: HUA-HCIA-DC-030
title: "ICMP"
official_title: "ICMP"
course_unit: "3.1.4"
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
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 371-376"
tags: [huawei, hcia, datacom, icmp, ping, tracert, ttl]
prerequisites:
  - "[[Network Layer Protocols]]"
related:
  - "[[Subnetting]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# ICMP

## Fonction

**ICMP** transporte des informations de contrôle et des erreurs entre équipements. Ses messages sont encapsulés dans des paquets IP dont le champ Protocol vaut 1.

Le format comprend Type, Code, Checksum et un corps variable. Type indique la famille de message et Code précise la situation.

| Type / code du cours | Signification |
| --- | --- |
| 0 / 0 | Echo Reply |
| 3 / 0 | Network Unreachable |
| 3 / 1 | Host Unreachable |
| 3 / 2 | Protocol Unreachable |
| 3 / 3 | Port Unreachable |
| 5 / 0 | Redirect pour un réseau |
| 8 / 0 | Echo Request |

## Ping

![Ping vérifie la destination tandis que tracert révèle les sauts](<../../../assets/diagrammes/03 IP Technology Basics/icmp-ping-tracert-fr.svg>)

`ping` envoie des Echo Request et attend les Echo Reply. Il vérifie la joignabilité et mesure le temps aller-retour.

```text
<RTA> ping 20.0.0.2
```

Une réponse confirme un échange IP de bout en bout pour ces messages, mais ne prouve pas qu'un service applicatif précis est disponible.

## Rapports d'erreur

Un équipement peut signaler :

- réseau ou hôte inaccessible ;
- protocole ou port inaccessible ;
- TTL expiré à cause d'un chemin trop long ou d'une boucle.

Une politique peut empêcher l'émission ou le retour de certains messages ICMP. L'absence de réponse n'identifie donc pas à elle seule la cause.

## Tracert

```text
<RTA> tracert 20.0.0.2
```

`tracert` augmente progressivement le TTL : 1 pour révéler le premier routeur, 2 pour le suivant, et ainsi de suite. Chaque routeur où le TTL expire peut renvoyer une erreur ICMP, ce qui permet de reconstruire le chemin et d'estimer les délais.

## Méthode de diagnostic

1. Pinguer l'adresse locale ou loopback pour valider la pile.
2. Pinguer la passerelle du segment.
3. Pinguer une destination distante connue.
4. Utiliser `tracert` pour localiser le saut où les réponses cessent.
5. Comparer avec les routes et politiques de filtrage.

## À retenir pour l'examen HCIA-Datacom

- ICMP est encapsulé dans IP avec Protocol 1.
- Echo Request est type 8 ; Echo Reply type 0.
- Type 3 signale une destination inaccessible.
- `ping` teste la joignabilité et le RTT.
- `tracert` exploite l'expiration progressive du TTL.
- Un échec de ping peut provenir d'un filtrage ICMP, pas uniquement d'une panne réseau.

## Révision TianSemi

1. Quel est le rôle d'ICMP ?
2. Quelle valeur du champ IPv4 Protocol identifie ICMP ?
3. Quels types correspondent à Echo Request et Echo Reply ?
4. Que mesure `ping` ?
5. Comment `tracert` découvre-t-il chaque routeur du chemin ?
6. Pourquoi l'absence de réponse ICMP ne prouve-t-elle pas toujours une perte de connectivité ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 371 à 376.
