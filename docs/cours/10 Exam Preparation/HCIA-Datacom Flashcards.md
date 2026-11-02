---
id: HUA-HCIA-DC-EXAM-005
title: "HCIA-Datacom Flashcards"
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
type: "flashcards"
source: "TianSemi synthesis from HCIA-Datacom V2.0 official materials"
source_pages: "Training PDF 1-1117; Lab Guide PDF 14-270"
tags: [huawei, hcia, datacom, flashcards, active-recall]
publication_status: "not-started"
site_url:
---

# Flashcards HCIA-Datacom V2.0

## Utilisation

Masquez la réponse, formulez votre réponse à voix haute, puis donnez un exemple ou une commande de preuve. Les cartes sont des synthèses TianSemi ; elles ne remplacent pas les explications et labs.

## 01 — Fondations

**Qu’est-ce qu’un protocole de communication ?**  
Un ensemble de règles communes qui permet aux entités d’encoder, d’échanger et d’interpréter correctement les informations.

**Quelle différence entre information et données ?**  
L’information possède un sens pour l’utilisateur ; les données en sont une représentation manipulable et transmissible par les systèmes.

**Que fait l’encapsulation ?**  
Chaque couche ajoute les informations de contrôle nécessaires avant transmission ; le récepteur effectue l’opération inverse.

**Rôle principal d’un switch Ethernet ?**  
Transférer les trames dans un LAN selon les adresses MAC et le contexte VLAN.

**Rôle principal d’un routeur ?**  
Choisir un next hop entre réseaux IP à partir de la table de routage.

**Pourquoi `display current-configuration` est-il une preuve utile ?**  
Il montre l’état de configuration actif, à comparer au plan attendu avant toute modification.

## 02 — Ethernet

**Que contient une table MAC ?**  
Des associations entre MAC, VLAN et interfaces de sortie, apprises ou configurées.

**Que fait un switch pour un unicast inconnu ?**  
Il le diffuse sur les ports autorisés du même VLAN, sauf le port d’entrée.

**Rôle du tag 802.1Q ?**  
Identifier le VLAN d’une trame transportée sur un lien qui en véhicule plusieurs.

**Rôle du PVID ?**  
Classer notamment les trames entrantes non taggées dans un VLAN donné.

**Pourquoi STP bloque-t-il certains chemins ?**  
Pour construire une topologie logique sans boucle tout en conservant des liens de secours.

**Différence entre Eth-Trunk manuel et LACP ?**  
Le mode manuel agrège sans négociation ; LACP négocie, sélectionne et surveille les membres.

## 03 — IP

**Que signifie `/27` en IPv4 ?**  
27 bits de réseau et 5 bits d’hôte, soit 32 adresses et généralement 30 hôtes utilisables.

**Quel principe de sélection de route intervient en premier ?**  
Le longest prefix match : la route la plus spécifique pour la destination.

**Que prouve une route `Direct` ?**  
Qu’un préfixe est directement connecté par une interface de niveau 3 active ; elle ne prouve pas la joignabilité de chaque hôte.

**Séquence DHCPv4 classique ?**  
Discover, Offer, Request, Acknowledge.

**Que signifie un voisin OSPF Full ?**  
Les voisins ont synchronisé leur LSDB sur ce lien ; cela ne garantit pas l’annonce de tous les préfixes souhaités.

**Rôle principal d’ICMPv6/NDP ?**  
Découverte de voisins/routeurs, résolution d’adresse, contrôle de reachability et autoconfiguration IPv6.

## 04 — Sécurité et services

**Pourquoi l’ordre des ACL compte-t-il ?**  
La première règle correspondante décide généralement de l’action ; une règle large placée tôt peut masquer les suivantes.

**Que représente une wildcard ACL ?**  
Les bits à comparer (`0`) ou ignorer (`1`) lors de la correspondance d’adresse.

**Les trois fonctions AAA ?**  
Authentication : identité ; Authorization : droits ; Accounting : traçabilité de l’usage.

**Pourquoi SSH est-il préféré à Telnet ?**  
SSH protège l’authentification et la session, alors que Telnet transmet sans chiffrement.

**Que fait le source NAT ?**  
Il remplace l’adresse source d’un flux, souvent pour permettre à des adresses privées de sortir.

**Quelle preuve confirme une traduction NAT ?**  
Une entrée de session/traduction montrant l’ancienne et la nouvelle adresse, complétée par un test de trafic.

## 05 — Data center

**Qu’est-ce qu’un DCN ?**  
Le réseau qui interconnecte les ressources de calcul, stockage, services et exploitation d’un data center.

**Pourquoi les architectures DCN ont-elles évolué ?**  
Pour répondre à la croissance est–ouest, à la virtualisation, à l’échelle et aux besoins de résilience.

**Caractéristique d’un DCN de calcul général ?**  
Il transporte des applications d’entreprise et cloud avec évolutivité et disponibilité.

**Caractéristique d’un DCN de calcul intelligent ?**  
Il sert des échanges massifs entre accélérateurs, très sensibles à la latence, au débit et aux pertes.

**Pourquoi une fabric leaf–spine est-elle utile ?**  
Elle offre des chemins courts et prévisibles, avec une capacité horizontale par ajout de nœuds.

**Idée de l’architecture dual-track ?**  
Faire coexister des voies réseau adaptées à des charges de calcul différentes plutôt que forcer un modèle unique.

## 06 — WLAN

**Différence entre BSS et ESS ?**  
Un BSS est une cellule radio ; un ESS réunit plusieurs BSS pour fournir un service WLAN étendu.

**Pourquoi 802.11 utilise CSMA/CA ?**  
Une station radio ne peut pas détecter de façon fiable toutes les collisions pendant qu’elle émet ; elle cherche donc à les éviter.

**Rôle du WAC avec des Fit AP ?**  
Centraliser le contrôle, la configuration et l’exploitation des AP.

**Rôle de CAPWAP ?**  
Établir le canal de contrôle AP–WAC et, en tunnel forwarding, transporter aussi les données utilisateur.

**Différence direct et tunnel forwarding ?**  
En direct, l’AP remet localement le trafic au VLAN de service ; en tunnel, les données traversent le tunnel CAPWAP jusqu’au WAC.

**Quels trois états vérifier dans l’ordre ?**  
AP en ligne (`display ap all`), VAP/SSID actif (`display vap all`), STA associé (`display station all`).

## 07 — O&M et dépannage

**Rôle général de SNMP ?**  
Superviser des objets de management, collecter des informations et recevoir des notifications.

**Rôle général de NETCONF ?**  
Lire et modifier des configurations structurées via RPC, souvent selon des modèles YANG.

**Rôle général de RESTCONF ?**  
Exposer des données structurées de management sous forme de ressources accessibles avec des méthodes HTTP.

**Pourquoi valider une clé hôte SSH ?**  
Pour confirmer l’identité du serveur et réduire le risque d’interception.

**Première étape d’un dépannage ?**  
Reproduire et délimiter le symptôme tout en définissant le résultat attendu.

**Quand une correction est-elle validée ?**  
Quand le test initial réussit et que des preuves de couche/service confirment la restauration sans effet secondaire.

## 08 — Campus

**Rôle du cœur de campus ?**  
Agrégation et transfert résilient à haute capacité ; dans le lab, Core1 porte aussi les passerelles et DHCP.

**Pourquoi séparer VLAN de gestion et de service ?**  
Pour isoler le contrôle/exploitation du trafic utilisateur et appliquer des politiques adaptées.

**Pourquoi valider STP avant les services IP ?**  
Une boucle ou un port bloqué inattendu peut rendre tous les diagnostics supérieurs trompeurs.

**Pourquoi tester les liens directs avant OSPF ?**  
Pour prouver l’adressage et la couche inférieure avant d’accuser le protocole dynamique.

**Que démontre le lab 16 ?**  
Qu’une panne se résout par observation et correction minimale : VLANIF arrêtée, route non annoncée, AP non autorisé ou profil SSID non lié.

**Que démontre le lab 17 ?**  
La dépendance de bout en bout entre VLAN, STP, IP, DHCP, OSPF, WLAN, NAT, SSH et sauvegarde.

## Révision TianSemi

1. Quelles cartes avez-vous répondues sans exemple ni preuve ?
2. Quelles cartes dépendent du sous-réseautage ou du modèle d’encapsulation ?
3. Quelles six cartes pourriez-vous transformer immédiatement en mini-lab ?
4. Quelle confusion revient entre état de contrôle et transfert réel ?
5. Quelles cartes doivent être retestées après un délai ?
6. Quelle nouvelle carte issue d’une erreur récente faut-il ajouter ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1 à 1117.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 14 à 270.
