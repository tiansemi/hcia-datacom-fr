---
id: HUA-HCIA-DC-013
title: "Working Principles of Ethernet Switches"
official_title: "Working Principles of Ethernet Switches"
course_unit: "2.1.3"
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
source_pages: "PDF 172-187"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 31-32 et 43"
tags: [huawei, hcia, datacom, ethernet-switch, mac-address-table, flooding, forwarding]
prerequisites:
  - "[[Overview of Ethernet Frames]]"
related:
  - "[[What Is a VLAN]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Principes de fonctionnement des commutateurs Ethernet (Working Principles of Ethernet Switches)

## Objectifs officiels couverts

Cette unité explique comment un switch de couche 2 :

- apprend les adresses MAC sources ;
- construit et actualise sa table d'adresses MAC ;
- recherche l'adresse MAC de destination ;
- choisit entre flooding, forwarding et discarding ;
- s'appuie sur ARP pendant les premiers échanges entre deux hôtes.

![Décision d'un commutateur Ethernet après réception d'une trame](<../../../assets/diagrammes/02 Ethernet Technology Basics/ethernet-switch-frame-decision-fr.svg>)

## Rôle de la table d'adresses MAC

Un switch de couche 2 transfère les trames en fonction de leur adresse MAC. Sa table d'adresses MAC mémorise principalement :

- une adresse MAC apprise ;
- le port sur lequel cette adresse a été observée ;
- le VLAN associé au port ;
- le type d'entrée et son âge.

Le switch **apprend à partir de la SMAC**, puis **cherche la DMAC** pour décider où envoyer la trame. Cette distinction est centrale pour l'examen.

Les entrées dynamiques vieillissent. Si aucune nouvelle trame source ne rafraîchit une entrée avant la fin de son délai, elle est supprimée. Huawei cite un délai par défaut de 300 secondes sur les switchs de série S dans l'exemple du cours ; la valeur réelle doit être vérifiée sur l'équipement utilisé.

## Trois comportements de traitement

### Flooding

Le switch transmet la trame par tous les autres ports du domaine concerné lorsqu'il reçoit :

- une trame unicast dont la DMAC est inconnue ;
- une trame de broadcast ;
- une trame multicast dans le comportement simplifié présenté par cette unité.

Le port d'entrée n'est pas utilisé comme port de sortie.

### Forwarding

Si la DMAC correspond à une entrée connue et que le port indiqué est différent du port d'entrée, le switch transfère la trame uniquement par ce port.

### Discarding

Si la DMAC correspond à une entrée dont le port est précisément le port d'entrée, le switch ne renvoie pas la trame sur ce même segment : il la supprime.

| DMAC dans la table | Port trouvé | Décision |
| --- | --- | --- |
| Absente | — | Flooding vers les autres ports |
| Présente | Différent du port d'entrée | Forwarding vers le port trouvé |
| Présente | Identique au port d'entrée | Discarding |

## Apprentissage initial avec ARP

Huawei décrit l'échange entre deux hôtes dont les tables sont initialement vides.

1. L'hôte 1 connaît l'adresse IP de l'hôte 2, mais pas sa MAC.
2. Il émet une requête ARP dans une trame de broadcast.
3. En recevant la trame, le switch apprend `MAC 1` sur le port d'entrée.
4. Il diffuse la requête ARP par les autres ports.
5. L'hôte 2 répond par une trame unicast destinée à `MAC 1`.
6. Le switch apprend `MAC 2` sur le nouveau port.
7. La table contenant déjà `MAC 1`, il envoie la réponse uniquement vers le port de l'hôte 1.
8. Les échanges ultérieurs entre les deux hôtes sont transférés directement d'après la table MAC.

Deux tables jouent donc des rôles différents :

| Table | Emplacement | Association mémorisée |
| --- | --- | --- |
| Table ARP | Hôte ou équipement de couche 3 | Adresse IP ↔ adresse MAC |
| Table MAC | Switch de couche 2 | Adresse MAC ↔ port et VLAN |

## Vérifications Huawei utiles

Les commandes suivantes apparaissent dans le Lab Guide VLAN et permettent d'observer le mécanisme décrit :

```text
<S2> display mac-address
<R1> display arp
```

- `display mac-address` vérifie les MAC apprises, leur VLAN et leur interface d'apprentissage ;
- `display arp` vérifie les correspondances IP–MAC connues par un hôte Huawei ou un équipement de couche 3.

Ces sorties ne représentent pas la même table et ne doivent pas être confondues.

## Mini-lab TianSemi : observer l'apprentissage

### Topologie

Reliez deux hôtes, `H1` et `H2`, à deux ports d'un switch Huawei. Placez-les dans le même réseau IP et le même VLAN.

### Procédure

1. Relevez les adresses MAC des deux hôtes.
2. Avant tout échange, observez la table du switch :

```text
<S1> display mac-address
```

3. Depuis `H1`, envoyez un ping vers `H2`.
4. Affichez de nouveau la table MAC.
5. Sur un hôte Huawei utilisé dans le lab, observez aussi la table ARP :

```text
<H1> display arp
```

### Résultats attendus

- la MAC de `H1` est associée à son port d'entrée ;
- la MAC de `H2` est associée à l'autre port ;
- la table ARP associe l'IP de `H2` à sa MAC ;
- un second ping utilise les entrées déjà apprises, tant qu'elles sont valides.

## Troubleshooting

| Symptôme | Vérification | Interprétation possible |
| --- | --- | --- |
| Une MAC n'apparaît pas | L'hôte a-t-il émis une trame ? | Un switch apprend la SMAC seulement à la réception d'une trame. |
| Une MAC apparaît sur le mauvais port | Observer le câblage et les changements récents | L'hôte a pu être déplacé ou une boucle peut provoquer une instabilité. |
| L'unicast est diffusé | Rechercher la DMAC dans `display mac-address` | La destination est encore inconnue ou l'entrée a expiré. |
| La table MAC est correcte mais le ping échoue | Comparer table ARP, adresses IP et VLAN | Le problème peut se situer hors de la commutation MAC. |
| Les entrées changent continuellement de port | Rechercher une boucle de couche 2 | Une boucle peut provoquer du MAC flapping et des broadcasts excessifs. |

## À retenir pour l'examen HCIA-Datacom

- Le switch apprend l'adresse MAC **source** sur le port d'entrée.
- Il cherche l'adresse MAC **destination** dans la table MAC.
- Une unicast inconnue est diffusée par les autres ports, pas supprimée.
- Une unicast connue est transférée par le port associé.
- Si le port de destination est le port d'entrée, la trame est supprimée.
- Les entrées MAC dynamiques possèdent un délai de vieillissement.
- ARP associe IP et MAC ; la table du switch associe MAC, port et VLAN.

## Résumé fidèle du cours

Le switch construit sa table en observant les adresses MAC sources des trames reçues. Pour chaque trame, il recherche ensuite la MAC de destination et choisit de diffuser, transférer ou supprimer. Le premier échange ARP permet souvent d'apprendre les MAC des deux hôtes ; les communications suivantes peuvent alors être envoyées directement par les ports connus.

## Révision TianSemi

1. À partir de quel champ le switch crée-t-il une entrée MAC ?
2. Que fait-il d'une trame unicast dont la DMAC est inconnue ?
3. Dans quel cas une trame unicast connue est-elle supprimée ?
4. Pourquoi une requête ARP initiale est-elle diffusée ?
5. Quelles informations une table MAC associe-t-elle ?
6. Quelle différence existe entre une table ARP et une table MAC ?
7. Pourquoi une entrée MAC dynamique peut-elle disparaître ?
8. Quelle commande Huawei permet d'observer les adresses MAC apprises ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 172 à 187.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 31 à 32 et 43 pour l'observation des tables ARP et MAC.
