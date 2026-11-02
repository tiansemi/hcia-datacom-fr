---
id: HUA-HCIA-DC-069
title: "WLAN Fundamentals"
official_title: "WLAN Fundamentals"
course_unit: "6.3.1"
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
source_pages: "PDF 878-904"
tags: [huawei, hcia, datacom, wlan, ap-onboarding, capwap, sta, scanning, association, forwarding]
prerequisites:
  - "[[WLAN Networking Application Scenarios]]"
related:
  - "[[WLAN Service Configuration Procedure]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Fondamentaux du WLAN (WLAN Fundamentals)

## Le processus de fonctionnement d’un WLAN centralisé

Dans une architecture WAC + Fit AP, le service ne commence pas par la connexion du terminal. Le Fit AP doit d’abord être reconnu et pris en charge par le WAC.

![Chaîne de fonctionnement d’un WLAN centralisé](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-working-process-fr.svg>)

Huawei décompose le fonctionnement en trois phases :

1. **mise en ligne de l’AP** : adresse IP, découverte du WAC et connexion CAPWAP ;
2. **accès de la station** : détection du SSID, authentification de lien et association ;
3. **transfert des données** : acheminement local par l’AP ou centralisé par le WAC.

## Mise en ligne d’un Fit AP

La mise en ligne suit une chaîne ordonnée. Une panne en amont empêche les étapes suivantes.

### 1. Obtenir une adresse IP

L’AP doit disposer d’une adresse pour communiquer avec le WAC. Trois modes sont présentés :

- configuration statique ;
- DHCP, mode généralement utilisé sur les WLAN ;
- SLAAC, pour obtenir uniquement une adresse IPv6.

Le serveur DHCP peut être dédié, intégré au WAC ou fourni par un équipement du réseau, par exemple le switch cœur.

En DHCP IPv4, l’échange suit `Discover → Offer → Request → ACK`. L’ACK fournit notamment l’adresse, la durée du bail, la passerelle et le DNS.

### 2. Découvrir le WAC

En mode statique, une liste d’adresses WAC est préconfigurée sur l’AP. En mode dynamique, trois mécanismes existent :

| Découverte | Information nécessaire | Portée ou prérequis |
| --- | --- | --- |
| DHCP | Adresse IPv4 du WAC dans l’option 43 ; adresse IPv6 dans l’option 52 | Serveur DHCP configuré pour annoncer le WAC. |
| DNS | Domaine WAC via option 15 en IPv4 ou 24 en IPv6, plus adresse DNS | DHCP et DNS ; résolution du nom préfixé par `huawei-wlan-controller`. |
| Broadcast | Discovery Request diffusée localement | AP et WAC dans le même domaine de broadcast. |

Lorsqu’il reçoit plusieurs réponses, l’AP départage les WAC selon les informations annoncées : capacité maximale d’AP, puis capacité maximale de stations, puis plus petite adresse IP.

### 3. Établir CAPWAP

Après `Discovery Request/Response`, l’AP établit les tunnels CAPWAP. DTLS peut protéger les échanges. Le tunnel de contrôle transporte la gestion ; le tunnel de données n’est utilisé pour le trafic utilisateur que lorsque le transfert par tunnel est activé.

### 4. Contrôler l’accès de l’AP

L’AP envoie un `Join Request`. Le WAC vérifie la version IP autorisée, les listes de refus, puis le mode d’authentification :

- adresse MAC ;
- numéro de série, ou SN ;
- absence d’authentification.

Avec l’authentification MAC ou SN, l’AP peut avoir été importé hors ligne, être présent dans une liste blanche, ou attendre une confirmation manuelle dans la liste non autorisée.

### 5. Aligner la version logicielle

L’AP compare sa version à celle imposée par le WAC. Si elles diffèrent, la mise à niveau peut s’effectuer depuis le WAC, un serveur FTP ou un serveur SFTP. SFTP protège les transferts ; FTP les transmet en clair et présente donc un risque. Après mise à niveau, l’AP redémarre et reprend le processus.

### 6. Vérifier l’état de configuration

L’AP demande sa configuration initiale, l’applique puis informe le WAC de l’état des radios et du résultat. Le WAC met alors à jour ses informations sur l’AP.

### 7. Maintenir le tunnel et livrer les changements

- Les messages **Echo** sur UDP 5246 vérifient le tunnel de contrôle.
- Les messages **Keepalive** sur UDP 5247 vérifient le tunnel de données.

Le support indique un intervalle par défaut de 25 secondes et six tentatives de détection. Une perte persistante provoque la reconstruction du tunnel. Lorsqu’une configuration change, le WAC et l’AP échangent des messages `Configuration Update` avant livraison du service.

## Accès d’une station

### Scanning

Une station peut rechercher les WLAN de deux manières :

- **active** : elle envoie des `Probe Request`, avec un SSID précis ou sans SSID, puis reçoit les `Probe Response` ;
- **passive** : elle écoute les trames `Beacon` périodiques des AP. Cette méthode convient notamment aux terminaux économes en énergie.

### Authentification de lien

802.11 décrit deux mécanismes dans ce cours :

- **open system** : pas d’authentification réelle à cette étape ;
- **shared key** : la station répond à un défi chiffré avec une clé WEP prépartagée.

Le shared key dépend de WEP, technologie ancienne. Cette description sert à comprendre le protocole ; elle ne constitue pas une recommandation de sécurité moderne.

### Association

Après l’authentification de lien, la station négocie le service. Elle transmet dans l’`Association Request` ses capacités et les paramètres choisis : débits, canaux, algorithme d’authentification et chiffrement. Dans WAC + Fit AP, l’AP encapsule cette demande dans CAPWAP ; le WAC décide et renvoie l’`Association Response` par l’intermédiaire de l’AP.

L’admission peut aussi dépendre des limites globales WAC/AP, du nombre maximal de stations par VAP ou radio, et des listes noire et blanche.

## Transfert des données utilisateur

| Mode | Chemin des données | Effet principal |
| --- | --- | --- |
| Direct ou local | AP → réseau supérieur | Réduit la charge et évite un goulot d’étranglement sur le WAC. |
| Tunnel ou centralisé | AP → tunnel CAPWAP → WAC → réseau supérieur | Centralise le contrôle de sécurité et le transfert. |

Le transfert direct s’intègre bien aux grands campus convergents et aux agences qui doivent sortir localement. Le transfert par tunnel facilite une politique centralisée mais consomme la capacité du WAC et du chemin vers lui.

## Troubleshooting par phase

| Observation | Phase probable | Vérification |
| --- | --- | --- |
| L’AP n’a pas d’adresse | Allocation IP | VLAN de gestion, DHCP, relais et pool. |
| L’AP a une adresse mais ne trouve aucun WAC | Découverte | Option 43/DNS, broadcast local ou adresse statique. |
| Le WAC répond mais refuse l’AP | Contrôle d’accès | Version IP, blacklist, mode MAC/SN et admission hors ligne. |
| L’AP redémarre en boucle | Mise à niveau | Compatibilité de version et disponibilité du fichier. |
| Le SSID est visible mais l’association échoue | Accès STA | Sécurité, limite de stations, listes et capacités négociées. |
| La station est associée sans trafic | Transfert | VLAN de service, DHCP STA, mode direct/tunnel et chemin supérieur. |

## Pratique TianSemi : reconstruire la chaîne

À partir du symptôme « l’AP possède `10.1.100.25`, mais n’apparaît pas en état normal sur le WAC » :

1. éliminez l’étape d’allocation IP ;
2. testez la joignabilité vers le WAC ;
3. identifiez le mécanisme de découverte utilisé ;
4. vérifiez l’établissement CAPWAP ;
5. contrôlez le mode d’authentification de l’AP ;
6. placez dans l’ordre `Discovery`, `Join`, mise à niveau, état de configuration et maintien du tunnel.

## À retenir pour l’examen HCIA-Datacom

- Le WLAN centralisé suit : mise en ligne AP, accès STA, transfert des données.
- DHCP est le mode d’adressage généralement utilisé pour les AP.
- La découverte dynamique du WAC peut utiliser DHCP, DNS ou broadcast.
- CAPWAP sépare contrôle et données ; Echo contrôle le port 5246 et Keepalive le port 5247.
- Le WAC peut authentifier un AP par MAC, SN ou ne pas l’authentifier.
- L’accès STA suit scanning, authentification de lien puis association.
- Le transfert direct évite le WAC pour les données ; le transfert tunnel les centralise sur le WAC.

## Révision TianSemi

1. Quelles sont les trois grandes phases du fonctionnement d’un WLAN centralisé ?
2. Quels modes permettent à un AP d’obtenir son adresse IP ?
3. Comment les options DHCP 43 et 15 interviennent-elles dans la découverte du WAC ?
4. Dans quelle condition la découverte par broadcast fonctionne-t-elle ?
5. Quelles vérifications précèdent l’admission d’un AP par le WAC ?
6. À quoi servent Echo et Keepalive dans CAPWAP ?
7. Quelle différence existe entre scanning actif et passif ?
8. Que transporte une demande d’association STA ?
9. Comment distinguer transfert direct et transfert par tunnel ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 878 à 904.
