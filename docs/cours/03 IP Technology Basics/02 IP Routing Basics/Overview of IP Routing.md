---
id: HUA-HCIA-DC-032
title: "Overview of IP Routing"
official_title: "Overview of IP Routing"
course_unit: "3.2.1"
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
source_pages: "PDF 394-413"
tags: [huawei, hcia, datacom, routing-table, longest-prefix-match, preference, metric]
prerequisites:
  - "[[Subnetting]]"
related:
  - "[[Direct Routes]]"
  - "[[Static Routes]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble du routage IP (Overview of IP Routing)

## Pourquoi le routage est nécessaire

Une adresse IP appartient à un sous-réseau. Lorsque la source et la destination se trouvent dans des sous-réseaux différents, un équipement de couche 3 doit choisir un chemin et transférer le paquet vers le réseau de destination.

Le **routage** désigne cette sélection de chemin. Un routeur s'appuie sur sa table de routage ; il ne construit pas un chemin complet dans chaque paquet, mais indique le prochain saut à utiliser.

![Du réseau source au réseau de destination : décisions de routage saut par saut](<../../../assets/diagrammes/03 IP Technology Basics/ip-routing-hop-by-hop-fr.svg>)

## Informations d'une route

Une entrée exploitable associe principalement :

| Élément | Rôle |
| --- | --- |
| Destination/Mask | Sous-réseau auquel la route s'applique |
| Proto | Origine de la route : Direct, Static, OSPF, etc. |
| Pre | Préférence administrative de la source |
| Cost | Coût ou métrique au sein d'une même source |
| NextHop | Adresse du prochain équipement |
| Interface | Interface locale de sortie |

```text
<Huawei> display ip routing-table
```

La table de routage n'affiche pas nécessairement toutes les routes découvertes : elle contient les routes retenues comme optimales pour le transfert.

## Comment une route est obtenue

- **Directe** : créée automatiquement pour un réseau attaché à une interface de couche 3 opérationnelle.
- **Statique** : saisie par l'administrateur.
- **Dynamique** : apprise par un protocole tel qu'OSPF.

Huawei présente par défaut une préférence de 0 pour une route directe, 10 pour une route OSPF interne, 60 pour une route statique et 150 pour une route OSPF externe. Une valeur numérique plus faible signifie une meilleure préférence.

## Sélection de la meilleure route

![Ordre des décisions : préfixe, préférence et métrique](<../../../assets/diagrammes/03 IP Technology Basics/ip-route-selection-fr.svg>)

Trois comparaisons doivent être distinguées :

1. Pour l'adresse de destination du paquet, le routeur retient d'abord l'entrée ayant le **préfixe correspondant le plus long**.
2. Pour des routes de destination et masque identiques provenant de sources différentes, il compare leur **préférence** ; la plus petite valeur gagne.
3. Pour des routes d'une même source et de même préférence, il compare leur **métrique** ; la plus petite valeur gagne.

Si une même source fournit plusieurs routes de coût égal vers la même destination, elles peuvent être installées comme routes à coût égal et servir à la répartition de charge.

### Exemple de longest prefix match

Pour une destination `192.168.2.2`, une route `192.168.2.0/24` est plus précise que `192.168.0.0/16`. Le routeur utilise donc le /24, même si les deux préfixes correspondent à l'adresse.

## Transfert d'un paquet entre deux sous-réseaux

1. L'hôte constate que la destination n'est pas locale et remet la trame à sa passerelle.
2. Le routeur enlève l'en-tête de couche 2 et consulte l'adresse IP de destination.
3. Il recherche la route la plus précise, résout le prochain saut et obtient sa MAC par ARP.
4. Il reconstruit une nouvelle trame de couche 2 vers ce prochain saut ; les adresses IP source et destination restent inchangées.
5. Le routeur du dernier saut utilise une route directe et remet le paquet à l'hôte final.

Le routage est **unidirectionnel** : une communication réussie exige aussi des routes valides dans le sens retour.

## Pratique TianSemi : interpréter la table

Supposez trois entrées vers une même zone : `192.168.0.0/16`, `192.168.2.0/24` et `0.0.0.0/0`.

1. Prédisez la route utilisée pour `192.168.2.20`.
2. Prédisez la route utilisée pour `192.168.9.20`.
3. Prédisez la route utilisée pour `203.0.113.10`.
4. Vérifiez avec `display ip routing-table` puis justifiez chaque résultat par la longueur du préfixe.

## Troubleshooting

| Symptôme | Vérification |
| --- | --- |
| Aucune route vers la destination | Destination/Mask et présence d'une route par défaut |
| Mauvais chemin | Préfixe le plus long, préférence, puis coût |
| Aller fonctionnel, retour impossible | Table de routage de la destination et de chaque saut retour |
| Route configurée mais absente | Joignabilité du next hop et état de l'interface |
| Paquet bloqué au dernier saut | Route directe, ARP et état de l'interface locale |

## À retenir pour l'examen HCIA-Datacom

- Une route indique destination, prochain saut et interface de sortie.
- La préférence compare des sources de routes ; la métrique compare des routes d'une même source.
- Une valeur de préférence ou de métrique plus faible est meilleure.
- Le préfixe correspondant le plus long détermine la route de transfert.
- Chaque routeur décide saut par saut.
- Le trajet retour doit lui aussi être routé.

## Révision TianSemi

1. Quelles informations minimales définissent une route ?
2. Quelles sont les trois méthodes d'obtention des routes ?
3. Quelle différence existe entre préférence et métrique ?
4. Pourquoi `192.168.2.0/24` est-il préféré à `192.168.0.0/16` pour `192.168.2.2` ?
5. Quand des routes à coût égal peuvent-elles être utilisées ?
6. Pourquoi un ping peut-il échouer malgré une route correcte dans le sens aller ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 394 à 413.
