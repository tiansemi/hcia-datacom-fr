---
id: HUA-HCIA-DC-005
title: Applications and Data
official_title: Applications and Data
course_unit: 1.2.1
category: Huawei HCIA-Datacom Topic
domain: Datacom
vendor: Huawei
certification: HCIA-Datacom
version: V2.0
difficulty: beginner
status: reviewed
author: MOULO OHOLO Jean Noel
reviewed: true
created: 2026-07-13
updated: 2026-08-10
source: HCIA-Datacom V2.0 Training Material
source_pages: PDF 46-52
tags:
  - huawei
  - hcia
  - datacom
  - applications
  - information
  - data
prerequisites:
  - "[[01 Data Communication Network Basics/Data Communication Process]]"
related:
  - "[[Network Reference Models and Standard Protocols]]"
  - "[[02 Network Reference Models/Data Communication Process]]"
lab_required: false
publication_status: generated
site_url: https://tiansemi.github.io/apprentissage/reseaux/hcia-datacom-applications-donnees
---

# Applications et données (Applications and Data)

## Place dans le cours

L'unité **1.2.1 Applications and Data** ouvre le cours Huawei consacré aux modèles de référence réseau. Elle pose la question de départ : avant d'étudier les couches, les protocoles et l'encapsulation, **qu'est-ce qui doit réellement être transporté sur le réseau ?**

L'objectif officiel couvert ici est de comprendre la définition des données et le principe général de leur transmission. Les modèles de référence, les protocoles standards, l'encapsulation et la décapsulation seront traités dans les unités suivantes.

## Des besoins humains aux applications

Les applications répondent à différents besoins humains. Huawei prend notamment comme exemples :

- la navigation web ;
- le jeu en ligne ;
- la diffusion de vidéos.

L'utilisation d'une application génère de l'**information**. Cette information peut prendre plusieurs formes : texte, image, son ou vidéo.

![Trois usages d'applications : navigation web, jeu en ligne et vidéo en streaming](<../../../assets/diagrammes/01 Data Communication Network Basics/applications-human-needs-triptych-light.webp>)

*Illustration TianSemi - Trois besoins couverts par des applications : consulter des informations sur le Web, jouer en ligne et regarder une vidéo en streaming, d'après le Training Material, page PDF 50.*

## De l'information aux données

Dans le domaine informatique, Huawei présente les **données** comme le support de l'information. Un ordinateur traite des données numériques composées de `0` et de `1`, mais il n'interprète pas directement les formes d'information utilisées par l'humain.

Inversement, les données doivent être converties sous une forme compréhensible par l'être humain.

Huawei distingue deux opérations :

| Opération                    | Sens dans le cours Huawei                                                         | Exemple simple                                                     |
| ---------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Génération des données**   | l'information est représentée sous une forme que l'ordinateur peut traiter        | le texte saisi dans un navigateur est encodé en données numériques |
| **Transmission des données** | les données produites par une application sont transportées entre des équipements | une requête web est envoyée du poste client vers un serveur        |

Il faut donc éviter de confondre les deux notions : l'**information** correspond au contenu et au sens ; les **données** correspondent à sa représentation exploitable par le système informatique.

## Transmission de bout en bout

La plupart des applications doivent transmettre leurs données entre plusieurs équipements. Huawei utilise l'exemple d'un **client web** qui envoie des données à un **serveur web**.

![Transmission de données entre un client web et un serveur web](<../../../assets/diagrammes/01 Data Communication Network Basics/web-data-transmission-path-fr-3d.webp>)

*Illustration TianSemi - Vue de bout en bout du client web, des équipements réseau intermédiaires et du serveur web, d'après le Training Material, page PDF 52.*

Le trajet fait intervenir plusieurs modules et équipements :

| Élément | Rôle dans le trajet présenté |
| --- | --- |
| Navigateur web | produit la demande du client et présente l'information reçue |
| Pile TCP/IP du client | applique les protocoles nécessaires à la communication |
| Pilote de la carte réseau | relie le traitement logiciel à l'interface réseau du poste |
| Switchs | transfèrent les données sur les réseaux locaux traversés |
| Routeurs | acheminent les données entre des réseaux différents |
| Pile TCP/IP du serveur | reçoit et traite les données destinées au serveur |
| Programme serveur web | interprète la demande et produit la réponse applicative |

Cette vue reste volontairement globale. Le cours détaillera ensuite comment les modèles de référence attribuent une responsabilité à chaque couche et comment les protocoles coopèrent pendant la transmission.

## Lecture du processus étape par étape

1. L'utilisateur exprime un besoin au moyen du navigateur.
2. Le navigateur génère les données correspondant à la demande.
3. La pile TCP/IP et le pilote de la carte réseau préparent leur transmission.
4. Les switchs et les routeurs transfèrent les données dans le réseau intermédiaire.
5. Le pilote et la pile TCP/IP du serveur remettent les données au programme serveur web.
6. Le serveur interprète la demande et génère une réponse.
7. Le processus inverse permet au navigateur de restituer une information compréhensible à l'utilisateur.

## Point de vue de l'ingénieur réseau

Huawei insiste sur le **processus de transmission de bout en bout**. L'ingénieur réseau ne doit pas observer uniquement un câble, un switch ou un routeur isolé : il doit comprendre la chaîne complète qui relie l'application cliente à l'application serveur.

La première compétence recherchée est de savoir identifier :

- les deux extrémités de la communication ;
- les applications qui produisent ou consomment les données ;
- les modules réseau présents sur les extrémités ;
- les équipements intermédiaires qui assurent le transfert.

## Mise en situation TianSemi

### Scénario

Un utilisateur ouvre une page web depuis son ordinateur. Le serveur est hébergé sur un autre réseau et le trafic traverse un switch local ainsi que plusieurs routeurs.

### Travail demandé

Sans utiliser de commande, reproduire le trajet suivant sur papier ou dans un outil de dessin :

```text
Navigateur -> pile TCP/IP -> pilote de carte réseau
           -> switch -> routeur(s) -> switch
           -> pilote de carte réseau -> pile TCP/IP -> serveur web
```

Pour chaque élément, indiquer s'il :

1. produit ou interprète l'information ;
2. prépare ou traite les données ;
3. transfère les données vers l'étape suivante.

### Résultat attendu

- Les applications cliente et serveur donnent un sens aux données.
- Les piles TCP/IP et les pilotes assurent le traitement réseau sur les extrémités.
- Les switchs et les routeurs assurent le transfert dans le réseau intermédiaire.
- L'ensemble constitue un processus de transmission de bout en bout.

## À retenir pour l'examen HCIA

- Une application répond à un besoin humain et produit ou présente de l'information.
- Dans le domaine informatique, les données servent de support à l'information.
- Les ordinateurs traitent des données numériques ; une conversion est nécessaire entre la représentation humaine et la représentation informatique.
- La génération des données et leur transmission sont deux opérations distinctes.
- L'exemple de référence est la communication entre un client web et un serveur web.
- L'ingénieur réseau s'intéresse à l'ensemble du trajet de bout en bout.

## Révision TianSemi

1. **Pourquoi les applications constituent-elles le point de départ de cette unité ?**
2. **Quelle différence faut-il établir entre information et données ?**
3. **Pourquoi une conversion est-elle nécessaire entre l'humain et l'ordinateur ?**
4. **Quelle différence existe-t-il entre génération des données et transmission des données ?**
5. **Quels modules sont présents sur le client et sur le serveur dans l'exemple Huawei ?**
6. **Pourquoi l'ingénieur réseau doit-il adopter une vision de bout en bout ?**

### Éléments de réponse

1. Les applications répondent aux besoins humains et génèrent l'information qui devra ensuite être représentée et transportée sous forme de données.
2. L'information correspond au contenu compréhensible et à son sens ; les données sont sa représentation exploitable par un système informatique.
3. L'ordinateur traite des données numériques composées de `0` et de `1`, tandis que l'utilisateur a besoin d'une représentation comme du texte, une image, un son ou une vidéo.
4. La génération représente l'information sous forme de données ; la transmission transporte ces données entre des équipements.
5. Chaque extrémité comporte une application, une pile TCP/IP et un pilote de carte réseau : navigateur côté client et programme serveur web côté serveur.
6. Parce que la communication dépend de plusieurs modules et équipements successifs entre l'application cliente et l'application serveur.

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 46-52 : introduction du cours, objectifs, applications, données et processus de transmission client-serveur.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 7-13 : sommaire des travaux pratiques ; aucun lab n'est consacré à l'unité théorique **Applications and Data**.

## Contenus générés

- Aucun contenu web généré pour le moment.
