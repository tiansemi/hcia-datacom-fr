---
id: HUA-HCIA-DC-017
title: "VLAN Configuration Examples"
official_title: "VLAN Configuration Examples"
course_unit: "2.2.4"
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
source_pages: "PDF 219-241"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 27-46"
tags: [huawei, hcia, datacom, vlan, access, trunk, hybrid, mac-vlan, cli]
prerequisites:
  - "[[VLAN Application]]"
related:
  - "[[Basic Concepts of VLANs]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Exemples de configuration VLAN (VLAN Configuration Examples)

## Commandes fondamentales

### Créer des VLAN

```text
[Huawei] vlan 10
[Huawei-vlan10] quit
[Huawei] vlan batch 10 20
```

- `vlan vlan-id` crée le VLAN ou ouvre sa vue ;
- `vlan batch` crée plusieurs VLAN ou une plage ;
- les VID utilisables vont de 1 à 4094 ;
- VLAN 1 existe par défaut et les interfaces y appartiennent initialement dans le contexte du cours.

### Interface access

```text
[Huawei] interface GE 1/0/1
[Huawei-GE1/0/1] port link-type access
[Huawei-GE1/0/1] port default vlan 10
```

### Interface trunk

```text
[Huawei] interface GE 1/0/3
[Huawei-GE1/0/3] port link-type trunk
[Huawei-GE1/0/3] port trunk allow-pass vlan 10 20
```

La configuration facultative du PVID du trunk utilise :

```text
[Huawei-GE1/0/3] port trunk pvid vlan 1
```

Le VLAN configuré comme PVID doit aussi être autorisé si des trames non taguées doivent être acceptées.

### Interface hybrid

```text
[Huawei] interface GE 1/0/1
[Huawei-GE1/0/1] port link-type hybrid
[Huawei-GE1/0/1] port hybrid pvid vlan 10
[Huawei-GE1/0/1] port hybrid untagged vlan 10 100
[Huawei-GE1/0/1] port hybrid tagged vlan 20
```

Les listes `untagged` et `tagged` déterminent le traitement des tags en sortie.

## Exemple 1 : deux départements sur deux switchs

Objectif : les hôtes d'un même département communiquent entre les switchs ; les VLAN 10 et 20 restent isolés à la couche 2.

Sur chaque switch :

```text
[SW1] vlan batch 10 20
[SW1] interface GE 1/0/1
[SW1-GE1/0/1] port link-type access
[SW1-GE1/0/1] port default vlan 10
[SW1-GE1/0/1] quit
[SW1] interface GE 1/0/2
[SW1-GE1/0/2] port link-type access
[SW1-GE1/0/2] port default vlan 20
[SW1-GE1/0/2] quit
[SW1] interface GE 1/0/3
[SW1-GE1/0/3] port link-type trunk
[SW1-GE1/0/3] port trunk allow-pass vlan 10 20
```

La configuration de l'autre switch suit la même logique. Les numéros d'interface doivent être adaptés à la topologie réelle.

## Exemple 2 : accès partagé à un serveur avec hybrid

Huawei utilise des ports hybrid pour que les VLAN 10 et 20 accèdent à un serveur du VLAN 100 sans permettre une communication directe entre les départements.

Sur le switch des utilisateurs, les ports des hôtes reçoivent leur PVID et émettent les VLAN utilisateur et serveur sans tag :

```text
[SW1] vlan batch 10 20 100
[SW1] interface GE 1/0/1
[SW1-GE1/0/1] port link-type hybrid
[SW1-GE1/0/1] port hybrid pvid vlan 10
[SW1-GE1/0/1] port hybrid untagged vlan 10 100
[SW1-GE1/0/1] quit
[SW1] interface GE 1/0/3
[SW1-GE1/0/3] port link-type hybrid
[SW1-GE1/0/3] port hybrid tagged vlan 10 20 100
```

Le port du serveur, sur l'autre switch, possède le PVID 100 et autorise les trois VLAN en mode untagged selon l'exemple officiel.

## Exemple 3 : affectation par adresse MAC

Objectif : trois hôtes restent dans le VLAN 10 même lorsqu'ils changent de port.

```text
[SW1] vlan 10
[SW1-vlan10] mac-vlan mac-address 001e-10dd-dd01
[SW1-vlan10] mac-vlan mac-address 001e-10dd-dd02
[SW1-vlan10] mac-vlan mac-address 001e-10dd-dd03
[SW1-vlan10] quit
[SW1] interface GE 1/0/2
[SW1-GE1/0/2] port link-type hybrid
[SW1-GE1/0/2] port hybrid untagged vlan 10
[SW1-GE1/0/2] mac-vlan enable
```

Huawei recommande les interfaces hybrid pour cette méthode. Sur access ou trunk, l'affectation par MAC ne fonctionne dans le cas présenté que lorsque le VLAN déterminé par la MAC égale le PVID.

## Vérification obligatoire

```text
[SW1] display vlan
[SW1] display mac-address
[SW1] display mac-vlan mac-address all
```

- `display vlan` vérifie les VID et les ports tagged/untagged ;
- `display mac-address` montre les MAC apprises, leur VLAN et leur interface ;
- `display mac-vlan mac-address all` vérifie les associations MAC–VLAN configurées.

## Mini-lab TianSemi

1. Créez les VLAN 10 et 20 sur deux switchs.
2. Placez un hôte de chaque switch dans le VLAN 10 et un autre dans le VLAN 20.
3. Configurez les ports utilisateurs en access et la liaison inter-switchs en trunk.
4. Vérifiez `display vlan` avant tout test de connectivité.
5. Confirmez que les hôtes d'un même VLAN communiquent.
6. Confirmez qu'un hôte du VLAN 10 ne joint pas directement un hôte du VLAN 20 sans routage inter-VLAN.
7. Observez les tables MAC après les tests.

## Troubleshooting

| Symptôme | Contrôle prioritaire |
| --- | --- |
| Deux hôtes du même VLAN ne communiquent pas entre switchs | VLAN créé des deux côtés et autorisé sur les deux trunks |
| Un PC en access ne communique pas | PVID du port, état du lien et VLAN existant |
| Une trame est rejetée sur un trunk | VID absent de la liste `allow-pass` |
| Un hybrid émet avec le mauvais tag | VLAN placé dans la mauvaise liste `tagged` ou `untagged` |
| L'affectation par MAC ne s'applique pas | Association créée, `mac-vlan enable` activé et type de port adapté |
| Les VLAN semblent corrects mais le ping inter-VLAN échoue | Absence de fonction de routage inter-VLAN |

## À retenir pour l'examen HCIA-Datacom

- Créer le VLAN ne configure pas automatiquement le rôle des ports.
- Access utilise `port default vlan`.
- Trunk utilise `port trunk allow-pass vlan`.
- Hybrid distingue `port hybrid tagged vlan` et `port hybrid untagged vlan`.
- L'affectation par MAC nécessite l'association dans la vue VLAN et `mac-vlan enable` sur l'interface.
- Toute configuration doit être suivie par `display vlan` et les vérifications de table appropriées.

## Révision TianSemi

1. Quelle commande crée plusieurs VLAN en une seule opération ?
2. Quelles deux commandes configurent un port access dans le VLAN 10 ?
3. Quelle commande autorise les VLAN 10 et 20 sur un trunk ?
4. Comment choisir si un hybrid émet un VLAN tagué ou non tagué ?
5. Quelles étapes sont nécessaires pour l'affectation VLAN par MAC ?
6. Que vérifie `display vlan` ?
7. Pourquoi deux hôtes de VLAN différents ne communiquent-ils pas après la seule configuration des switchs ?
8. Quel oubli courant provoque l'échec d'un VLAN sur une liaison trunk ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 219 à 241.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 27 à 46 pour le lab officiel de configuration VLAN.
