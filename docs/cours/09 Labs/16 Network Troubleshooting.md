---
id: HUA-HCIA-LAB-016
title: "Network Troubleshooting"
official_title: "Network Troubleshooting"
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
source_pages: "PDF 198-228 (guide pages 196-226)"
lab_required: true
tags: [huawei, hcia, datacom, lab, troubleshooting, dhcp, ospf, wlan]
devices: [R1, Core1, ACC1, ACC2, WAC1, AP1, STA]
commands: [display interface brief, display vlan, display ip routing-table, display ap online-fail-record all, display vap all]
publication_status: "not-started"
site_url:
---

# Lab 16 — Dépannage d’un réseau de campus

## But du lab

Appliquer une démarche de dépannage structurée à quatre incidents d’un même réseau de campus : échec DHCP d’un terminal filaire, perte de connectivité de couche 3, AP incapable de rejoindre le WAC et SSID attendu invisible pour le STA.

![Chaîne de diagnostic du réseau filaire jusqu’au service WLAN](<../../assets/diagrammes/09 Labs/lab16-troubleshooting-path-fr.svg>)

Huawei attend deux acquis : comprendre le processus élémentaire de dépannage d’un campus et maîtriser les commandes qui permettent d’isoler une panne.

## Topologie et données utiles

Core1 est le cœur de niveau 3, la passerelle des services et le serveur DHCP. ACC1 et ACC2 assurent l’accès de niveau 2. R1, Core1 et WAC1 échangent leurs routes avec OSPF. AP1 reçoit son adresse de gestion depuis Core1 et transporte les données du STA en transfert direct.

| Usage | VLAN / réseau |
| --- | --- |
| Gestion des AP | VLAN 3 — `192.168.3.0/24` |
| Core1–R1 | VLAN 10 — `192.168.10.0/30` |
| Core1–WAC1 | VLAN 11 — `192.168.11.0/30` |
| STA | VLAN 100 — `192.168.100.0/24` |
| Terminal filaire | VLAN 101 — `192.168.101.0/24` |

Les Loopbacks de Core1, R1 et WAC1 sont respectivement `10.0.1.1/32`, `10.0.2.2/32` et `10.0.3.3/32`.

## Méthode de diagnostic

Pour chaque incident :

1. reproduire le symptôme et définir le résultat attendu ;
2. commencer par les états observables, sans modifier la configuration ;
3. progresser de la liaison et du VLAN vers l’IP, le routage, puis le service ;
4. comparer l’état réel au plan de données ;
5. corriger uniquement la cause identifiée ;
6. répéter le test initial et collecter une preuve de restauration.

## Incident 1 — Le terminal filaire n’obtient pas d’adresse

### Symptôme et investigation

ACC2 ne peut pas joindre `192.168.101.254` et son `Vlanif101` ne reçoit aucune adresse. Le lien physique et le transport du VLAN 101 sont pourtant opérationnels, le client est bien en DHCP et le pool de Core1 est correctement défini.

```text
[ACC2] ping 192.168.101.254
[ACC2] display ip interface Vlanif 101
[ACC2] display interface brief | include up
[ACC2] display vlan
[Core1] display ip pool interface Vlanif101
[Core1] display interface Vlanif 101
```

La dernière commande révèle `Administratively DOWN` : le service DHCP ne peut pas fonctionner tant que l’interface passerelle est arrêtée.

### Correction et preuve

```text
[Core1] interface Vlanif 101
[Core1-Vlanif101] undo shutdown
[ACC2] display ip interface Vlanif 101
[ACC2] ping 192.168.101.254
```

La preuve attendue est une adresse `192.168.101.0/24`, un protocole `UP` et un ping réussi vers la passerelle.

## Incident 2 — La Loopback de R1 est injoignable

Les interfaces d’interconnexion sont actives et R1 apprend les routes OSPF de Core1, mais Core1 ne possède aucune route vers `10.0.2.2/32`. La relation OSPF existe : c’est l’annonce de la Loopback de R1 qui manque.

```text
[Core1] ping 10.0.2.2
[Core1] display interface brief | include up
[Core1] display ip routing-table
[R1] display ip routing-table
[R1] ospf 1
[R1-ospf-1] display this
```

```text
[R1] ospf 1
[R1-ospf-1] area 0
[R1-ospf-1-area-0.0.0.0] network 10.0.2.2 0.0.0.0
[Core1] display ip routing-table
[Core1] ping 10.0.2.2
```

La route `10.0.2.2/32` doit ensuite apparaître avec le protocole OSPF.

## Incident 3 — AP1 reste hors ligne

`display ap all` place AP1 dans l’état `unauth`. Le journal d’échec donne directement la cause : l’AP n’est pas dans la liste MAC autorisée.

```text
[WAC1] display ap all
[WAC1] display ap online-fail-record all
```

Relevez la MAC réelle de l’AP ; la valeur du guide n’est qu’un exemple.

```text
[WAC1] wlan
[WAC1-wlan] ap-id 1 ap-mac <MAC-AP1>
[WAC1-wlan-ap-1] ap-name AP1
[WAC1-wlan-ap-1] ap-group ap-group1
[WAC1] display ap all
```

La preuve attendue est l’état `nor`, avec AP1 associé à `ap-group1` et une adresse de gestion.

## Incident 4 — Le STA ne voit pas `HCIA-Guest`

Le VAP est actif, mais diffuse un autre SSID. La configuration montre que `HCIA-Guest` existe dans le profil SSID, sans être référencé par le profil VAP.

```text
[WAC1] display vap all
[WAC1] wlan
[WAC1-wlan] display this
[WAC1-wlan] vap-profile name HCIA-WLAN
[WAC1-wlan-vap-prof-HCIA-WLAN] ssid-profile HCIA-WLAN
[WAC1] display vap all
```

L’association de profil peut interrompre momentanément le service. Après confirmation, `display vap all` doit afficher `HCIA-Guest` sur les radios actives.

## Matrice cause–preuve

| Incident | Cause officielle | Correction minimale | Preuve finale |
| --- | --- | --- | --- |
| DHCP filaire | `Vlanif101` de Core1 arrêtée | `undo shutdown` | bail DHCP + ping passerelle |
| Couche 3 | Loopback R1 non annoncée | ajouter `10.0.2.2/32` à l’area 0 | route OSPF + ping |
| AP hors ligne | MAC absente de la liste autorisée | importer AP1 dans le groupe | état `nor` |
| SSID invisible | profil SSID non lié au VAP | lier `ssid-profile` | SSID correct dans le VAP |

## Vérification de bout en bout

- Le STA rejoint `HCIA-Guest`, obtient une adresse du VLAN 100 et atteint `192.168.100.254`.
- ACC2 obtient une adresse du VLAN 101 et atteint `192.168.101.254`.
- Core1, R1 et WAC1 joignent leurs Loopbacks respectives.
- AP1 est `nor` et le VAP diffuse le SSID planifié.

Ne concluez pas sur un seul ping : conservez aussi les états d’interface, les routes, le bail DHCP et les états AP/VAP comme preuves.

## Révision TianSemi

1. Pourquoi faut-il reproduire le symptôme avant toute correction ?
2. Comment distinguer un problème de transport VLAN d’une interface VLANIF arrêtée ?
3. Pourquoi une adjacency OSPF fonctionnelle ne garantit-elle pas que toutes les routes utiles sont annoncées ?
4. Quelle commande explique précisément l’échec d’enregistrement d’un AP ?
5. Quelle relation existe entre profil SSID, profil VAP et groupe AP ?
6. Quelles preuves faut-il collecter après une correction pour éviter un faux positif ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 198 à 228 (pages 196 à 226 du guide).
