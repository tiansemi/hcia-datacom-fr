---
id: HUA-HCIA-DC-040
title: "ICMPv6"
official_title: "ICMPv6"
course_unit: "3.4.2"
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
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 528-541"
tags: [huawei, hcia, datacom, icmpv6, ndp, rs, ra, ns, na, dad, path-mtu]
prerequisites:
  - "[[Overview of IPv6]]"
related:
  - "[[IPv6 Address Autoconfiguration]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# ICMPv6

## Un protocole indispensable à IPv6

ICMPv6 transporte des erreurs et des informations, mais il sert aussi de fondation à NDP, à la découverte du MTU du chemin, à l'autoconfiguration, à la résolution d'adresse et à la détection de doublons.

| Catégorie | Types principaux du cours |
| --- | --- |
| Erreur | 1 Destination Unreachable, 2 Packet Too Big, 3 Time Exceeded, 4 Parameter Problem |
| Information | 128 Echo Request, 129 Echo Reply |
| NDP | 133 RS, 134 RA, 135 NS, 136 NA, 137 Redirect |

## Path MTU Discovery

Les routeurs IPv6 ne fragmentent pas. Lorsqu'un paquet dépasse le MTU du prochain lien, le routeur le supprime et renvoie **Packet Too Big** (type 2) avec le MTU accepté. La source réduit alors la taille de ses paquets.

Bloquer indistinctement ICMPv6 peut donc casser la découverte du MTU et d'autres fonctions essentielles.

## NDP et ses fonctions

![RS, RA, NS et NA au service des fonctions NDP](<../../../assets/diagrammes/03 IP Technology Basics/icmpv6-ndp-functions-fr.svg>)

NDP utilise ICMPv6 pour :

- découvrir les routeurs et préfixes ;
- résoudre une adresse IPv6 en adresse de couche 2 ;
- vérifier l'unicité d'une adresse avec DAD ;
- suivre l'état des voisins ;
- prendre en charge redirection et autoconfiguration.

## Découverte des routeurs : RS et RA

Au démarrage, l'hôte envoie un **RS** (type 133) aux routeurs du lien. Un routeur répond par **RA** (type 134) et en émet aussi périodiquement.

Le RA fournit notamment préfixes, paramètres d'autoconfiguration et informations de route par défaut. Sa source doit être une adresse link-local du routeur et sa destination peut viser l'hôte ou tous les nœuds du lien.

## Résolution d'adresse : NS et NA

IPv6 n'utilise pas ARP. Le demandeur envoie un **NS** (type 135) au groupe solicited-node de la cible. La cible répond par **NA** (type 136) avec son adresse de couche 2.

La sollicitation multicast réduit la charge par rapport à un broadcast reçu par tous les nœuds.

```text
<R1> display ipv6 neighbors
```

## Duplicate Address Detection

Avant d'utiliser une adresse unicast, y compris link-local, l'interface la considère comme **tentative** et réalise DAD.

1. Elle émet un NS avec source `::` vers le solicited-node de l'adresse testée.
2. Si aucune réponse conflictuelle n'arrive, l'adresse devient utilisable.
3. Si un NA ou un NS concurrent révèle un doublon, l'adresse ne doit pas être utilisée.

## Ping IPv6

```text
<R1> ping ipv6 2001:DB8::2
<R1> ping ipv6 FE80::2 -i GE 0/0/1
```

Une adresse link-local n'est unique qu'à l'intérieur d'un lien ; il faut donc préciser l'interface de sortie ou une source appropriée.

## Troubleshooting

| Symptôme | Contrôle prioritaire |
| --- | --- |
| Ping link-local ambigu | Interface de sortie avec `-i` |
| Gros transferts bloqués | ICMPv6 type 2 et MTU du chemin |
| Voisin absent | NS/NA, groupe solicited-node et couche 2 |
| Adresse reste tentative | DAD et doublon sur le lien |
| Pas de route par défaut SLAAC | Réception RA et émission RA du routeur |
| Autoconfiguration partielle | Préfixe et bits M/O du RA |

## À retenir pour l'examen HCIA-Datacom

- ICMPv6 est nécessaire au bon fonctionnement d'IPv6.
- Packet Too Big remplace la fragmentation intermédiaire.
- RS/RA découvrent les routeurs ; NS/NA résolvent les adresses.
- NDP remplace plusieurs fonctions d'ARP et ajoute DAD.
- DAD utilise une source `::` et une destination solicited-node.
- Un ping vers une link-local doit identifier le lien.

## Révision TianSemi

1. Quelles fonctions IPv6 s'appuient sur ICMPv6 ?
2. Pourquoi Packet Too Big est-il indispensable ?
3. Quels messages réalisent la découverte de routeur ?
4. Quels messages remplacent ARP pour la résolution d'adresse ?
5. Comment DAD décide-t-il qu'une adresse est unique ?
6. Pourquoi faut-il préciser une interface pour pinguer une link-local ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 528 à 541.
