---
id: HUA-HCIA-DC-003
title: "Data Communication Process"
official_title: "Data Communication Process"
course_unit: "1.1.3"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-06
updated: 2026-07-13
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 31-36"
tags: [huawei, hcia, datacom, communication-process]
prerequisites:
  - "[[Data Communication Network]]"
  - "[[Data Communication Network Devices]]"
related:
  - "[[02 Network Reference Models/Data Communication Process]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Processus de communication de données (Data Communication Process)

## Objectif du cours

Cette unité décrit le transfert d'une information entre un terminal source et un terminal destination. Huawei utilise l'analogie de l'expédition d'un colis pour introduire les notions de payload, paquet, en-tête, trailer, encapsulation, décapsulation, passerelle et terminal.

## Analogie entre colis et données

Le transfert d'un objet physique et celui d'une information virtuelle suivent une logique comparable : préparation, étiquetage, choix du prochain point de passage, transport intermédiaire et livraison finale.

```text
Objet -> colis -> centres de tri -> transport -> destinataire
Donnée -> paquet -> passerelles/routeurs -> réseau -> application destination
```

Cette analogie prépare l'étude plus précise des modèles en couches, présentée dans l'unité 1.2.

## Préparation et étiquetage chez l'émetteur

Pour un colis, l'expéditeur emballe l'objet et ajoute une étiquette contenant les adresses de l'expéditeur et du destinataire.

Pour une donnée :

- l'application produit la donnée utile ;
- des informations sont ajoutées avant et parfois après cette donnée ;
- l'ensemble forme un paquet ;
- les informations importantes comprennent notamment une adresse source et une adresse destination.

## Expédition locale

Le point de collecte local examine l'adresse du destinataire afin de choisir le prochain centre de traitement.

Dans le réseau, le paquet atteint une passerelle. Selon la présentation simplifiée de Huawei, la passerelle retire l'encapsulation nécessaire à son traitement, lit l'adresse de destination, puis réencapsule l'information pour l'envoyer vers le prochain équipement.

## Tri et transport dans le réseau intermédiaire

Les centres de tri successifs transmettent le colis vers la ville de destination en fonction de l'adresse indiquée.

De même, les routeurs intermédiaires traitent les informations nécessaires, choisissent le prochain chemin et transmettent le paquet à travers le réseau jusqu'au réseau de destination.

## Livraison et réception

Le centre de destination dirige le colis vers le bon point de distribution, puis le destinataire l'ouvre et vérifie son contenu.

Dans le réseau :

1. le paquet atteint la passerelle du réseau destination ;
2. il est traité et transmis vers le terminal approprié ;
3. le terminal vérifie le paquet ;
4. si le paquet est accepté, le payload est remis à l'application concernée.

La communication réseau est alors terminée.

## Termes communs

| Terme | Définition fidèle au support |
| --- | --- |
| Data payload | information réellement destinée à être transportée ; la PDU d'une couche supérieure devient le payload de la couche inférieure |
| Packet | unité de données échangée et transmise sur le réseau selon un format déterminé |
| Header | informations ajoutées avant le payload |
| Trailer | informations ajoutées après le payload ; tous les paquets n'en possèdent pas |
| Encapsulation | ajout d'un header et, selon le protocole, d'un trailer autour du payload |
| Decapsulation | retrait du header et du trailer afin de récupérer le payload |
| Gateway | équipement reliant des réseaux différents ; le terme décrit un rôle de déploiement, pas un type matériel unique |
| Terminal | équipement final qui agit comme source ou destination |

## Place dans le parcours HCIA-Datacom

La dernière diapositive relie ce processus aux modules suivants :

- modèles de référence réseau ;
- Ethernet ;
- IP ;
- sécurité et services réseau ;
- data center ;
- WLAN ;
- exploitation et troubleshooting.

Le parcours détaillera ensuite les fonctions assurées par chaque couche et chaque équipement pendant le transport.

## Résumé fidèle du cours

- Une donnée est préparée et étiquetée avant son transfert.
- Les équipements intermédiaires lisent les informations nécessaires au choix du prochain chemin.
- Le terminal destination vérifie le paquet et remet le payload à l'application.
- Encapsulation et décapsulation permettent aux protocoles en couches d'ajouter puis de retirer leurs informations de contrôle.

## Révision TianSemi

1. Dans l'analogie Huawei, à quoi correspondent l'étiquette, les centres de tri et le destinataire ?
2. Quelle différence existe entre payload, header et trailer ?
3. Pourquoi une gateway est-elle un rôle plutôt qu'un type d'équipement unique ?
4. Quelles étapes permettent de transporter la donnée de l’application source jusqu’à l’application destination ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 31-36.
- Huawei Talent, HCIA-Datacom V2.0 : https://e.huawei.com/en/talent/#/cert/product-details?certifiedProductId=1316&authenticationLevel=CTYPE_CARE_HCIA&technicalField=IIC&version=2.0
