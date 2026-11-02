---
id: HUA-HCIA-DC-072
title: "Basic Concepts of Network Management"
official_title: "Basic Concepts of Network Management"
course_unit: "7.1.1"
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
source_pages: "PDF 937-942"
tags: [huawei, hcia, datacom, network-management, nms, agent, fcaps, monitoring]
prerequisites:
  - "[[WLAN Service Configuration Examples]]"
related:
  - "[[SNMP]]"
  - "[[NETCONF]]"
  - "[[RESTCONF]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Concepts de base de la gestion de réseau (Basic Concepts of Network Management)

## Pourquoi gérer le réseau comme un système

La gestion de réseau ne consiste pas seulement à configurer chaque équipement. Elle combine surveillance, test, configuration, analyse, évaluation et contrôle afin de maintenir le fonctionnement de l’ensemble du réseau.

![Architecture NMS et fonctions FCAPS de la gestion de réseau](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/network-management-fcaps-fr.svg>)

Une plateforme de gestion agrège l’état des équipements et des liaisons. Elle permet ainsi de passer d’une lecture locale — « ce switch fonctionne-t-il ? » — à une lecture globale — « le service de bout en bout est-il disponible et performant ? ».

## Architecture type d’un système de gestion

### Équipement de gestion : NMS

Le **Network Management System** est le centre de gestion. Huawei le présente selon une architecture client/serveur :

- le client fournit l’interface utilisée par l’administrateur ;
- le serveur exécute la plateforme de gestion sur une machine physique ou virtuelle.

### Équipement géré : agent

Le routeur, switch, WAC ou autre équipement exécute un processus **agent**. Cet agent maintient les informations locales, répond aux demandes du NMS et peut lui signaler des événements.

Le NMS et les agents échangent au moyen de protocoles de gestion. Le choix du protocole dépend de l’objectif : observer, modifier une configuration, automatiser ou collecter des données à grande fréquence.

## Les cinq fonctions FCAPS

| Fonction | Question opérationnelle | Exemples |
| --- | --- | --- |
| Configuration | Quelle configuration doit être appliquée ? | Interfaces, VLAN, routes, politiques. |
| Performance | Le réseau fournit-il le niveau de service attendu ? | Débit, latence, perte, charge CPU. |
| Fault — pannes | Où se trouve le défaut et comment le résoudre ? | Alarmes, isolation, correction. |
| Security — sécurité | Qui peut accéder au réseau et à sa gestion ? | Authentification, contrôle d’accès, chiffrement. |
| Accounting — comptabilité | Qui consomme quelles ressources ? | Statistiques d’usage et imputation des coûts. |

Le Training Material se concentre surtout sur la **gestion de configuration** ; les autres fonctions sont approfondies dans les parcours supérieurs.

## Configuration et monitoring : outils disponibles

| Mécanisme | Configuration | Monitoring | Positionnement |
| --- | :---: | :---: | --- |
| CLI via SSH/Telnet | Oui | Oui | Direct et précis, mais difficile à industrialiser équipement par équipement. |
| Interface web | Oui | Oui | Graphique et économique sur un petit réseau. |
| SNMP | `Set` | Requêtes et notifications | Gestion centralisée et interopérable, surtout orientée supervision. |
| NETCONF | Opérations d’édition | Opérations de lecture | Configuration structurée, transactions et modèles de données. |
| RESTCONF | Méthodes HTTP | `GET` | API RESTful familière aux applications IT. |
| Telemetry | Non dans ce contexte | Oui, à haute fréquence | Collecte distante rapide de données. |
| Syslog | Non | Oui | Centralisation des journaux et événements. |

Les protocoles ne s’excluent pas. Une exploitation réelle combine souvent SSH pour l’intervention ponctuelle, un protocole structuré pour l’automatisation, SNMP ou télémétrie pour la supervision, et Syslog pour les événements.

## Limites de la gestion équipement par équipement

CLI et interface web ne nécessitent pas de serveur supplémentaire et conviennent aux petits réseaux. Leur coût humain augmente cependant avec :

- le nombre d’équipements ;
- l’étendue géographique ;
- la diversité des constructeurs et interfaces ;
- le besoin de surveillance en temps réel ;
- la nécessité d’appliquer une politique cohérente.

Une plateforme centralisée devient alors nécessaire pour éviter les configurations divergentes et réduire le temps de localisation des pannes.

## Pratique TianSemi : concevoir le plan de gestion

Pour un campus de 50 switches, 30 AP, deux WAC et quatre routeurs :

1. identifiez le NMS et les agents ;
2. associez un mécanisme à chaque fonction FCAPS ;
3. choisissez ce qui doit rester accessible en CLI ;
4. indiquez les données à collecter périodiquement et les événements à recevoir immédiatement ;
5. séparez le réseau de gestion du trafic utilisateur ;
6. définissez les preuves permettant de confirmer que la gestion fonctionne de bout en bout.

## À retenir pour l’examen HCIA-Datacom

- La gestion de réseau couvre logiciels et matériels et vise l’état global du réseau.
- Le NMS centralise la gestion ; les équipements gérés exécutent des agents.
- FCAPS regroupe configuration, performance, pannes, sécurité et comptabilité.
- CLI et web conviennent aux petits environnements mais évoluent mal à grande échelle.
- SNMP, NETCONF et RESTCONF peuvent tous contribuer à la gestion de configuration selon des mécanismes différents.
- Telemetry collecte rapidement des données ; Syslog centralise les journaux.

## Révision TianSemi

1. Quelle différence existe entre gérer un équipement et gérer un réseau ?
2. Quels rôles remplissent le client NMS, le serveur NMS et l’agent ?
3. Que signifie l’acronyme FCAPS ?
4. Pourquoi la CLI devient-elle insuffisante à grande échelle ?
5. Quels mécanismes conviennent à la configuration et lesquels au monitoring ?
6. Pourquoi plusieurs protocoles de gestion peuvent-ils coexister ?
7. Quel est le rôle de Telemetry et de Syslog ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 937 à 942.
