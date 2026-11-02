---
id: HUA-HCIA-LAB-017
title: "Campus Network Deployment"
official_title: "Campus Network Deployment"
category: "Huawei HCIA-Datacom Lab"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "advanced"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-06
updated: 2026-08-11
module: "09 Labs"
type: "lab"
source: "HCIA-Datacom V2.0 Lab Guide"
source_pages: "PDF 229-270 (guide pages 227-268)"
lab_required: true
tags: [huawei, hcia, datacom, lab, campus, vlan, stp, ospf, dhcp, wlan, nat, ssh]
devices: [R1, Core1, ACC1, ACC2, WAC1, AP1, PC1, PC2]
commands: [display vlan, display stp brief, display ip routing-table, display ap all, display session all verbose, stelnet]
publication_status: "not-started"
site_url:
---

# Lab 17 — Déploiement complet d’un réseau de campus

## Finalité

Déployer et valider un petit campus d’entreprise d’environ 50 terminaux filaires et sans fil. Ce lab d’intégration mobilise les technologies étudiées dans tout le parcours : commutation, routage, WLAN, services IP, sécurité d’accès et exploitation.

![Architecture fonctionnelle du campus et ordre de déploiement](<../../assets/diagrammes/09 Labs/lab17-campus-deployment-fr.svg>)

## Architecture attendue

- Core1 est le root bridge, la passerelle de niveau 3 et le serveur DHCP.
- ACC1 et ACC2 sont les commutateurs d’accès de niveau 2.
- R1, Core1 et WAC1 utilisent OSPF pour l’interconnexion de niveau 3.
- WAC1 contrôle AP1 à travers un réseau de gestion de niveau 3.
- Le trafic du STA est transféré directement par AP1.
- La politique NAT autorise uniquement le réseau filaire à sortir.
- Core1 administre ACC1 et ACC2 en STelnet.
- Le guide sauvegarde la configuration de Core1 sur un serveur FTP hébergé par WAC1.

## Plan de données

| VLAN | Usage | Passerelle / lien |
| --- | --- | --- |
| 2 | gestion ACC1/ACC2 | Core1 `192.168.2.254/24` |
| 3 | gestion AP | Core1 `192.168.3.254/24` |
| 10 | Core1–R1 | `192.168.10.1/30` ↔ `192.168.10.2/30` |
| 11 | Core1–WAC1 | `192.168.11.1/30` ↔ `192.168.11.2/30` |
| 100 | service sans fil | Core1 `192.168.100.254/24` |
| 101 | service filaire | Core1 `192.168.101.254/24` |

Loopbacks : Core1 `10.0.1.1/32`, R1 `10.0.2.2/32`, WAC1 `10.0.3.3/32`.

## Ordre de déploiement

L’ordre officiel limite les dépendances cachées :

1. préparation et nommage des équipements ;
2. VLAN et rôles des interfaces ;
3. STP avec Core1 comme racine ;
4. adressage IP et tests des liens directs ;
5. DHCP pour AP, STA et terminal filaire ;
6. OSPF entre les équipements de niveau 3 ;
7. service WLAN ;
8. NAT sortant sélectif ;
9. administration distante SSH ;
10. sauvegarde de configuration.

Validez chaque couche avant la suivante. Un service WLAN ne compensera jamais un VLAN de gestion ou un routage défaillant.

## 1 — VLAN et STP

Créez uniquement les VLAN nécessaires, placez les ports terminaux en access et les interconnexions en trunk avec une liste autorisée explicite.

```text
[Core1] vlan batch 2 3 10 11 100 101
[Core1] stp enable
[Core1] stp root primary
[Core1] display vlan
[Core1] display stp brief
```

La sortie STP doit confirmer Core1 comme racine et montrer les interfaces utiles en forwarding. Vérifiez le PVID des ports AP et terminaux ainsi que les VLAN taggés sur chaque trunk.

## 2 — Adressage et DHCP

Configurez les VLANIF et Loopbacks selon le plan, puis testez chaque voisin directement connecté avant d’activer OSPF.

```text
[Core1] dhcp enable
[Core1] interface Vlanif 3
[Core1-Vlanif3] dhcp select interface
[Core1] interface Vlanif 100
[Core1-Vlanif100] dhcp select interface
[Core1] interface Vlanif 101
[Core1-Vlanif101] dhcp select interface
```

Les pools d’interface desservent respectivement AP1, PC1/STA et PC2. Les passerelles ne doivent jamais être attribuées aux clients.

```text
[Core1] display ip pool interface Vlanif3 used
[Core1] display ip pool interface Vlanif100 used
[Core1] display ip pool interface Vlanif101 used
```

## 3 — Routage OSPF

Activez OSPF process 1, utilisez les Loopbacks comme router IDs et annoncez les interconnexions ainsi que les réseaux devant être joignables. Contrôlez les voisins avant les routes.

```text
[Core1] display ospf peer brief
[Core1] display ip routing-table protocol ospf
[R1] display ip routing-table
[Core1] ping 10.0.2.2
[Core1] ping 10.0.3.3
```

R1 doit apprendre les réseaux de gestion et de service via Core1. WAC1 doit pouvoir joindre le réseau de gestion d’AP1.

## 4 — WLAN invité

Le plan officiel emploie le VLAN 3 pour la gestion AP, le VLAN 100 pour les clients, `ap-group1`, le SSID `HCIA-Guest` et le transfert direct.

```text
[WAC1] capwap source interface Vlanif 11
[WAC1] wlan
[WAC1-wlan] ap auth-mode mac-auth
[WAC1-wlan] ap-id 1 ap-mac <MAC-AP1>
[WAC1-wlan-ap-1] ap-name AP1
[WAC1-wlan-ap-1] ap-group ap-group1
[WAC1] display ap all
[WAC1] display vap all
[WAC1] display station all
```

Adaptez le code pays à la réglementation locale et remplacez le secret d’exemple par une PSK de lab robuste. La MAC d’AP1 doit être relevée sur votre équipement.

## 5 — NAT sortant sélectif

Le scénario attribue à R1 l’adresse publique de démonstration `1.2.3.4/24` et simule un service public sur WAC1 avec `1.2.3.100/24`. La règle ne traduit que `192.168.101.0/24` : le réseau filaire peut sortir, le réseau sans fil ne le peut pas.

```text
[R1] nat-policy
[R1-policy-nat] rule name policy_nat1
[R1-policy-nat-rule-policy_nat1] source-address 192.168.101.0 24
[R1-policy-nat-rule-policy_nat1] action source-nat easy-ip
[R1] interface GE 0/0/1
[R1-GE0/0/1] nat enable
[R1] ospf 1
[R1-ospf-1] default-route-advertise always
[R1] display session all verbose
```

Ces adresses sont réservées au scénario : ne les réutilisez pas comme plan public réel. La table de session doit montrer la traduction de PC2 vers `1.2.3.4`, mais aucune traduction pour PC1.

## 6 — Exploitation sécurisée

Le guide configure STelnet sur ACC1 et ACC2, puis se connecte depuis Core1. Utilisez des comptes distincts, une interface source de gestion restrictive et vérifiez l’empreinte de clé avant confiance.

```text
[ACC1] user-interface vty 0 4
[ACC1-ui-vty0-4] authentication-mode aaa
[ACC1-ui-vty0-4] protocol inbound ssh
[ACC1] stelnet server enable
<Core1> stelnet 192.168.2.1 1025
[ACC1] display users
```

Le Lab Guide utilise ensuite FTP pour sauvegarder `test1.cfg` sur WAC1 et signale lui-même que ce protocole n’est pas sûr. Reproduisez FTP uniquement dans un environnement isolé si l’objectif pédagogique l’exige ; privilégiez SFTP/SCP ou une plateforme de sauvegarde sécurisée en exploitation.

```text
<Core1> save test1.cfg
<Core1> dir
```

Ne placez aucun mot de passe réel dans la note, un script ou Git.

## Recette de validation

| Test | Résultat attendu |
| --- | --- |
| PC1 rejoint `HCIA-Guest` | adresse `192.168.100.0/24`, passerelle joignable |
| PC2 en DHCP | adresse `192.168.101.0/24`, passerelle joignable |
| PC1 ↔ PC2 | connectivité inter-VLAN réussie |
| PC2 → `1.2.3.100` | réussite avec session NAT |
| PC1 → `1.2.3.100` | échec conforme à la politique |
| WAC1 | AP1 `nor`, VAP actif, STA visible |
| Core1 → ACC1/ACC2 | connexion STelnet authentifiée |
| Sauvegarde | fichier présent localement et sur la cible choisie |

## Troubleshooting transversal

- Pas de DHCP : vérifier port, VLAN, trunk, état VLANIF, pool et bail.
- Voisin OSPF absent : vérifier lien, adressage `/30`, area et network statement.
- AP offline : contrôler DHCP de gestion, routage vers le WAC, CAPWAP et authentification MAC.
- SSID sans client : contrôler profils SSID/security/VAP, VLAN de service et domaine réglementaire.
- NAT absent : vérifier source de la règle, interface NAT, route par défaut et trajet retour.
- STelnet refusé : vérifier AAA, service-type, VTY, reachability et port d’écoute.

## Révision TianSemi

1. Pourquoi déployer et valider le campus dans cet ordre précis ?
2. Quels rôles distincts remplissent les VLAN 2, 3, 10, 11, 100 et 101 ?
3. Quelles preuves démontrent que le plan de contrôle OSPF et le transfert IP fonctionnent ?
4. Pourquoi PC2 peut-il atteindre le réseau public simulé alors que PC1 ne le peut pas ?
5. Quels contrôles permettent de distinguer un AP hors ligne d’un SSID mal construit ?
6. Pourquoi FTP doit-il être limité au lab et remplacé en exploitation ?
7. Quelles données faut-il sauvegarder pour pouvoir auditer la recette du campus ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 229 à 270 (pages 227 à 268 du guide).
