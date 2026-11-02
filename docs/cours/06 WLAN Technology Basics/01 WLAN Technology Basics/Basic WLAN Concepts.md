---
id: HUA-HCIA-DC-063
title: "Basic WLAN Concepts"
official_title: "Basic WLAN Concepts"
course_unit: "6.1.2"
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
source_pages: "PDF 791-804"
tags: [huawei, hcia, datacom, wlan, radio-wave, frequency, wavelength, modulation, channel, interference]
prerequisites:
  - "[[WLAN Overview]]"
related:
  - "[[Key WLAN Technologies]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Concepts de base du WLAN (Basic WLAN Concepts)

## Chaîne de communication sans fil

![Étapes d'une communication radio WLAN](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-radio-chain-fr.svg>)

1. La source produit l'information.
2. Le codage source la représente efficacement.
3. Le codage canal ajoute de la redondance contre les erreurs.
4. La modulation place les bits sur une porteuse radio.
5. l'onde traverse l'interface air et subit bruit ou interférences ;
6. le récepteur démodule et décode l'information pour le destinataire.

## Paramètres d'une onde

- la **fréquence** est le nombre de cycles par seconde, en hertz ;
- la **longueur d'onde** est la distance parcourue pendant un cycle, avec `λ = c / f` ;
- l'**amplitude** représente l'écart maximal et traduit la force du signal ;
- la **phase** décrit la position relative de deux ondes de même fréquence.

À vitesse identique, plus la fréquence augmente, plus la longueur d'onde diminue. Deux signaux en phase peuvent s'additionner ; à 180°, ils peuvent s'affaiblir.

## Codage et modulation

Le codage source retire la redondance ou numérise l'information. Le codage canal ajoute des bits de contrôle pour augmenter la probabilité de réception correcte ; LDPC est un exemple utilisé en WLAN.

Une porteuse ne contient initialement aucune information. La modulation modifie son amplitude, sa fréquence ou sa phase selon le signal numérique. La démodulation effectue l'opération inverse.

## Bandes et canaux

Une bande est une plage de fréquences autorisée. Elle est divisée en canaux.

| Bande | Point clé du support |
| --- | --- |
| 2,4 GHz | 14 canaux qui se chevauchent ; 1, 6 et 11 sont traditionnellement non chevauchants. |
| 5 GHz | Davantage de ressources et de canaux généralement non chevauchants. |
| 6 GHz | Ressources plus abondantes, disponibilité variable selon le pays. |

Les règles nationales déterminent les fréquences réellement utilisables.

## Interférences et utilisation du canal

Une interférence est un signal indésirable occupant la même bande que le WLAN : four à micro-ondes, Bluetooth, autre émetteur ou réseau Wi-Fi sur le même canal.

Lorsque plusieurs stations veulent émettre, elles doivent attendre et reculer après collision. Cela augmente le temps d'attente et diminue la capacité utile. L'utilisation du canal correspond au rapport entre le temps occupé et la période observée ; une forte occupation n'est donc pas automatiquement synonyme de données utiles élevées.

## Pratique TianSemi : plan 2,4 GHz

Dessinez trois points d'accès voisins. Attribuez les canaux 1, 6 et 11, puis ajoutez un quatrième AP. Expliquez pourquoi la réutilisation doit tenir compte de la distance, de la puissance reçue et des interférences. Listez ensuite trois sources non-Wi-Fi à rechercher lors d'un diagnostic.

## Troubleshooting conceptuel

| Observation | Hypothèse |
| --- | --- |
| Signal fort mais débit faible | Canal très occupé ou interférences. |
| Dégradation localisée près d'un équipement | Source non-Wi-Fi possible. |
| Plusieurs AP voisins sur le même canal | Contention et interférence co-canal. |
| Couverture courte en fréquence élevée | Longueur d'onde, atténuation et obstacles à considérer. |

## À retenir pour l'examen HCIA-Datacom

- La chaîne radio combine codage, modulation, canal, démodulation et décodage.
- Fréquence et longueur d'onde varient en sens inverse.
- Le codage source vise l'efficacité ; le codage canal vise la fiabilité.
- Amplitude, fréquence et phase peuvent porter l'information.
- Les canaux 1, 6 et 11 sont le plan traditionnel non chevauchant en 2,4 GHz.
- Interférences, collisions et backoff réduisent l'efficacité du canal.

## Révision TianSemi

1. Quel est le rôle du codage canal ?
2. Quelle relation lie fréquence et longueur d'onde ?
3. Comment la phase de deux signaux peut-elle modifier le signal reçu ?
4. Qu'est-ce qu'une porteuse ?
5. Quelle différence existe entre bande de fréquences et canal ?
6. Pourquoi utilise-t-on traditionnellement 1, 6 et 11 en 2,4 GHz ?
7. Comment une interférence augmente-t-elle le temps d'attente radio ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 791 à 804.
