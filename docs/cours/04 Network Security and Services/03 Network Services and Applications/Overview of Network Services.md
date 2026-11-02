---
id: HUA-HCIA-DC-049
title: "Overview of Network Services"
official_title: "Overview of Network Services"
course_unit: "4.3.1"
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
source_pages: "PDF 624-637"
tags: [huawei, hcia, datacom, tcp, udp, transport-layer, port, three-way-handshake]
prerequisites:
  - "[[Configuration Examples for AAA]]"
related:
  - "[[FTP for File Transfer]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble des services réseau (Overview of Network Services)

## Des applications au transport

Les protocoles applicatifs définissent les règles d'échange propres à un service, par exemple HTTP pour le Web ou DNS pour la résolution de noms. Ils s'appuient sur les capacités de communication de bout en bout fournies par la couche transport.

Huawei oppose principalement :

- **TCP**, orienté connexion et conçu pour livrer les données de façon fiable et ordonnée ;
- **UDP**, sans connexion et à acheminement au mieux, avec moins de surcharge et de latence.

![Comparaison opérationnelle de TCP et UDP](<../../../assets/diagrammes/04 Network Security and Services/tcp-vs-udp-fr.svg>)

| Critère | TCP | UDP |
| --- | --- | --- |
| Connexion préalable | Oui | Non |
| Fiabilité | Accusés de réception, retransmission, séquencement | Non garantie par le protocole |
| Ordre des données | Préservé | Non garanti |
| Surcharge | Plus élevée | Faible |
| Usages du support | HTTP/HTTPS, FTP, SMTP, Telnet/SSH | Voix, vidéo, jeu en ligne, requêtes DNS, diffusion |

Une application utilisant UDP peut elle-même ajouter les mécanismes dont elle a besoin. « Non fiable » ne signifie donc pas « inutilisable », mais « sans garantie intégrée comparable à TCP ».

## Ports source et destination

TCP et UDP utilisent des numéros de port sur 16 bits, de 0 à 65535, pour identifier les applications émettrice et réceptrice.

- le client choisit généralement un port source dynamique inutilisé, souvent supérieur à 1023 ;
- le serveur écoute sur le port associé à son service ;
- l'adresse IP identifie l'hôte, tandis que le port identifie le processus applicatif sur cet hôte.

Exemples du cours : HTTP sur TCP 80, Telnet sur TCP 23 et DNS sur UDP 53.

## En-têtes TCP et UDP

L'en-tête TCP minimal mesure 20 octets et contient notamment les ports, les numéros de séquence et d'acquittement, les indicateurs de contrôle, la fenêtre et la somme de contrôle. Les options peuvent l'allonger jusqu'à 60 octets.

L'en-tête UDP mesure 8 octets : ports source et destination, longueur et somme de contrôle. Cette simplicité contribue à sa faible surcharge.

## Établissement d'une connexion TCP

Avant de transmettre les données, TCP réalise un **three-way handshake** :

1. le client envoie `SYN` avec une séquence initiale `a` ;
2. le serveur répond `SYN, ACK` avec sa séquence `b` et `Ack = a + 1` ;
3. le client confirme avec `ACK` et `Ack = b + 1`.

La connexion bidirectionnelle est alors établie.

## Fiabilité pendant le transfert

Chaque octet du flux TCP reçoit un numéro de séquence. L'accusé de réception indique le numéro du prochain octet attendu.

Si un segment commence à `Seq = a + 1` et contient 12 octets, le récepteur répond avec `Ack = a + 13`. L'émetteur sait ainsi quels octets ont été reçus. TCP peut envoyer plusieurs segments avant un acquittement cumulatif.

## Fermeture de la connexion

TCP étant full-duplex, chaque direction doit être fermée. Le cas standard utilise quatre échanges : `FIN`, `ACK`, `FIN`, `ACK`. Chaque extrémité annonce séparément qu'elle n'a plus de données à transmettre.

## Pratique TianSemi : observer une session

Dans Wireshark, lancez une connexion HTTPS ou SSH et filtrez `tcp`. Identifiez :

1. les trois segments du handshake ;
2. les ports client et serveur ;
3. un couple `Seq`/`Ack` pendant le transfert ;
4. la fermeture de la connexion.

Pour UDP, lancez une résolution DNS, filtrez `udp.port == 53` et vérifiez qu'aucun handshake transport ne précède la requête.

## À retenir pour l'examen HCIA-Datacom

- TCP est orienté connexion, fiable et ordonné ; UDP est sans connexion et best effort.
- Les ports identifient les applications de chaque extrémité.
- Les numéros de port utilisent 16 bits.
- L'en-tête TCP minimal fait 20 octets, celui d'UDP 8 octets.
- TCP établit la connexion en trois échanges et la ferme généralement en quatre.
- `Seq` repère le premier octet du segment et `Ack` le prochain octet attendu.

## Révision TianSemi

1. Quel service la couche transport fournit-elle aux protocoles applicatifs ?
2. Pourquoi UDP est-il adapté aux applications sensibles à la latence ?
3. Quelle différence existe entre un port source client et un port destination serveur ?
4. Quels indicateurs TCP sont utilisés pendant le three-way handshake ?
5. Que signifie un acquittement `Ack = 1013` ?
6. Pourquoi la fermeture TCP nécessite-t-elle deux annonces `FIN` ?
7. Quelle différence de taille existe entre les en-têtes minimaux TCP et UDP ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 624 à 637.
