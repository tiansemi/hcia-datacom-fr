---
id: HUA-HCIA-DC-025
title: "Link Aggregation in LACP Mode"
official_title: "Link Aggregation in LACP Mode"
course_unit: "2.4.3"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "intermediate"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-10
updated: 2026-08-10
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 313-324"
tags: [huawei, hcia, datacom, lacp, lacpdu, eth-trunk, actor, load-balancing]
prerequisites:
  - "[[Link Aggregation in Manual Mode]]"
related:
  - "[[Link Aggregation Configuration Examples]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Agrégation de liens en mode LACP (Link Aggregation in LACP Mode)

## Négociation par LACPDU

En mode LACP, les équipements échangent des **LACPDU** contenant notamment priorité système, adresse MAC, priorité d'interface et numéro d'interface. Ils vérifient ainsi que les interfaces homologues appartiennent au même groupe sur le même pair.

Huawei distingue :

- **static LACP** : après échec de négociation, l'Eth-Trunk passe Down et ne transfère plus ;
- **dynamic LACP** : l'Eth-Trunk passe Down, mais ses membres peuvent hériter des attributs VLAN et transférer indépendamment en couche 2.

Le mode dynamique est réservé dans le cours aux connexions Huawei–serveur. Pour les autres scénarios, Huawei recommande le mode statique afin d'éviter un risque de boucle. Certains switchs ne prennent en charge que le mode statique.

## Élection de l'Actor

Les deux extrémités doivent sélectionner le même nombre de liens actifs. Elles élisent donc un **Actor**, dont la décision sera suivie par le Partner.

1. plus petite priorité système LACP ;
2. plus petite adresse MAC système en cas d'égalité.

La priorité système par défaut vaut 32768 ; une valeur plus faible est prioritaire.

## Sélection des interfaces actives

L'Actor compare ensuite :

1. la priorité LACP des interfaces ;
2. le numéro d'interface si les priorités sont identiques.

La priorité d'interface par défaut vaut 32768 et la valeur la plus faible gagne. Si le maximum de liens actifs est inférieur au nombre de membres, les autres deviennent inactifs et servent de secours.

![Élection LACP de l'Actor puis des liens actifs et de secours](<../../../assets/diagrammes/02 Ethernet Technology Basics/lacp-active-link-election-fr.svg>)

Lorsqu'un lien actif tombe, le membre inactif de meilleure priorité devient actif. La capacité globale peut ainsi rester stable si un lien de secours équivalent est disponible.

Les drapeaux Synchronization, Collecting et Distributing d'une LACPDU valent 1 pour une interface active et 0 pour une interface inactive dans le mécanisme présenté.

## Répartition par flux

L'agrégation peut théoriquement répartir par paquet ou par flux. La répartition par paquet utilise bien la bande passante, mais peut désordonner les paquets. Eth-Trunk emploie donc une répartition **par flux** : tous les paquets d'un même flux empruntent le même membre.

Le hachage peut utiliser :

- adresses IP source et/ou destination ;
- adresses MAC source et/ou destination.

Le choix est local pour le trafic sortant. Il doit correspondre à la diversité réelle du trafic. Si toutes les MAC sont identiques mais que les IP varient, un hachage IP répartit mieux les flux qu'un hachage MAC.

## Pratique TianSemi : dérouler l'élection

Deux switchs ont quatre membres et un maximum de deux actifs. Les priorités système sont égales, mais `SW1` possède la plus petite MAC.

1. `SW1` devient Actor.
2. Si les priorités d'interface sont égales, les deux plus petits numéros deviennent actifs.
3. Les deux autres membres restent inactifs.
4. Si le premier membre actif tombe, le membre inactif au plus petit numéro est sélectionné.

## Troubleshooting

| Symptôme | Vérification |
| --- | --- |
| L'Eth-Trunk ne monte pas | Mode identique, câblage pair à pair, membres dans le même groupe |
| Nombre d'actifs différent | Même maximum `active-linknumber` aux deux extrémités |
| Mauvais Actor | Priorité système puis MAC système |
| Mauvais membre actif | Priorité LACP du port puis numéro d'interface |
| Répartition très inégale | Clés de hachage adaptées aux flux observés |
| Un seul gros flux ne dépasse pas un lien | Comportement normal de la répartition par flux |

## À retenir pour l'examen HCIA-Datacom

- LACP négocie par LACPDU.
- Actor : priorité système, puis MAC ; plus petit gagne.
- Liens actifs : priorité d'interface, puis numéro ; plus petit gagne.
- Les priorités système et interface valent 32768 par défaut.
- Le maximum d'actifs doit être identique aux deux extrémités.
- Eth-Trunk répartit par flux pour préserver l'ordre des paquets.

## Révision TianSemi

1. Quelles informations principales une LACPDU transporte-t-elle ?
2. Quelle différence existe entre static et dynamic LACP après un échec de négociation ?
3. Comment l'Actor est-il élu ?
4. Comment les interfaces actives sont-elles sélectionnées ?
5. Quel rôle joue le maximum de liens actifs ?
6. Pourquoi Eth-Trunk préfère-t-il la répartition par flux ?
7. Comment choisir entre un hachage IP et MAC ?
8. Un flux unique utilise-t-il simultanément tous les membres ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 313 à 324.
