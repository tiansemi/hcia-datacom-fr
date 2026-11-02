---
id: HUA-HCIA-DC-081
title: "Overview of Network Troubleshooting"
official_title: "Overview of Network Troubleshooting"
course_unit: "7.3.1"
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
source_pages: "PDF 1027-1039"
tags: [huawei, hcia, datacom, troubleshooting, incident, severity, ping, tracert, maintenance]
prerequisites:
  - "[[Example for Network Programming Using Paramiko]]"
related:
  - "[[Common Network Troubleshooting Cases]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Vue d’ensemble du troubleshooting réseau

## Un défaut se traite comme une enquête contrôlée

Un incident réseau est toute situation dans laquelle le réseau ne fournit plus les fonctions essentielles attendues et dégrade un service. Le symptôme visible — interruption, perte, boucle, instabilité de protocole ou anomalie de routage — ne révèle pas automatiquement la cause.

![Procédure générale de troubleshooting réseau](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/network-troubleshooting-process-fr.svg>)

Huawei propose cinq étapes :

1. déterminer la sévérité ;
2. collecter les informations ;
3. localiser la cause ;
4. corriger le défaut ;
5. vérifier la restauration du service.

## 1 — Déterminer la sévérité

Commencez par l’impact, pas par une commande au hasard.

- Tous les utilisateurs sont-ils affectés ?
- Les utilisateurs touchés partagent-ils un port, un équipement, un bâtiment ou un service ?
- Les autres services fonctionnent-ils ?
- L’incident s’étend-il ou reste-t-il stable ?

| Niveau Huawei | Impact résumé |
| --- | --- |
| Critical | Interruption importante à l’échelle d’une zone entière. |
| Major | Site de taille moyenne ou site VIP affecté. |
| Minor | Petit site affecté. |
| Warning | Défaut dispersé ou qualité de couverture dégradée. |

La sévérité détermine la priorité, l’escalade et l’ampleur des mesures de confinement.

## 2 — Collecter les faits

Conservez au minimum :

- topologie, équipements amont/aval et interfaces ;
- état avant et après l’incident ;
- heure, symptômes et action ayant précédé le défaut ;
- modèle, version logicielle et état matériel ;
- configuration pertinente et journaux.

```text
<HUAWEI> display diagnostic-information
<HUAWEI> display version
<HUAWEI> display current-configuration
<HUAWEI> display saved-configuration
<HUAWEI> display clock
<HUAWEI> display trapbuffer
```

Collectez avant une modification, sinon vous risquez d’effacer la preuve de la cause initiale.

## 3 — Localiser la cause

L’étendue fournit une première hypothèse :

- un seul terminal : poste, câble, port, alimentation ;
- plusieurs terminaux d’une zone : équipement amont ou configuration commune ;
- tous les terminaux : liaison structurante, équipement critique ou attaque.

Huawei présente quatre méthodes :

| Méthode | Principe | Précaution |
| --- | --- | --- |
| Observation | Alarmes, voyants, états et compteurs | Corréler avec l’heure du défaut. |
| Élimination | Désactiver ou isoler une hypothèse | Uniquement en lab ou fenêtre autorisée, avec rollback. |
| Comparaison | Comparer au composant ou état normal | Garder tous les autres paramètres identiques. |
| Swap | Remplacer par un élément connu fonctionnel | Étiqueter, sauvegarder et suivre le déplacement du défaut. |

Une bonne hypothèse doit produire un test discriminant : son résultat confirme ou élimine une famille de causes.

## 4 — Corriger et contenir

Isolez d’abord le défaut s’il peut se propager. Corrigez ensuite la cause identifiée : câble, composant, configuration ou logiciel. Ne changez qu’un ensemble cohérent de paramètres et gardez une voie de retour.

Une remise à zéro ou un remplacement matériel n’est justifié qu’après collecte et localisation suffisantes.

## 5 — Vérifier et capitaliser

La disparition du symptôme initial ne suffit pas. Vérifiez :

- le service de bout en bout pour tous les groupes affectés ;
- l’absence de nouveau défaut ;
- les performances et alarmes ;
- la persistance après sauvegarde ou redémarrage lorsque cela est pertinent.

Documentez ensuite chronologie, cause racine, correction, preuves et mesure préventive.

## Commandes de maintenance orientées questions

| Question | Commande |
| --- | --- |
| Les composants sont-ils présents et opérationnels ? | `display device`, `display health` |
| Les interfaces sont-elles up et perdent-elles des paquets ? | `display interface`, `display interface brief` |
| Quelle version et quels correctifs sont actifs ? | `display version`, `display patch-information` |
| CPU ou mémoire sont-ils saturés ? | `display cpu-usage`, `display memory-usage` |
| Quelle configuration est active ou sauvegardée ? | `display current-configuration`, `display saved-configuration` |
| Quelles alarmes ou logs ont été enregistrés ? | `display trapbuffer`, `display logfile buffer`, `display diag-logfile buffer` |
| Quel est l’état d’un AP ou des stations ? | `display ap all`, `display ap run-info`, `display station` |

Les commandes et leurs vues peuvent varier selon le modèle et la version ; vérifiez la documentation du produit concerné.

## Ping et tracert : interpréter plutôt qu’exécuter

```text
<HUAWEI> ping -a <source> -c <nombre> -s <taille> -t <timeout> <destination>
<HUAWEI> tracert -a <source> -f <ttl-initial> -m <ttl-max> -q <sondes> -w <timeout> <destination>
```

- `ping` teste la joignabilité IP et mesure pertes et délais.
- `-a` fixe la source et permet de tester un chemin ou une politique précise.
- `-f` interdit la fragmentation et aide à tester un problème de MTU.
- `tracert` augmente le TTL pour révéler les sauts ; un `*` signifie absence de réponse dans le délai, pas nécessairement perte du trafic de transit.

Un ping réussi prouve un échange ICMP entre deux adresses, pas le fonctionnement de toutes les applications. Un échec peut provenir du chemin, du retour, d’une ACL ou d’un filtrage ICMP.

## Pratique TianSemi : fiche d’incident

Simulez une perte d’accès pour un seul VLAN et remplissez :

1. impact et niveau de sévérité ;
2. chronologie ;
3. topologie et interfaces concernées ;
4. état et configuration collectés avant changement ;
5. trois hypothèses classées ;
6. test discriminant pour chacune ;
7. correction et rollback ;
8. preuves de restauration et action préventive.

## À retenir pour l’examen HCIA-Datacom

- La procédure suit sévérité, collecte, localisation, correction et vérification.
- L’étendue de l’impact oriente la première hypothèse.
- Observation, élimination, comparaison et swap sont quatre méthodes de localisation.
- Il faut collecter les preuves avant de modifier l’état du réseau.
- Ping teste la joignabilité ; tracert aide à localiser le saut où les réponses cessent.
- La correction doit être suivie d’une validation de service et d’un rapport.

## Révision TianSemi

1. Quelles sont les cinq étapes du troubleshooting Huawei ?
2. Comment déterminer l’étendue et la sévérité d’un incident ?
3. Quelles informations faut-il collecter avant une modification ?
4. Quelle différence existe entre élimination, comparaison et swap ?
5. Pourquoi faut-il isoler un défaut avant de le corriger ?
6. Que prouve réellement un ping réussi ?
7. Que signifie un astérisque dans tracert ?
8. Quelles preuves permettent de conclure à la restauration complète ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1027 à 1039.
