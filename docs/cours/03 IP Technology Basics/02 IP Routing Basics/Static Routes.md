---
id: HUA-HCIA-DC-034
title: "Static Routes"
official_title: "Static Routes"
course_unit: "3.2.3"
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
source_pages: "PDF 427-432"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 79-94"
tags: [huawei, hcia, datacom, static-route, default-route, next-hop, return-route]
prerequisites:
  - "[[Direct Routes]]"
related:
  - "[[Advanced Routing Features]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Routes statiques (Static Routes)

## Quand utiliser une route statique

Une route statique est créée manuellement. Elle consomme peu de ressources et convient aux topologies petites et stables, mais elle ne s'adapte pas seule à un changement de chemin.

![Une route dans chaque sens pour une communication bidirectionnelle](<../../../assets/diagrammes/03 IP Technology Basics/static-route-round-trip-fr.svg>)

Chaque routeur du trajet doit connaître la destination suivante, et le trafic retour doit disposer de ses propres routes. Ajouter uniquement la route aller ne suffit pas.

## Syntaxe Huawei

```text
[Huawei] ip route-static ip-address { mask | mask-length } nexthop-address
[Huawei] ip route-static ip-address { mask | mask-length } interface-type interface-number
[Huawei] ip route-static ip-address { mask | mask-length } interface-type interface-number [ nexthop-address ]
```

- sur une interface point à point, l'interface de sortie peut suffire ;
- sur Ethernet ou une interface VT, Huawei demande de spécifier le prochain saut ;
- fournir interface et next hop lève toute ambiguïté lorsque le contexte le justifie.

Exemple :

```text
[RTA] ip route-static 20.1.1.0 255.255.255.0 10.0.0.2
```

La route n'est installée que si son prochain saut est joignable et si elle constitue la route optimale pour cette destination.

## Route par défaut

`0.0.0.0/0` correspond à toutes les adresses, mais c'est le préfixe le moins précis. Elle n'est utilisée que si aucune route plus spécifique ne correspond.

```text
[RTA] ip route-static 0.0.0.0 0 1.2.3.254
```

Elle est typiquement placée sur un équipement de sortie d'entreprise vers un fournisseur ou sur un équipement d'extrémité qui ne possède qu'une seule issue.

## Vérifications indispensables

```text
<R1> display ip interface brief
<R1> display ip routing-table
<R1> display ip routing-table protocol static
<R1> ping -a 10.0.2.2 10.0.1.1
<R1> tracert -a 10.0.2.2 10.0.1.1
```

Spécifier l'adresse source dans `ping -a` teste explicitement le retour vers le réseau choisi. Un ping sans source peut sélectionner l'adresse de l'interface de sortie et masquer l'absence d'une route retour vers une loopback.

## Mini-lab TianSemi inspiré du lab officiel

1. Reliez trois équipements en triangle et créez une loopback /32 sur chacun.
2. Vérifiez les routes directes et tentez un ping sourcé entre loopbacks : il doit échouer sans routes distantes.
3. Ajoutez des routes statiques dans les deux sens.
4. Vérifiez leur présence avec `display ip routing-table protocol static`.
5. Remplacez une route spécifique par une route par défaut sur l'équipement d'extrémité.
6. Utilisez `tracert -a` pour confirmer le chemin réellement suivi.

## Troubleshooting

| Symptôme | Cause probable | Action |
| --- | --- | --- |
| Route configurée mais absente | Next hop non joignable | Vérifier route directe et état de lien |
| Ping aller sans réponse | Route retour absente | Inspecter la table du destinataire et des sauts retour |
| Mauvaise route utilisée | Une route plus spécifique existe | Appliquer le longest prefix match |
| Route par défaut ignorée | Une entrée plus précise correspond | Comportement normal ; examiner cette entrée |
| Ping non sourcé réussi, sourcé échoue | Retour absent vers l'adresse source choisie | Ajouter ou corriger la route retour |

## À retenir pour l'examen HCIA-Datacom

- Une route statique est simple et peu coûteuse, mais non adaptative.
- Sur Ethernet, le prochain saut doit être précisé.
- Une route configurée n'est installée que si le next hop est joignable et la route optimale.
- Une route par défaut est `0.0.0.0/0`.
- Toute communication bidirectionnelle requiert des routes aller et retour.
- `ping -a` permet de contrôler l'adresse source du test.

## Révision TianSemi

1. Dans quels réseaux les routes statiques sont-elles particulièrement adaptées ?
2. Pourquoi faut-il indiquer le prochain saut sur une interface Ethernet ?
3. Quelles conditions permettent l'installation d'une route statique ?
4. Quand la route `0.0.0.0/0` est-elle utilisée ?
5. Pourquoi un ping peut-il échouer après la configuration de la route aller ?
6. Quel intérêt présente un ping avec une adresse source imposée ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 427 à 432.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 79 à 94 pour les routes directes, statiques, de secours et par défaut.
