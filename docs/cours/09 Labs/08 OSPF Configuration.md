---
id: HUA-HCIA-LAB-008
title: "OSPF Configuration"
official_title: "OSPF Configuration"
category: "Huawei HCIA-Datacom Lab"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "intermediate"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-06
updated: 2026-08-11
module: "09 Labs"
type: "lab"
source: "HCIA-Datacom V2.0 Lab Guide"
source_pages: "PDF 108-124 (guide pages 106-122)"
lab_required: true
tags: [huawei, hcia, datacom, lab, ospf, neighbor, router-id, authentication, cost, default-route]
devices: [R1, S1, S2]
commands: [ospf, area, network, display ospf peer, ospf authentication-mode, default-route-advertise, ospf cost]
publication_status: "not-started"
site_url:
---

# Lab 08 — Configuration d’OSPF

## Objectif

Déployer OSPF dans l’aire 0 sur le triangle R1–S1–S2, fixer les Router IDs avec des loopbacks, authentifier les adjacences, annoncer une route par défaut et influencer le meilleur chemin par le coût.

## Topologie et adressage

![Triangle OSPF en aire 0](<../../assets/diagrammes/09 Labs/lab08-ospf-topology-fr.svg>)

Les liens et loopbacks reprennent le lab 06 : `10.0.12.0/24`, `10.0.23.0/24`, `10.0.13.0/24`, avec Router IDs S1 `10.0.1.1`, R1 `10.0.2.2`, S2 `10.0.3.3`.

## 1 — Activer OSPF dans l’aire 0

Après l’adressage et les tests des liens :

```text
[R1] ospf 1
[R1-ospf-1] area 0
[R1-ospf-1-area-0.0.0.0] network 10.0.12.2 0.0.0.255
[R1-ospf-1-area-0.0.0.0] network 10.0.23.2 0.0.0.255
[R1-ospf-1-area-0.0.0.0] network 10.0.2.2 0.0.0.0
```

Sur S1 et S2, le guide utilise des correspondances hôte `0.0.0.0` pour chaque interface et loopback. La commande `network` sélectionne les interfaces dont l’adresse primaire correspond à l’adresse et au wildcard ; elle n’annonce pas simplement un préfixe arbitraire.

```text
[R1] display ospf peer
[R1] display ip routing-table protocol ospf
```

## 2 — Stabiliser les Router IDs

```text
[R1] ospf 1 router-id 10.0.2.2
[S1] ospf 1 router-id 10.0.1.1
[S2] ospf 1 router-id 10.0.3.3
```

Redémarrez le processus OSPF selon la demande du système pour appliquer le nouveau Router ID, puis contrôlez que deux voisins sont Full sur chaque routeur du triangle.

## 3 — Authentifier les adjacences

Configurez le même algorithme, Key ID et secret aux deux extrémités de chaque lien. Utilisez un secret propre au lab, non publié :

```text
[R1-GE0/0/1] ospf authentication-mode hmac-sha256 1 cipher <SECRET-OSPF>
[S1-GE1/0/1] ospf authentication-mode hmac-sha256 1 cipher <SECRET-OSPF>
```

Le guide illustre aussi l’authentification au niveau de l’aire sur S2. Une incohérence fait tomber l’adjacence ; observez-le volontairement quelques instants, puis terminez la configuration symétrique et vérifiez le retour à Full.

## 4 — Annoncer la route par défaut

R1 joue le rôle de sortie :

```text
[R1] ospf 1
[R1-ospf-1] default-route-advertise always
[S1] display ip routing-table
[S2] display ip routing-table
```

S1 et S2 doivent apprendre `0.0.0.0/0` via OSPF. L’option `always` force l’annonce dans le scénario du lab ; en production, justifiez ce choix par la réalité de la sortie.

## 5 — Influencer le chemin

Le chemin direct R1–S2 a initialement un coût inférieur au détour par S1. Augmentez le coût de R1 `GE0/0/2` :

```text
[R1-GE0/0/2] ospf cost 10
[R1] display ip routing-table
[R1] tracert -a 10.0.2.2 10.0.3.3
```

Le trafic R1 vers la loopback S2 doit alors passer par S1. Le chemin retour peut rester direct : le routage IP n’impose pas la symétrie.

## Vérifications attendues

- `display ospf peer` : Router ID prévu et adjacences Full.
- `display ip routing-table protocol ospf` : loopbacks distantes apprises.
- Après authentification : aucune adjacence durablement interrompue.
- Sur S1/S2 : route par défaut OSPF présente.
- Après coût 10 : tracert aller R1–S1–S2.
- Après arrêt d’un lien : recalcul et maintien de la connectivité via le triangle.

## Troubleshooting

| Symptôme | Cause probable | Correction |
| --- | --- | --- |
| Aucun voisin | Interface non sélectionnée, aire ou réseau différent | Vérifier IP, wildcard, aire et `display ospf interface`. |
| Voisin tombe après sécurité | Key ID, secret ou mode incohérent | Aligner les deux extrémités sans exposer le secret. |
| Router ID inattendu | Processus non redémarré après changement | Appliquer le redémarrage contrôlé puis revérifier. |
| Route par défaut absente | Annonce non configurée | Contrôler `default-route-advertise` sur R1. |
| Chemin inchangé | Coût réglé sur la mauvaise interface | Lire le next-hop et comparer le coût cumulé des deux chemins. |

## Révision TianSemi

1. Que sélectionne réellement la commande OSPF `network` ?
2. Pourquoi utiliser les loopbacks comme Router IDs ?
3. Quelles valeurs d’authentification doivent correspondre entre voisins ?
4. Quel effet produit `default-route-advertise always` ?
5. Pourquoi un coût 10 sur R1 `GE0/0/2` détourne-t-il le trafic via S1 ?
6. Pourquoi le chemin retour peut-il rester différent ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 108 à 124 (pages 106 à 122 du guide).
