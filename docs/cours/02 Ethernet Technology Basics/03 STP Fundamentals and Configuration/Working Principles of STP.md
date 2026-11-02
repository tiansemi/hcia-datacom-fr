---
id: HUA-HCIA-DC-020
title: "Working Principles of STP"
official_title: "Working Principles of STP"
course_unit: "2.3.3"
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
source_pages: "PDF 266-282"
tags: [huawei, hcia, datacom, stp, root-bridge, root-port, designated-port, port-state]
prerequisites:
  - "[[Basic Concepts of STP]]"
related:
  - "[[Basic STP Configuration]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Principes de fonctionnement de STP (Working Principles of STP)

## Calcul de l'arbre en quatre étapes

![Séquence de calcul STP du root bridge au blocage du port alternatif](<../../../assets/diagrammes/02 Ethernet Technology Basics/stp-election-process-fr.svg>)

### 1. Élection du root bridge

Au démarrage, chaque switch se considère comme racine et annonce son BID. Les BPDU sont comparées jusqu'à ce que le plus petit BID soit reconnu par tous comme Root ID.

La racine peut être préemptée si un switch de BID inférieur rejoint le réseau. Huawei recommande donc de planifier la racine et d'attribuer la priorité 0 au switch choisi afin de stabiliser la conception.

### 2. Élection d'un root port par switch non racine

Chaque switch non racine choisit le port qui reçoit la meilleure BPDU, selon :

1. le plus petit RPC ;
2. le plus petit BID du switch amont ;
3. le plus petit PID du port amont ;
4. si nécessaire, le plus petit PID local.

Le root port pointe vers la racine et il n'en existe qu'un par switch non racine.

### 3. Élection d'un designated port par lien

Chaque segment choisit le port qui offre la meilleure BPDU vers la racine :

1. plus petit RPC ;
2. plus petit BID local en cas d'égalité ;
3. plus petit PID en dernier recours.

Tous les ports du root bridge sont designated dans la topologie présentée.

### 4. Blocage des ports non désignés

Tout port qui n'est ni root port ni designated port devient alternate et est bloqué. Il continue de recevoir et traiter des BPDU, mais ne transfère pas le trafic utilisateur. L'arbre logique obtenu ne contient plus de boucle.

## États des ports STP

![États d'un port STP et progression vers Forwarding](<../../../assets/diagrammes/02 Ethernet Technology Basics/stp-port-states-fr.svg>)

| État | BPDU | Apprentissage MAC | Trafic utilisateur |
| --- | --- | --- | --- |
| Disabled | Aucun traitement | Non | Non |
| Blocking | Reçoit et traite | Non | Non |
| Listening | Émet et reçoit | Non | Non |
| Learning | Traite | Oui | Non |
| Forwarding | Émet et reçoit | Oui | Oui |

Un port élu root ou designated passe normalement de Blocking à Listening, puis Learning et Forwarding. Les états transitoires évitent de créer temporairement une boucle pendant la convergence.

## Réaction aux défaillances

### Défaillance du root bridge

Les BPDU expirent après Max Age, une nouvelle racine est élue, puis le port traverse deux intervalles Forward Delay. Avec les valeurs par défaut, Huawei indique environ **50 s** : 20 + 15 + 15.

### Défaillance physique directe

Le port détecte immédiatement l'état Down. Un alternate devient root port, puis traverse Listening et Learning. Le cours indique environ **30 s**, soit deux Forward Delay.

### Défaillance non physique

Le lien peut rester physiquement Up alors que les BPDU ne passent plus. Il faut attendre Max Age, puis les deux Forward Delay : environ **50 s**.

## Changement de topologie et table MAC

Après une reconvergence, d'anciennes entrées MAC peuvent pointer vers le mauvais port. Un switch en aval émet alors des TCN BPDU. La racine propage le bit TC et les équipements réduisent le vieillissement des entrées MAC de 300 s à la valeur de Forward Delay, 15 s dans le cours. Les entrées obsolètes sont supprimées plus vite puis réapprises.

## Exercice de calcul

Trois switchs forment un triangle. Ils ont tous la priorité 4096 et les MAC croissantes `...0001`, `...0002`, `...0003`.

1. `SW1` devient racine grâce à la plus petite MAC.
2. Les ports directs de `SW2` et `SW3` vers `SW1` deviennent root ports si les coûts sont identiques.
3. Sur le lien `SW2–SW3`, le port de `SW2` devient designated car son BID est inférieur.
4. Le port correspondant de `SW3` devient alternate et reste bloqué.

## Troubleshooting

| Constat | Interprétation |
| --- | --- |
| La mauvaise racine est élue | Priorités non planifiées ou BID inférieur inattendu |
| Le mauvais lien devient root path | Coûts, méthode de calcul ou priorités à vérifier |
| Convergence proche de 50 s | Détection par Max Age puis deux Forward Delay en STP classique |
| Trafic encore incorrect après reconvergence | Entrées MAC anciennes en cours de vieillissement |
| Aucun port n'est bloqué dans une topologie physique bouclée | STP désactivé, incohérent ou non opérationnel |

## À retenir pour l'examen HCIA-Datacom

- Séquence : root bridge, root ports, designated ports, blocage des alternate.
- Un switch non racine possède un seul root port.
- Un lien possède un seul designated port.
- Seuls root et designated peuvent atteindre Forwarding.
- STP classique possède cinq états.
- Une panne directe converge plus vite qu'une panne détectée par expiration des BPDU.
- TCN/TC accélère la mise à jour des tables MAC après un changement.

## Révision TianSemi

1. Quelles sont les quatre étapes du calcul STP ?
2. Quels critères départagent les candidats root port ?
3. Quels critères départagent les candidats designated port ?
4. Quel rôle reçoit un port qui n'est ni root ni designated ?
5. Quels sont les cinq états STP ?
6. Pourquoi l'état Learning existe-t-il ?
7. Pourquoi une panne non physique peut-elle nécessiter environ 50 secondes ?
8. Comment STP accélère-t-il la correction des entrées MAC après une reconvergence ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 266 à 282.
