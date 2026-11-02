---
id: HUA-HCIA-DC-043
title: "ACL Overview"
official_title: "ACL Overview"
course_unit: "4.1.1"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-11
updated: 2026-08-11
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 562-568"
tags: [huawei, hcia, datacom, acl, packet-filtering, qos, traffic-control]
prerequisites:
  - "[[IPv6 Address Configuration Example]]"
related:
  - "[[Fundamentals of ACL]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Vue d'ensemble des ACL (ACL Overview)

## Besoin de filtrer le trafic

Une entreprise peut devoir autoriser certains services, interdire l'accès à une ressource sensible ou classifier des flux pour la QoS. Une **Access Control List** fournit les conditions de correspondance utilisées pour distinguer ces paquets.

Une ACL est une collection ordonnée de règles. Chaque règle décrit :

- une condition, par exemple adresse source, destination, protocole ou port ;
- une action de correspondance `permit` ou `deny`.

## L'ACL ne traite pas seule le paquet

![Une ACL classe le paquet, puis un module de service applique l'action finale](<../../../assets/diagrammes/04 Network Security and Services/acl-role-and-service-fr.svg>)

L'ACL produit un résultat de correspondance. Un **module de service** exploite ensuite ce résultat : filtrage par une traffic policy, restriction FTP/Telnet, QoS, routage politique, etc.

Cette distinction est essentielle : `permit` ou `deny` dans une règle ne suffit pas toujours à conclure que le paquet sera transmis ou supprimé. Le comportement final dépend du module qui invoque l'ACL et de son action par défaut.

## Cas d'usage

| Besoin | Information à faire correspondre | Module possible |
| --- | --- | --- |
| Bloquer un sous-réseau source | Adresse source IPv4 | MQC / traffic policy |
| Autoriser uniquement FTP vers un serveur | Source, destination, TCP et port 21 | ACL avancée + politique |
| Restreindre l'administration du routeur | Adresse source | Service Telnet/STelnet/FTP |
| Différencier la QoS | Champs IP, ports ou VLAN | Traffic classifier |

## Méthode de conception

1. Définir précisément le trafic légitime et le trafic interdit.
2. Choisir les champs permettant de les distinguer.
3. Choisir le type d'ACL adapté.
4. Organiser les règles de la plus spécifique à la plus générale selon l'ordre retenu.
5. Identifier le module qui appliquera le résultat.
6. Choisir l'interface et la direction où le trafic sera observé.
7. Tester les flux autorisés et interdits.

## Pratique TianSemi

Pour un serveur financier `192.168.254.10`, concevez sans configurer :

1. la règle métier : R&D interdit, direction autorisée ;
2. les sources et destination à reconnaître ;
3. le type d'ACL requis ;
4. l'endroit où la politique sera appliquée ;
5. les tests positifs et négatifs à exécuter.

## À retenir pour l'examen HCIA-Datacom

- Une ACL est un ensemble ordonné de règles.
- Une règle contient condition et action de correspondance.
- Une ACL peut examiner des informations de couches 2, 3 et 4 selon son type.
- Le traitement s'arrête à la première règle correspondante dans l'ordre appliqué.
- Le module de service décide du traitement final du paquet.
- Une ACL doit être invoquée pour produire un effet opérationnel.

## Révision TianSemi

1. Quel problème une ACL résout-elle ?
2. Quels éléments composent une règle ACL ?
3. Pourquoi `permit` ne signifie-t-il pas toujours « transmettre » à lui seul ?
4. Quels modules peuvent invoquer une ACL ?
5. Pourquoi l'emplacement et la direction d'application comptent-ils ?
6. Quels tests faut-il prévoir avant de déployer un filtre ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 562 à 568.
