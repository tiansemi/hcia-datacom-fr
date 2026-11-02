---
id: HUA-HCIA-DC-015
title: "Basic Concepts of VLANs"
official_title: "Basic Concepts of VLANs"
course_unit: "2.2.2"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-10
updated: 2026-08-10
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 196-215"
tags: [huawei, hcia, datacom, vlan, dot1q, access, trunk, hybrid, pvid]
prerequisites:
  - "[[What Is a VLAN]]"
related:
  - "[[VLAN Application]]"
  - "[[VLAN Configuration Examples]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Concepts fondamentaux des VLAN (Basic Concepts of VLANs)

## Identifier un VLAN sur plusieurs switchs

Quand des utilisateurs d'un même service sont raccordés à plusieurs switchs, le switch distant doit savoir à quel VLAN appartient chaque trame. La norme **IEEE 802.1Q** répond à ce besoin en insérant un tag de 4 octets dans la trame Ethernet.

![Tag IEEE 802.1Q et comportement des interfaces access, trunk et hybrid](<../../../assets/diagrammes/02 Ethernet Technology Basics/dot1q-access-trunk-hybrid-fr.svg>)

## Tag IEEE 802.1Q

Le tag est inséré entre l'adresse MAC source et le champ Type de la trame originale.

| Champ du tag | Taille | Signification |
| --- | ---: | --- |
| TPID | 16 bits | `0x8100` identifie une trame IEEE 802.1Q |
| PRI | 3 bits | Priorité 802.1p, de 0 à 7 |
| CFI | 1 bit | Vaut 0 sur Ethernet dans le cours |
| VID | 12 bits | Identifiant du VLAN |

Le VID couvre mathématiquement 0 à 4095, mais 0 et 4095 sont réservés. Les VLAN utilisables vont donc de **1 à 4094**.

Les terminaux classiques du scénario Huawei émettent et traitent des trames non taguées. Le switch ajoute, conserve ou retire le tag selon le type d'interface et sa configuration.

## Méthodes d'affectation à un VLAN

Huawei présente cinq méthodes :

| Méthode | Critère utilisé |
| --- | --- |
| Par interface | PVID du port d'entrée |
| Par adresse MAC | Adresse MAC source |
| Par sous-réseau IP | Adresse IP source et masque |
| Par protocole | Type et encapsulation du protocole |
| Par politique | Combinaison d'interface, MAC, IP ou autres critères |

### Affectation par interface

Chaque interface reçoit un **PVID** (*Port VLAN ID*). Quand une trame non taguée entre, le switch ajoute un tag portant ce PVID. Cette méthode est simple, intuitive et la plus utilisée. Si l'hôte change de port, son VLAN peut changer.

Le PVID par défaut est 1 dans le contexte du cours.

### Affectation par adresse MAC

Le switch compare la MAC source d'une trame non taguée à une table d'associations MAC–VLAN. L'utilisateur conserve ainsi son VLAN lorsqu'il change de port. La méthode est plus flexible, mais plus complexe et vulnérable à l'usurpation d'adresse MAC.

## Types d'interfaces Ethernet de couche 2

### Interface access

Une interface access relie généralement un terminal et appartient à un seul VLAN.

- Réception non taguée : ajout du PVID et acceptation.
- Réception taguée : acceptation seulement si le VID égale le PVID.
- Émission : retrait du tag si le VID égale le PVID ; sinon la trame n'est pas envoyée.

Les trames émises par une interface access sont donc non taguées.

### Interface trunk

Une interface trunk transporte plusieurs VLAN entre switchs ou vers un équipement qui comprend les tags.

- Elle possède un PVID et une liste de VLAN autorisés.
- À la réception d'une trame non taguée, elle ajoute son PVID et vérifie que ce VLAN est autorisé.
- Une trame taguée est acceptée uniquement si son VID est autorisé.
- À l'émission, une trame dont le VID égale le PVID sort non taguée ; les autres VLAN autorisés sortent tagués.

### Interface hybrid

Une interface hybrid possède un PVID, une liste de VLAN autorisés **tagged** et une liste **untagged**.

- Elle accepte les VLAN présents dans l'une de ces listes.
- À l'émission, elle retire le tag pour un VLAN configuré `untagged`.
- Elle conserve le tag pour un VLAN configuré `tagged`.

La différence importante avec un trunk est qu'une interface hybrid peut émettre des trames non taguées pour plusieurs VLAN selon la configuration.

## Tableau de décision à l'émission

| Interface | VLAN admis | Traitement du tag en sortie |
| --- | --- | --- |
| Access | PVID uniquement | Tag retiré |
| Trunk | Liste autorisée | Retiré si VID = PVID, conservé sinon |
| Hybrid | Listes tagged et untagged | Selon la liste configurée |

## Pratique TianSemi : suivre une trame

Deux switchs sont reliés par un trunk dont le PVID vaut 1 et qui autorise les VLAN 10 et 20. Un PC du VLAN 10 envoie une trame non taguée sur un port access VLAN 10.

1. Quel tag est ajouté sur le switch d'accès ?
2. Sous quelle forme la trame sort-elle sur le trunk ?
3. Pourquoi le switch distant peut-il identifier le VLAN ?
4. Sous quelle forme la trame sort-elle vers un autre PC raccordé à un port access VLAN 10 ?

Résultat attendu : ajout d'un tag VID 10, transport tagué sur le trunk puisque le PVID du trunk vaut 1, identification par le VID, puis retrait du tag sur le port access de sortie.

## À retenir pour l'examen HCIA-Datacom

- IEEE 802.1Q ajoute un tag de 4 octets.
- `0x8100` est le TPID standard présenté.
- Le VID utilisable va de 1 à 4094.
- Le PVID classe les trames non taguées reçues.
- Access : un VLAN ; trunk : plusieurs VLAN ; hybrid : contrôle tagged/untagged par VLAN.
- Un trunk n'accepte que les VLAN de sa liste autorisée.

## Révision TianSemi

1. Où le tag 802.1Q est-il inséré dans la trame ?
2. Quels sont les quatre champs du tag et leurs tailles ?
3. Quelle plage de VID est utilisable ?
4. Quel rôle joue le PVID à la réception d'une trame non taguée ?
5. Quelle différence essentielle existe entre access, trunk et hybrid ?
6. Comment un trunk émet-il une trame dont le VID égale son PVID ?
7. Pourquoi l'affectation par MAC est-elle à la fois flexible et risquée ?
8. Quelles sont les cinq méthodes d'affectation VLAN citées par Huawei ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 196 à 215.
