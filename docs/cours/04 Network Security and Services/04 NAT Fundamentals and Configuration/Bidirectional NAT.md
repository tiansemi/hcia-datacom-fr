---
id: HUA-HCIA-DC-056
title: "Bidirectional NAT"
official_title: "Bidirectional NAT"
course_unit: "4.4.4"
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
source_pages: "PDF 732-736"
tags: [huawei, hcia, datacom, bidirectional-nat, source-nat, destination-nat, session-table]
prerequisites:
  - "[[Destination NAT]]"
related:
  - "[[What Is a DC]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# NAT bidirectionnelle (Bidirectional NAT)

## Une combinaison, pas un nouveau type autonome

La NAT bidirectionnelle traduit à la fois la source et la destination d'un même paquet. Huawei précise qu'il ne s'agit pas d'une fonction indépendante : elle résulte de la combinaison de **source NAT** et de **NAT Server**.

Elle répond aux scénarios où :

- la destination possède une association fixe ;
- la source doit également être présentée sous une autre adresse ;
- un utilisateur Internet accède à un serveur interne avec traduction des deux côtés ;
- un utilisateur interne accède à un serveur interne par son adresse publique.

## Lecture du traitement

![La NAT bidirectionnelle applique NAT Server sur la destination et source NAT sur la source](<../../../assets/diagrammes/04 Network Security and Services/bidirectional-nat-flow-fr.svg>)

Pour le premier paquet :

1. la correspondance NAT Server traduit la destination publique et son port vers le serveur privé ;
2. la politique source NAT choisit une nouvelle adresse source et éventuellement un nouveau port ;
3. le routeur crée une session contenant les deux traductions ;
4. le paquet réécrit est transmis au serveur.

Pour la réponse, la table de sessions permet de restaurer simultanément les valeurs attendues par l'émetteur initial.

## Exemple de transformation

| Étape | Source | Destination |
| --- | --- | --- |
| Paquet reçu | `200.1.2.3:2474` | `122.1.2.1:80` |
| Paquet transmis | `192.168.1.10:2674` | `192.168.1.1:80` |
| Réponse du serveur | `192.168.1.1:80` | `192.168.1.10:2674` |
| Réponse rendue au client | `122.1.2.1:80` | `200.1.2.3:2474` |

Le premier couple provient de NAT Server ; le second, du pool de source NAT. La session conserve les deux transformations.

## Pratique TianSemi : reconstruire la session

À partir du tableau précédent :

1. identifiez la règle NAT Server ;
2. identifiez l'adresse choisie dans le pool source ;
3. écrivez le cinq-tuple avant et après traduction ;
4. inversez le flux et vérifiez chaque remplacement ;
5. expliquez pourquoi une seule des deux configurations ne suffirait pas au scénario.

## Troubleshooting

| Symptôme | Contrôle |
| --- | --- |
| Seule la destination change | Politique source NAT et conditions de correspondance. |
| Seule la source change | Association NAT Server et port public demandé. |
| Retour non corrélé | Entrée de session, symétrie du chemin et vieillissement. |
| Mauvaise adresse visible par le serveur | Pool source sélectionné et ordre des règles. |
| Accès par adresse publique depuis l'interne impossible | Chemin du paquet, NAT Server et source NAT tous deux applicables. |

## À retenir pour l'examen HCIA-Datacom

- La NAT bidirectionnelle combine source NAT et NAT Server.
- Elle traduit source et destination dans le même flux.
- La destination fixe est obtenue depuis la server mapping table.
- La source est choisie selon la politique source NAT.
- La session conserve les deux traductions et permet le traitement inverse.
- Elle peut servir lorsqu'un hôte interne utilise l'adresse publique d'un serveur interne.

## Révision TianSemi

1. Pourquoi la NAT bidirectionnelle n'est-elle pas une fonction autonome ?
2. Quelles deux configurations doivent correspondre au premier paquet ?
3. Dans quel ordre les traductions sont-elles appliquées selon le flux Huawei ?
4. Quelle table fournit la destination privée fixe ?
5. Où les deux transformations sont-elles conservées pour les paquets suivants ?
6. Dans quel scénario un client interne peut-il nécessiter cette combinaison ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 732 à 736.
