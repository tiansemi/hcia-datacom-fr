---
id: HUA-HCIA-DC-082
title: "Common Network Troubleshooting Cases"
official_title: "Common Network Troubleshooting Cases"
course_unit: "7.3.2"
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
source_pages: "PDF 1040-1078"
tags: [huawei, hcia, datacom, troubleshooting, dhcp, vlan, ping, ap-onboarding, sta-access]
prerequisites:
  - "[[Overview of Network Troubleshooting]]"
related:
  - "[[WLAN Fundamentals]]"
  - "[[WLAN Service Configuration Examples]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Cas courants de troubleshooting réseau

## Ordre de diagnostic dans un campus

Pour un terminal filaire, vérifiez successivement : adresse IP, communication dans le VLAN, puis accès au réseau externe. Pour une station Wi-Fi, vérifiez d’abord que le Fit AP est en ligne sur le WAC, puis le service WLAN et l’accès de la station.

![Arbre de diagnostic des accès filaire et WLAN](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/campus-troubleshooting-tree-fr.svg>)

Cet ordre évite d’investiguer le routage Internet alors que le terminal n’a pas d’adresse, ou la sécurité STA alors que l’AP n’a jamais établi CAPWAP.

## Cas 1 — Le terminal n’obtient pas d’adresse IP

### Causes principales

- liaison client–serveur DHCP interrompue ;
- configuration DHCP incorrecte ;
- pool sans adresse disponible.

### Démarche

1. Vérifier les interfaces physique et VLANIF.
2. Contrôler les erreurs croissantes et remplacer le câble si nécessaire.
3. Vérifier le mode de pool et la configuration de la passerelle.
4. Contrôler les baux utilisés et la valeur `Idle`.

```text
<HUAWEI> display interface brief | include up
<HUAWEI> display this
<HUAWEI> display ip pool interface Vlanif10
```

Si `Idle` vaut zéro, le pool est épuisé. Les options présentées incluent l’agrandissement du sous-réseau, la répartition des clients sur d’autres VLAN/pools, l’ajout de serveurs ou l’ajustement du bail.

## Cas 2 — Deux terminaux du même VLAN ne communiquent pas

### Causes possibles

- lien ou port down ;
- VLAN absent ou mauvaise affectation des ports ;
- état STP non forwarding ;
- mauvaise entrée MAC ou ARP statique ;
- isolation de ports ;
- conflit d’adresse MAC.

### Démarche

```text
<HUAWEI> display interface brief | include up
<HUAWEI> display vlan 10
<HUAWEI> display stp brief
```

Les interfaces d’entrée et de sortie doivent être up, appartenir au VLAN attendu avec un type et un marquage cohérents, et être en état `FORWARDING` si STP est actif. Un port `DISCARDING` peut être un comportement normal de prévention de boucle ; ne désactivez pas STP sans comprendre la topologie.

## Cas 3 — Le ping échoue

Huawei traite ici surtout les voisins directs : terminal vers passerelle ou deux équipements adjacents.

### Ordre de contrôle

1. paramètres de la commande et adresses dans le même sous-réseau ;
2. type de port et VLAN sur les deux extrémités ;
3. état physique et VLANIF ;
4. état STP ;
5. route aller et route retour ;
6. résolution ARP et cohérence de la table MAC ;
7. ACL, blacklist ou défaut de transmission.

```text
<HUAWEI> display port vlan
<HUAWEI> display ip interface brief
<HUAWEI> display stp brief
<HUAWEI> display ip routing-table 192.168.1.11
<HUAWEI> display arp | include 192.168.1.11
<HUAWEI> display mac-address 4c1f-cc2f-3634
```

Pour une destination directement connectée, la table doit contenir une route `Direct`. L’interface apprise dans la table MAC doit être cohérente avec la sortie associée à l’entrée ARP ; une divergence peut signaler boucle ou conflit MAC.

## Cas 4 — Le Fit AP ne passe pas en ligne

### Causes principales

- pas d’adresse IP ou adresse statique erronée ;
- rupture de connectivité entre AP et WAC ;
- échec d’authentification MAC/SN ;
- capacité de licence atteinte ;
- défaut matériel.

### Lire l’état avant d’agir

```text
<WAC> display ap all
```

| État | Interprétation |
| --- | --- |
| `normal` / `nor` | AP correctement en ligne. |
| `standby` | AP rattaché au WAC de secours. |
| `idle` | AP ajouté mais lien WAC non établi. |
| `download` | Mise à niveau en cours. |
| `fault` | Enregistrement échoué. |
| `unauth` | Authentification échouée. |
| `committing` | Services en cours de livraison. |
| `config-failed` | Configuration initiale échouée. |
| `name-conflicted` | Nom déjà utilisé. |
| `ver-mismatch` | Version AP incompatible avec le WAC. |

### Commandes discriminantes

```text
<WAC> display ap online-fail-record mac <mac-ap>
<WAC> display ip pool interface Vlanif1219 used | include <mac-ap>
<WAC> display license resource usage
<WAC> display ap global configuration
<WAC> display ap unauthorized record
```

Le record d’échec donne une cause directe : blacklist, absence de whitelist, ressource de licence, etc. Confirmez un AP non autorisé ou ajoutez-le à la whitelist uniquement après vérification de son identité.

## Cas 5 — La station Wi-Fi n’accède pas au réseau

### Causes principales

- VLAN de service absent ou incorrect dans le VAP profile ;
- VLAN pool partiellement non créé ;
- station filtrée par blacklist/whitelist ;
- forte utilisation du canal ;
- limite d’associations ou problème d’authentification ;
- file ou pilote de transmission, réseau intermédiaire ou serveur d’authentification.

### Lire la cause et l’adressage

```text
<WAC> display station online-fail-record all
<WAC> display ip pool interface Vlanif100 used | include <mac-sta>
<WAC> display radio ap-id 0
```

Le record distingue notamment timeout d’authentification, station non authentifiée, limite AP/WAC et listes d’accès.

Pour l’adressage :

- en transfert direct, le VLAN de service doit être créé et autorisé de l’AP jusqu’à la passerelle ;
- en transfert tunnel avec DHCP sur le WAC, le VLAN de service doit exister sur le WAC ;
- le VLAN de service doit rester distinct du VLAN de gestion.

Huawei indique qu’une utilisation de canal devrait généralement rester sous 80 %. Au-delà de 85 %, la transmission radio peut être affectée. Recherchez alors interférences, charge Ethernet de l’AP et volume broadcast/multicast avant d’ajuster la radio.

## Tableau de triage rapide

| Symptôme | Première preuve | Étape suivante |
| --- | --- | --- |
| Pas d’IP filaire | Interfaces/VLANIF up | Configuration puis capacité du pool. |
| Même VLAN injoignable | VLAN et ports membres | STP, MAC, isolation et ARP statique. |
| Ping direct échoué | IP/VLAN/état lien | Route, ARP puis MAC. |
| AP absent du WAC | État et fail record | IP, CAPWAP, auth, licence, matériel. |
| STA refusée | Station fail record | Sécurité, limites et listes. |
| STA associée sans IP | Bail et VLAN de service | Chemin direct/tunnel vers DHCP. |

## Mini-lab TianSemi : cinq pannes injectées

Dans un environnement isolé, injectez une panne à la fois :

1. épuiser ou réduire un pool DHCP ;
2. retirer un port du VLAN attendu ;
3. supprimer la route de retour d’un voisin ;
4. utiliser une mauvaise MAC lors de l’import d’un AP ;
5. placer une STA de test dans une blacklist.

Pour chaque panne, consignez symptôme, première commande, hypothèse, preuve, correction et test final. Restaurez l’état initial avant l’injection suivante.

## À retenir pour l’examen HCIA-Datacom

- L’ordre filaire est IP, VLAN local, puis réseau externe.
- Un échec DHCP se traite par lien, configuration, puis disponibilité du pool.
- Une communication intra-VLAN exige interfaces up, VLAN cohérent et ports forwarding.
- Un ping direct exige adresses, lien, route et résolution ARP/MAC cohérentes.
- Le dépannage AP suit son onboarding : IP, WAC/CAPWAP, authentification, licence, matériel.
- `display station online-fail-record` fournit la cause d’un échec STA.
- Le chemin du VLAN de service dépend du mode direct ou tunnel.

## Révision TianSemi

1. Dans quel ordre diagnostiquer un terminal filaire ?
2. Quelles sont les trois causes principales d’un échec DHCP ?
3. Quels états doivent présenter les ports d’un même VLAN ?
4. Comment vérifier la cohérence entre route, ARP et table MAC ?
5. Que signifient les états `idle`, `download`, `unauth` et `ver-mismatch` d’un AP ?
6. Quelle commande révèle la cause d’un échec d’onboarding AP ?
7. Quelle commande révèle la cause d’un échec d’accès STA ?
8. Comment le mode de transfert modifie-t-il le chemin du VLAN de service ?
9. À partir de quel niveau Huawei signale-t-il un risque lié à l’utilisation du canal ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1040 à 1078.
