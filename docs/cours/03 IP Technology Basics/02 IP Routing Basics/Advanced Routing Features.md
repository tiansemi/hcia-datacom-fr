---
id: HUA-HCIA-DC-035
title: "Advanced Routing Features"
official_title: "Advanced Routing Features"
course_unit: "3.2.4"
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
source_pages: "PDF 433-445"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 88-94"
tags: [huawei, hcia, datacom, route-recursion, floating-route, route-summarization, cidr, null0]
prerequisites:
  - "[[Static Routes]]"
related:
  - "[[Overview of OSPF]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Fonctions avancées de routage (Advanced Routing Features)

Huawei regroupe ici trois mécanismes liés mais distincts : la récursion de route, la route statique flottante et la sommation de routes.

![Récursion, secours et sommation : trois fonctions avancées du routage](<../../../assets/diagrammes/03 IP Technology Basics/advanced-routing-features-fr.svg>)

## Récursion de route

Une route ne peut guider le transfert que si son prochain saut se résout finalement par un réseau directement connecté et une interface de sortie.

```text
[RTA] ip route-static 30.1.2.0 24 20.1.1.3
```

Si `20.1.1.3` n'est pas directement connecté, le routeur recherche une autre route permettant de l'atteindre. Sans cette route de résolution, l'entrée vers `30.1.2.0/24` reste invalide.

## Route statique flottante

Deux routes statiques peuvent viser exactement la même destination avec des prochains sauts et préférences différents.

```text
[RTA] ip route-static 20.0.0.0 30 10.1.1.2
[RTA] ip route-static 20.0.0.0 30 10.1.2.2 preference 70
```

La première conserve la préférence statique par défaut 60. La seconde, moins prioritaire avec 70, « flotte » hors de la table active tant que la première reste valide. Si le next hop principal devient injoignable, la route de secours peut être installée.

Ce mécanisme dépend de la disparition effective de la route principale. Une panne non détectée qui laisse le next hop résolu peut nécessiter un mécanisme de détection plus fin dans une conception réelle.

## Sommation de routes

La sommation remplace plusieurs routes détaillées partageant des bits de tête communs par un préfixe agrégé. Elle réduit la table et le travail de configuration.

### Méthode de calcul

1. Écrire les adresses réseau en binaire.
2. Aligner les bits.
3. Identifier les bits identiques depuis la gauche.
4. Leur nombre devient la longueur du préfixe résumé.
5. Mettre tous les bits restants à 0 pour l'adresse réseau du résumé.

Exemple du cours : `192.168.1.0/24`, `192.168.2.0/24` et `192.168.3.0/24` partagent les 22 premiers bits, ce qui permet un résumé `192.168.0.0/22`. Ce résumé couvre aussi `192.168.0.0/24` : il faut donc vérifier les réseaux supplémentaires inclus.

```text
[RTA] ip route-static 10.1.0.0 16 12.1.1.2
```

## Risque de boucle avec une route résumée

Un résumé couvre parfois des sous-réseaux qui n'existent pas. Si un routeur amont possède une route par défaut vers le routeur qui annonce le résumé, un paquet pour un sous-réseau absent peut rebondir entre les deux.

Huawei propose une route de rejet sur le routeur qui possède les routes détaillées :

```text
[RTB] ip route-static 10.1.0.0 16 0 NULL0
```

La route `NULL0` supprime localement les paquets correspondant au résumé mais à aucune route plus précise. Les routes détaillées existantes restent préférées grâce au longest prefix match.

## Mini-lab TianSemi

1. Configurez une route dont le next hop est distant et observez son absence sans route de récursion.
2. Ajoutez la route permettant d'atteindre ce next hop et vérifiez l'installation.
3. Créez deux chemins statiques vers une loopback avec préférences 60 et 100.
4. Coupez le lien principal et observez la route flottante prendre le relais.
5. Remplacez quatre routes /24 contiguës par un résumé calculé.
6. Ajoutez une route `NULL0` du même résumé et vérifiez qu'une route /24 existante reste prioritaire.

## Troubleshooting

| Symptôme | Contrôle |
| --- | --- |
| Route récursive absente | Route vers le next hop et interface finale Up |
| Route flottante toujours inactive | Route principale encore valide ou préférence de secours trop élevée numériquement |
| Deux chemins actifs alors qu'un secours était attendu | Destination, masque ou préférence identiques |
| Destination inexistante boucle | Résumé trop large, route par défaut opposée, absence de route NULL0 |
| Réseau valide rejeté | Route détaillée manquante ou résumé mal calculé |

## À retenir pour l'examen HCIA-Datacom

- La récursion résout un next hop non directement connecté.
- Une préférence plus grande rend une route statique moins prioritaire.
- Une route flottante fournit un chemin de secours.
- CIDR permet des préfixes de longueur arbitraire et la sommation.
- Le résumé se fonde sur les bits communs les plus à gauche.
- Une route vers NULL0 peut empêcher une boucle créée par un résumé trop large.

## Révision TianSemi

1. Qu'est-ce que la récursion de route doit finalement déterminer ?
2. Pourquoi une route avec un next hop distant peut-elle rester invalide ?
3. Comment la préférence transforme-t-elle une route statique en route flottante ?
4. Comment calcule-t-on la longueur d'un préfixe résumé ?
5. Pourquoi un résumé peut-il inclure des sous-réseaux non déployés ?
6. Comment une route NULL0 interrompt-elle une boucle sans bloquer les routes plus spécifiques ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 433 à 445.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 88 à 94 pour le basculement statique et la vérification du chemin.
