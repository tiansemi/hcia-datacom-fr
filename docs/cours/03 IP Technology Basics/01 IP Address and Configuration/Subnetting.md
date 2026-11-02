---
id: HUA-HCIA-DC-029
title: "Subnetting"
official_title: "Subnetting"
course_unit: "3.1.3"
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
source_pages: "PDF 361-370"
tags: [huawei, hcia, datacom, subnetting, vlsm, ipv4, prefix-length]
prerequisites:
  - "[[Introduction to IPv4 Addresses]]"
related:
  - "[[Overview of IP Routing]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Découpage en sous-réseaux (Subnetting)

## Pourquoi subnetter ?

L'adressage par classes est trop rigide. Affecter un réseau de classe B entier à un seul domaine de broadcast gaspille des milliers d'adresses et produit un domaine trop vaste.

Le subnetting emprunte des bits à la partie hôte pour créer plusieurs préfixes plus petits. Huawei associe cette démarche à VLSM afin d'adapter les tailles aux besoins.

![Découpage de 192.168.10.0/24 en deux sous-réseaux /25](<../../../assets/diagrammes/03 IP Technology Basics/ipv4-subnetting-24-to-25-fr.svg>)

## Exemple officiel : /24 vers deux /25

Réseau initial `192.168.10.0/24` : 8 bits hôte, 256 adresses, 254 utilisables.

Emprunter un bit donne `/25` (`255.255.255.128`) :

| Sous-réseau | Réseau | Hôtes utilisables | Broadcast |
| --- | --- | --- | --- |
| 1 | `192.168.10.0/25` | `.1` à `.126` | `192.168.10.127` |
| 2 | `192.168.10.128/25` | `.129` à `.254` | `192.168.10.255` |

Chaque /25 conserve 7 bits hôte : 128 adresses, 126 utilisables.

## Méthode de calcul

1. Déterminer le nombre d'hôtes nécessaires.
2. Trouver `h` tel que `2^h - 2` couvre ce besoin.
3. Calculer le préfixe `32 - h`.
4. Déterminer la taille de bloc : `2^h` adresses.
5. Énumérer les adresses réseau par pas de cette taille.
6. Pour chaque bloc, placer le broadcast à l'adresse précédant le réseau suivant.
7. Les hôtes occupent les valeurs entre réseau et broadcast.

## Exemple VLSM du cours

Pour au moins 10 hôtes, `2^4 - 2 = 14` : quatre bits hôte suffisent, donc `/28`.

Les réseaux /28 progressent par blocs de 16 :

```text
192.168.1.0/28
192.168.1.16/28
192.168.1.32/28
...
192.168.1.240/28
```

Huawei recommande de planifier d'abord le sous-réseau qui possède le plus grand nombre d'hôtes, puis les plus petits.

## Principes de planification Huawei

- **hiérarchie** : découper de haut en bas selon géographie et services ;
- **contiguïté** : conserver des blocs voisins pour faciliter la sommation de routes ;
- **évolutivité** : réserver de l'espace pour la croissance ;
- **efficacité** : éviter des préfixes beaucoup plus grands que le besoin.

Le support illustre des /32 pour les loopbacks, /30 pour les interconnexions et /24 pour les terminaux. Ce sont des pratiques d'exemple, pas une règle universelle à appliquer sans analyser le réseau.

## Pratique TianSemi

Découpez `192.168.20.0/24` pour trois groupes de 60, 30 et 12 hôtes.

1. 60 hôtes : `/26`, bloc de 64.
2. 30 hôtes : `/27`, bloc de 32.
3. 12 hôtes : `/28`, bloc de 16.
4. Allouez du plus grand au plus petit, puis documentez réseau, plage et broadcast.

Une allocation possible : `192.168.20.0/26`, `192.168.20.64/27`, `192.168.20.96/28`.

## Erreurs fréquentes

- compter réseau et broadcast parmi les hôtes ;
- oublier que la taille de bloc doit être une puissance de deux ;
- commencer un sous-réseau sur une adresse non alignée ;
- allouer les petits réseaux avant les grands en VLSM ;
- créer des blocs impossibles à résumer faute de contiguïté.

## À retenir pour l'examen HCIA-Datacom

- Emprunter un bit double le nombre de sous-réseaux et divise la taille de bloc par deux.
- `2^h - 2` donne les hôtes utilisables dans le modèle du cours.
- Les bits hôte à 0 donnent le réseau, à 1 le broadcast.
- VLSM adapte les préfixes aux besoins.
- Planifier du plus grand besoin au plus petit.

## Révision TianSemi

1. Pourquoi le subnetting réduit-il le gaspillage d'adresses ?
2. Que produit l'emprunt d'un bit à la partie hôte ?
3. Quels sont les deux sous-réseaux obtenus à partir de `192.168.10.0/24` avec /25 ?
4. Quel préfixe minimal couvre 40 hôtes ?
5. Pourquoi commencer par le plus grand sous-réseau en VLSM ?
6. Quels quatre principes Huawei propose-t-il pour la planification ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 361 à 370.
