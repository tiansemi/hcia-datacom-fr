---
id: HUA-HCIA-DC-EXAM-003
title: "HCIA-Datacom Weak Points Tracker"
category: "Huawei HCIA-Datacom Exam Preparation"
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
module: "10 Exam Preparation"
type: "learning-tracker"
source: "TianSemi synthesis from HCIA-Datacom V2.0 Training Material and Lab Guide"
source_pages: "Training PDF 1-1117; Lab Guide PDF 14-270"
tags: [huawei, hcia, datacom, tracker, weak-points, revision]
publication_status: "not-started"
site_url:
---

# Tracker des points faibles HCIA-Datacom V2.0

## Principe

Un point faible n’est pas « un chapitre que je n’aime pas », mais un comportement observable qui échoue : calcul incorrect, mécanisme mal expliqué, mauvaise commande choisie ou sortie mal interprétée. Ce tracker transforme chaque erreur en prochaine action.

## Échelle commune

| Score | Définition | Preuve minimale |
| --- | --- | --- |
| 0 | notion absente ou confondue | aucune réponse exploitable |
| 1 | reconnaissance passive | réponse possible avec la note |
| 2 | application guidée | exercice ou lab réussi avec aide |
| 3 | maîtrise démontrée | explication + pratique + vérification sans aide |

## Vue par domaine

| Domaine | Rappel | Application | Vérification | Dépannage | Prochaine preuve | Revue |
| --- | ---: | ---: | ---: | ---: | --- | --- |
| 01 — Fondations |  |  |  |  |  |  |
| 02 — Ethernet |  |  |  |  |  |  |
| 03 — IP |  |  |  |  |  |  |
| 04 — Sécurité et services |  |  |  |  |  |  |
| 05 — Data center |  |  |  |  |  |  |
| 06 — WLAN |  |  |  |  |  |  |
| 07 — O&M et troubleshooting |  |  |  |  |  |  |
| 08 — Campus |  |  |  |  |  |  |

Ne calculez pas uniquement une moyenne : un score 0 en sous-réseautage ou VLAN peut bloquer plusieurs domaines malgré un bon total général.

## Journal des erreurs

Dupliquez une ligne après chaque quiz, lab ou explication à froid.

| Date | Unité | Déclencheur | Erreur exacte | Cause | Correction active | Retest prévu | Résultat |
| --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |

### Catégories de cause

- **Concept** : définition ou relation entre notions incorrecte.
- **Séquence** : étapes connues mais exécutées dans le mauvais ordre.
- **Calcul** : masque, wildcard, préfixe ou coût mal calculé.
- **Commande** : mauvaise vue, syntaxe ou portée.
- **Interprétation** : sortie lue sans identifier le champ décisif.
- **Dépendance** : service étudié sans vérifier la couche inférieure.
- **Sécurité** : solution fonctionnelle mais secret, protocole ou exposition inadapté.

## Test de sortie d’un point faible

Un point peut passer de « faible » à « maîtrisé » seulement si les quatre conditions sont réunies :

1. expliquer le mécanisme avec un exemple différent du cours ;
2. résoudre une question sans indice ;
3. produire ou interpréter une preuve opérationnelle ;
4. réussir un nouveau test après un délai.

Cocher une note comme lue n’est pas une preuve.

## Questions diagnostiques rapides

### Fondations

- Puis-je expliquer encapsulation et décapsulation sans réciter les couches ?
- Puis-je distinguer le rôle d’un switch, d’un routeur, d’un WAC et d’un AP ?

### Ethernet

- Puis-je prédire les décisions MAC d’un switch sur une trame inconnue ?
- Puis-je justifier PVID, tagging et rôle STP d’un port ?

### IP

- Puis-je calculer réseau, broadcast et plage d’hôtes ?
- Puis-je expliquer pourquoi une route est choisie parmi plusieurs candidates ?

### Sécurité et services

- Puis-je prédire la première règle ACL correspondante ?
- Puis-je distinguer AAA, SSH et NAT sans mélanger leurs rôles ?

### Data center et WLAN

- Puis-je expliquer les contraintes de trafic d’un DCN de calcul intelligent ?
- Puis-je suivre séparément le trafic de gestion AP et le trafic de service STA ?

### Exploitation et campus

- Puis-je choisir entre SNMP, NETCONF, RESTCONF et SSH selon le besoin ?
- Puis-je isoler une panne sans modifier plusieurs couches à la fois ?

## Révision TianSemi

1. Pourquoi une moyenne globale peut-elle masquer un blocage critique ?
2. Quelle différence existe entre une erreur de commande et une erreur d’interprétation ?
3. Pourquoi faut-il un retest différé avant de déclarer un point maîtrisé ?
4. Comment transformer une erreur de quiz en preuve pratique ?
5. Quel point faible actuel bloque le plus grand nombre de domaines ?
6. Quelle est la prochaine action précise, réalisable et mesurable ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1 à 1117.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 14 à 270.
- [[HCIA-Datacom Revision Plan]].
