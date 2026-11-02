---
id: HUA-HCIA-DC-EXAM-002
title: "HCIA-Datacom Command Checklist"
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
type: "command-checklist"
source: "TianSemi synthesis from HCIA-Datacom V2.0 Training Material and Lab Guide"
source_pages: "Training PDF 87-1078; Lab Guide PDF 14-270"
tags: [huawei, hcia, datacom, vrp, commands, checklist]
publication_status: "not-started"
site_url:
---

# Checklist de commandes HCIA-Datacom V2.0

## Mode d’emploi

Une commande n’est acquise que si vous savez répondre à trois questions : dans quelle vue l’exécuter, quelle information chercher et quelle décision prendre selon la sortie. Cette checklist privilégie les commandes de configuration et de preuve réellement mobilisées dans les unités et labs.

## Prise en main VRP

- [ ] `system-view` — passer de la vue utilisateur à la configuration.
- [ ] `sysname <NOM>` — identifier l’équipement.
- [ ] `display current-configuration` — lire la configuration active.
- [ ] `display this` — contrôler la section courante.
- [ ] `display version` — identifier plateforme et version VRP.
- [ ] `display device` — vérifier les composants reconnus.
- [ ] `display interface brief` — obtenir un état synthétique des interfaces.
- [ ] `save` — sauvegarder explicitement la configuration.
- [ ] `dir` — vérifier les fichiers présents.

## Ethernet, VLAN et STP

- [ ] `display mac-address` — lire la table MAC et ses interfaces de sortie.
- [ ] `vlan batch <LISTE>` — créer plusieurs VLAN.
- [ ] `port link-type access|trunk|hybrid` — définir le comportement du port.
- [ ] `port default vlan <ID>` — affecter le VLAN d’un port access.
- [ ] `port trunk pvid vlan <ID>` — définir le PVID d’un trunk.
- [ ] `port trunk allow-pass vlan <LISTE>` — limiter les VLAN transportés.
- [ ] `display vlan` — contrôler appartenance, tag et état.
- [ ] `display port vlan` — vérifier le comportement VLAN d’un port.
- [ ] `stp enable` — activer STP.
- [ ] `stp root primary|secondary` — influencer la racine.
- [ ] `display stp` — analyser racine, coûts et timers.
- [ ] `display stp brief` — lire rôles et états des ports.
- [ ] `interface Eth-Trunk <ID>` — créer l’agrégat logique.
- [ ] `mode lacp-static` — activer LACP statique.
- [ ] `eth-trunk <ID>` — joindre une interface membre.
- [ ] `display eth-trunk <ID>` — vérifier membres sélectionnés et état.

## IPv4, DHCP et diagnostic

- [ ] `ip address <IP> <MASK>` — adresser une interface de niveau 3.
- [ ] `undo portswitch` — convertir un port compatible en interface routée.
- [ ] `display ip interface brief` — contrôler adresses et états IPv4.
- [ ] `display arp` — vérifier la résolution IP–MAC.
- [ ] `ping [-a <SOURCE>] <DESTINATION>` — tester avec une source maîtrisée.
- [ ] `tracert <DESTINATION>` — observer le chemin de niveau 3.
- [ ] `dhcp enable` — activer globalement DHCP.
- [ ] `dhcp select interface` — utiliser le pool de l’interface.
- [ ] `ip address dhcp-alloc` — agir comme client DHCP.
- [ ] `display ip pool interface <VLANIF> [used]` — vérifier pool et baux.

## Routage statique et OSPF

- [ ] `ip route-static <PREFIX> <MASK> <NEXT-HOP>` — créer une route statique.
- [ ] `ip route-static 0.0.0.0 0 <NEXT-HOP>` — créer une route par défaut.
- [ ] `display ip routing-table` — lire protocole, préférence, coût et next hop.
- [ ] `display ip routing-table <PREFIX>` — cibler une destination.
- [ ] `ospf 1 router-id <RID>` — créer le processus OSPF.
- [ ] `area 0` puis `network <IP> <WILDCARD>` — activer les interfaces correspondantes.
- [ ] `display ospf peer brief` — contrôler les voisinages.
- [ ] `display ospf interface` — vérifier paramètres et état OSPF.
- [ ] `display ospf lsdb` — examiner la base d’états de liens.
- [ ] `display ip routing-table protocol ospf` — isoler les routes apprises.

## IPv6

- [ ] `ipv6` — activer IPv6 dans la vue concernée.
- [ ] `ipv6 address <PREFIX>/<LONGUEUR>` — configurer une adresse globale.
- [ ] `ipv6 address auto link-local` — générer la link-local.
- [ ] `ipv6 address auto global` — utiliser l’autoconfiguration disponible.
- [ ] `display ipv6 interface brief` — vérifier les adresses et états.
- [ ] `display ipv6 neighbors` — contrôler NDP.
- [ ] `ping ipv6 <DESTINATION>` — tester la connectivité IPv6.
- [ ] `display ipv6 routing-table` — lire les routes IPv6.

## ACL, AAA et administration

- [ ] `acl 2000|3000` — entrer dans une ACL basic ou advanced.
- [ ] `rule <SEQ> permit|deny ...` — créer une règle ordonnée.
- [ ] `display acl all` — vérifier règles et compteurs.
- [ ] `traffic-filter inbound|outbound acl <ID>` — appliquer l’ACL au bon sens.
- [ ] `aaa` — entrer dans le modèle AAA local.
- [ ] `local-user <USER> password irreversible-cipher <SECRET>` — créer un secret local sans l’exposer.
- [ ] `local-user <USER> service-type ssh` — limiter le service autorisé.
- [ ] `user-interface vty 0 4` — configurer les accès distants.
- [ ] `authentication-mode aaa` — imposer AAA sur les VTY.
- [ ] `protocol inbound ssh` — refuser Telnet sur ces VTY.
- [ ] `stelnet server enable` — activer le serveur SSH Huawei.
- [ ] `display users` — contrôler les sessions actives.

Ne placez jamais un secret réel dans une note ou un dépôt Git. Telnet et FTP apparaissent dans le programme pour leur fonctionnement et leurs labs historiques ; préférez SSH et SFTP/SCP en exploitation.

## NAT

- [ ] `nat address-group ...` — définir un pool quand le scénario l’exige.
- [ ] `nat-policy` — construire une politique NAT.
- [ ] `source-address <PREFIX> <LONGUEUR>` — sélectionner la source.
- [ ] `action source-nat easy-ip` — traduire vers l’adresse de l’interface.
- [ ] `nat enable` — activer NAT sur l’interface prévue par la plateforme/scénario.
- [ ] `display nat session all` ou `display session all verbose` — prouver la traduction.

## WLAN

- [ ] `capwap source interface <INTERFACE>` — définir la source du tunnel de contrôle.
- [ ] `wlan` — entrer dans la configuration WLAN.
- [ ] `ap auth-mode mac-auth` — choisir l’authentification AP du lab.
- [ ] `ap-id <ID> ap-mac <MAC>` — importer l’AP réel.
- [ ] `ap-group name <GROUPE>` — créer le groupe AP.
- [ ] `security-profile name <PROFIL>` — définir la sécurité.
- [ ] `ssid-profile name <PROFIL>` puis `ssid <NOM>` — définir le SSID.
- [ ] `vap-profile name <PROFIL>` — assembler SSID, sécurité, VLAN et transfert.
- [ ] `display ap all` — vérifier l’état `nor`.
- [ ] `display ap online-fail-record all` — expliquer un échec d’enregistrement.
- [ ] `display vap all` — vérifier VAP, radio et SSID.
- [ ] `display station all` — vérifier les clients associés.

Adaptez toujours le code pays à la réglementation locale.

## O&M et automatisation

- [ ] `display logbuffer` — lire les événements récents.
- [ ] `display alarm active` — examiner les alarmes actives si supporté.
- [ ] `snmp-agent` et commandes associées — uniquement selon le modèle de sécurité retenu.
- [ ] vérifier la reachability SSH avant Paramiko.
- [ ] désactiver temporairement la pagination pour une collecte contrôlée.
- [ ] valider l’empreinte de clé hôte ; ne pas accepter aveuglément une clé inconnue.

## Drill final

Pour chaque famille, choisissez une commande et rédigez :

1. le symptôme qui justifie son exécution ;
2. le champ exact à observer ;
3. deux sorties possibles ;
4. la prochaine commande différente pour chaque sortie.

## Révision TianSemi

1. Pourquoi `display interface brief` ne suffit-il pas à prouver un service DHCP ?
2. Quelle suite de commandes sépare une panne VLAN d’une panne de routage ?
3. Quelle preuve confirme qu’une route OSPF est installée, et pas seulement qu’un voisin existe ?
4. Comment vérifier séparément AP, VAP et STA ?
5. Pourquoi faut-il contrôler le sens d’application d’une ACL ?
6. Quelles commandes de cette liste présentent un risque si elles sont exécutées sans plan de retour ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 87 à 1078.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 14 à 270.
