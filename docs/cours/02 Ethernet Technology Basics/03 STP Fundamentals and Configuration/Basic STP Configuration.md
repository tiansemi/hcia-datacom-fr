---
id: HUA-HCIA-DC-021
title: "Basic STP Configuration"
official_title: "Basic STP Configuration"
course_unit: "2.3.4"
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
source_pages: "PDF 283-288"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 47-59"
tags: [huawei, hcia, datacom, stp, cli, root-bridge, path-cost]
prerequisites:
  - "[[Working Principles of STP]]"
related:
  - "[[Advanced Spanning Tree Protocols]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Configuration STP de base (Basic STP Configuration)

## Choisir le mode

```text
[HUAWEI] stp mode { stp | rstp | mstp }
```

Le switch prend en charge STP, RSTP et MSTP. Le support indique que le mode par défaut est MSTP. Pour reproduire le cas STP classique, il faut donc sélectionner explicitement `stp`.

## Planifier le root bridge

Méthode directe :

```text
[HUAWEI] stp root primary
[HUAWEI] stp root secondary
```

- `root primary` fixe automatiquement la priorité à 0 ;
- `root secondary` fixe automatiquement la priorité à 4096 ;
- ces priorités ne peuvent ensuite pas être modifiées tant que ces rôles sont configurés.

Méthode manuelle :

```text
[HUAWEI] stp priority 0
```

La priorité par défaut est 32768 et les valeurs valides respectent les multiples de 4096.

## Coût et priorité de port

```text
[HUAWEI] stp pathcost-standard { dot1d-1998 | dot1t | legacy }
[HUAWEI-GE1/0/1] stp cost cost
[HUAWEI-GE1/0/1] stp priority priority
```

- la méthode par défaut est `dot1t` ;
- tous les switchs doivent utiliser la même méthode de calcul ;
- la priorité de port par défaut est 128.

## Activer STP

```text
[HUAWEI] stp enable
```

STP, RSTP ou MSTP est activé par défaut dans le contexte du cours, mais l'état réel doit toujours être vérifié.

## Cas officiel simplifié

Trois switchs forment une topologie redondante. `SW1` doit devenir racine et `SW2` secondaire :

```text
[SW1] stp mode stp
[SW1] stp enable
[SW1] stp priority 0

[SW2] stp mode stp
[SW2] stp enable
[SW2] stp priority 4096

[SW3] stp mode stp
[SW3] stp enable
```

Le calcul doit rendre un port de `SW3` alternate/discarding selon les BID, coûts et PID de la topologie.

## Vérification

```text
<SW3> display stp brief
```

La sortie résumée du support montre notamment :

```text
MSTID  Port      Role  STP State   Protection
0      GE1/0/21  ROOT  FORWARDING  NONE
0      GE1/0/22  ALTE  DISCARDING  NONE
```

Vérifiez le rôle et l'état ; un port alternate en discarding confirme qu'un chemin redondant est bloqué logiquement.

## Mini-lab TianSemi

1. Construisez un triangle de trois switchs dans un environnement isolé.
2. Relevez l'état STP avant modification.
3. Configurez `SW1` comme root primary et `SW2` comme secondary.
4. Vérifiez `display stp brief` sur les trois équipements.
5. Identifiez root ports, designated ports et alternate port.
6. Coupez le lien actif vers la racine et mesurez la reconvergence.
7. Rétablissez le lien, puis confirmez le retour à la topologie planifiée.

## Troubleshooting

| Symptôme | Vérification |
| --- | --- |
| Le root bridge n'est pas celui prévu | Priorité effective et présence d'un BID inférieur |
| Aucun alternate n'apparaît | Topologie, état des liens et activation STP |
| Le trafic utilise le mauvais chemin | Coûts de ports et méthode `pathcost-standard` |
| Les résultats diffèrent entre switchs | Mode STP/RSTP/MSTP et méthode de coût homogènes |
| Le port attendu reste discarding | Comparer RPC, BID et PID plutôt que forcer au hasard |

## À retenir pour l'examen HCIA-Datacom

- Le mode par défaut présenté est MSTP.
- `stp root primary` et `secondary` fixent les priorités à 0 et 4096.
- La priorité de pont par défaut est 32768.
- `dot1t` est la méthode de coût par défaut.
- `display stp brief` vérifie rôles et états.
- Une configuration STP doit être planifiée et vérifiée, pas seulement activée.

## Révision TianSemi

1. Quelle commande sélectionne le mode STP classique ?
2. Quelles priorités `root primary` et `root secondary` appliquent-elles ?
3. Quelle méthode de calcul de coût est utilisée par défaut ?
4. Pourquoi tous les switchs doivent-ils utiliser la même méthode de coût ?
5. Quelle commande modifie le coût d'un port ?
6. Quelle commande résume les rôles et états STP ?
7. Que signifie un port `ALTE DISCARDING` ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 283 à 288.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 47 à 59 pour le lab officiel STP.
