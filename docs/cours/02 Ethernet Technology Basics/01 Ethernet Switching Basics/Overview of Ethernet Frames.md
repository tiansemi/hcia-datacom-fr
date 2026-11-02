---
id: HUA-HCIA-DC-012
title: "Overview of Ethernet Frames"
official_title: "Overview of Ethernet Frames"
course_unit: "2.1.2"
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
source_pages: "PDF 161-171"
tags: [huawei, hcia, datacom, ethernet-frame, mac-address, ethernet-ii, ieee-802-3]
prerequisites:
  - "[[Overview of the Ethernet Protocol]]"
related:
  - "[[Working Principles of Ethernet Switches]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble des trames Ethernet (Overview of Ethernet Frames)

## Objectifs officiels couverts

Cette unité permet de :

- reconnaître les principaux formats de trame Ethernet ;
- expliquer les champs d'une trame Ethernet II ;
- décrire la représentation et la structure d'une adresse MAC ;
- distinguer les adresses et trames unicast, broadcast et multicast ;
- différencier le rôle d'une adresse IP de celui d'une adresse MAC.

![Structure d'une trame Ethernet II et types d'adresses MAC](<../../../assets/diagrammes/02 Ethernet Technology Basics/ethernet-frame-and-mac-types-fr.svg>)

## Formats de trames Ethernet

Huawei présente trois formats :

- **Ethernet II**, aussi appelé Ethernet V2, qui est le format le plus courant ;
- **IEEE 802.3 LLC**, qui ajoute les champs DSAP, SSAP et Control ;
- **IEEE 802.3 SNAP**, qui ajoute notamment un OUI et un champ Type pour prendre en charge davantage de protocoles supérieurs.

### Trame Ethernet II

| Champ | Taille | Fonction |
| --- | ---: | --- |
| DMAC | 6 octets | Identifie le destinataire de la trame |
| SMAC | 6 octets | Identifie l'émetteur de la trame |
| Type | 2 octets | Indique le protocole encapsulé |
| Données utilisateur | 46 à 1500 octets | Transporte les données de couche supérieure et, si nécessaire, du bourrage |
| FCS | 4 octets | Permet de détecter des erreurs dans la trame |

Valeurs de Type citées dans le support :

- `0x0800` : IPv4 ;
- `0x0806` : ARP.

La longueur totale d'une trame est comprise entre **64 et 1518 octets** dans le format présenté, hors préambule et délimiteur de début de trame.

### Pourquoi 64 à 1518 octets ?

La taille minimale de 64 octets permettait à l'émetteur de détecter une collision avant d'avoir terminé l'envoi sur un Ethernet partagé. Si les données utiles occupent moins de 46 octets, un bourrage complète le champ de données.

La taille maximale de 1518 octets correspond à :

- 14 octets d'en-tête Ethernet ;
- 1500 octets de données, valeur à l'origine de la MTU Ethernet habituelle ;
- 4 octets de FCS.

Cette limite établit un compromis entre efficacité de transmission et délai imposé aux autres communications sur un lien partagé.

## Adresse MAC

Une adresse MAC identifie une NIC Ethernet. Elle contient **48 bits**, soit **6 octets**, et s'écrit généralement en hexadécimal, par exemple :

```text
00-1E-10-DD-DD-02
001E-10DD-DD02
```

Les 24 premiers bits forment l'**OUI** attribué par l'IEEE au fabricant. Les 24 derniers bits sont attribués par ce fabricant pour distinguer ses interfaces.

## Adresse IP et adresse MAC

| Adresse IP | Adresse MAC |
| --- | --- |
| Identifie un nœud selon la topologie IP | Identifie une interface Ethernet |
| Peut changer lorsqu'un équipement change de réseau | Est attribuée à la NIC par le fabricant dans le modèle présenté |
| Permet l'acheminement entre réseaux | Sert à la livraison sur le segment Ethernet local |

Les deux niveaux d'adressage sont complémentaires. Un hôte peut conserver son adresse IP si sa carte réseau est remplacée, ou recevoir une nouvelle adresse IP lorsqu'il change de réseau sans remplacer sa NIC.

## Trois classes d'adresses MAC

### Unicast

Une adresse MAC unicast identifie une seule interface. Une trame unicast est envoyée d'une source vers un destinataire unique. Une adresse unicast peut apparaître comme adresse source ou destination.

### Broadcast

L'adresse `FF-FF-FF-FF-FF-FF` désigne tous les nœuds du domaine de broadcast. Elle ne peut pas être une adresse source valide.

### Multicast

Une adresse MAC multicast identifie un groupe de nœuds. Hors adresse de broadcast, une adresse dont le bit de poids faible du premier octet vaut 1 est une adresse multicast. Elle ne peut être utilisée que comme adresse de destination.

| Mode | Destinataires | Exemple de DMAC du cours |
| --- | --- | --- |
| Unicast | Un seul nœud | `00-1E-10-DD-DD-02` |
| Broadcast | Tous les nœuds du domaine | `FF-FF-FF-FF-FF-FF` |
| Multicast | Un groupe de nœuds | `01-80-C2-00-00-01` |

## Pratique TianSemi : décoder une trame

Analysez l'en-tête Ethernet II suivant :

```text
DMAC  FF-FF-FF-FF-FF-FF
SMAC  00-1E-10-DD-DD-02
Type  0x0806
```

Répondez aux questions :

1. Quel est le mode de livraison de la trame ?
2. Quelle interface est l'émetteur ?
3. Quel protocole est encapsulé ?
4. Quels équipements du domaine de broadcast doivent examiner la trame ?

Résultat attendu : il s'agit d'une trame de broadcast émise par `00-1E-10-DD-DD-02`, transportant ARP ; toutes les interfaces du même domaine de broadcast la reçoivent et examinent son contenu.

## Troubleshooting : vérifier le bon champ

| Symptôme ou question | Champ à examiner |
| --- | --- |
| La trame atteint-elle le bon destinataire local ? | DMAC |
| Quelle interface a émis la trame ? | SMAC |
| Le contenu est-il IPv4 ou ARP ? | Type |
| Une corruption a-t-elle été détectée ? | FCS |
| La charge dépasse-t-elle la MTU habituelle ? | Taille du champ de données |

## À retenir pour l'examen HCIA-Datacom

- Ethernet II est le format le plus courant.
- DMAC et SMAC mesurent chacun 6 octets.
- `0x0800` désigne IPv4 et `0x0806` ARP.
- Une trame Ethernet standard présentée dans le cours mesure de 64 à 1518 octets.
- Une adresse MAC contient 48 bits ; l'OUI occupe les 24 premiers bits.
- Unicast vise un nœud, broadcast tous les nœuds, multicast un groupe.
- Une adresse multicast ou broadcast ne doit pas être utilisée comme SMAC.

## Résumé fidèle du cours

Les trames Ethernet transportent les données de couche supérieure avec des adresses MAC source et destination, une indication du protocole encapsulé et un contrôle d'erreur. Ethernet II est le format le plus utilisé. Les adresses MAC de 48 bits se répartissent en unicast, broadcast et multicast, et complètent l'adressage IP au niveau du segment local.

## Révision TianSemi

1. Quels sont les cinq champs principaux d'une trame Ethernet II ?
2. À quels protocoles correspondent `0x0800` et `0x0806` ?
3. Pourquoi une trame Ethernet possède-t-elle une taille minimale de 64 octets ?
4. Quelle différence de rôle existe entre une adresse IP et une adresse MAC ?
5. Comment reconnaît-on l'adresse MAC de broadcast ?
6. Une adresse MAC multicast peut-elle être utilisée comme adresse source ?
7. Quel organisme attribue l'OUI et quelle est sa longueur ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 161 à 171.
