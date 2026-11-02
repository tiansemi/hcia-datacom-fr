---
id: HUA-HCIA-DC-073
title: "SNMP"
official_title: "SNMP"
course_unit: "7.1.2"
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
source_pages: "PDF 943-954"
tags: [huawei, hcia, datacom, snmp, nms, mib, oid, trap, inform, snmpv3]
prerequisites:
  - "[[Basic Concepts of Network Management]]"
related:
  - "[[NETCONF]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# SNMP

## Une gestion centralisée fondée sur des objets

**Simple Network Management Protocol** permet à un NMS de superviser et, dans certaines limites, modifier des équipements de types ou constructeurs différents. Le NMS exécute un processus de gestion ; chaque équipement exécute un agent.

![Architecture SNMP, MIB et échanges entre NMS et agent](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/snmp-mib-exchanges-fr.svg>)

L’agent maintient les informations de l’équipement, répond aux requêtes du NMS et lui signale certains événements. Les informations manipulées sont des **objets gérés** organisés dans une **MIB**.

## MIB et OID

La **Management Information Base** est une structure arborescente décrivant les variables gérables. Pour chaque objet, elle précise notamment :

- l’Object Identifier, ou **OID** ;
- son statut ;
- ses droits d’accès ;
- son type de données.

Un OID est le chemin numérique d’un objet dans l’arbre. Les MIB publiques normalisent les objets communs ; les MIB privées complètent ces objets pour des fonctions propres à un constructeur. Huawei utilise la branche entreprise `1.3.6.1.4.1.2011`.

## Opérations SNMP

| Opération | Origine | Fonction |
| --- | --- | --- |
| Get | NMS | Lit une ou plusieurs valeurs. |
| Get-Next | NMS | Lit l’objet suivant dans l’ordre lexicographique. |
| Set | NMS | Modifie une ou plusieurs valeurs autorisées. |
| Response | Agent | Répond à Get, Get-Next ou Set. |
| Trap | Agent | Signale spontanément un événement, sans acquittement applicatif. |
| Get-Bulk | NMS | Regroupe plusieurs lectures successives, à partir de SNMPv2c. |
| Inform | Agent | Notifie le NMS et exige un acquittement, à partir de SNMPv2c. |

`Inform` est plus fiable que `Trap` : sans réponse, l’agent conserve l’événement dans un tampon et retransmet jusqu’à acquittement ou jusqu’à la limite prévue.

## Versions et sécurité

| Version | Apport | Limite ou usage |
| --- | --- | --- |
| SNMPv1 | Modèle initial et cinq opérations de base | Community string, faible sécurité et codes d’erreur limités. |
| SNMPv2c | Get-Bulk, Inform, Counter32/64 et erreurs enrichies | Community string, protection insuffisante. |
| SNMPv3 | USM, authentification, chiffrement et VACM | Recommandé pour les réseaux nécessitant une gestion sûre. |

Le **User-based Security Model** vérifie l’identité et l’intégrité puis peut chiffrer les messages. Le **View-based Access Control Model** limite les objets accessibles à chaque groupe.

## Configuration SNMPv3 Huawei

Le support propose la séquence suivante. Les algorithmes et vues doivent être adaptés à la politique locale.

```text
[HUAWEI] snmp-agent
[HUAWEI] snmp-agent sys-info version v3
[HUAWEI] snmp-agent protocol source-interface Vlanif 10
[HUAWEI] snmp-agent mib-view included iso iso
[HUAWEI] snmp-agent group v3 admin privacy read-view iso write-view iso notify-view iso
[HUAWEI] snmp-agent usm-user v3 nms-admin group admin
[HUAWEI] snmp-agent usm-user v3 nms-admin authentication-mode sha2-256
[HUAWEI] snmp-agent usm-user v3 nms-admin privacy-mode aes128
[HUAWEI] snmp-agent target-host trap address udp-domain 192.168.1.1 params securityname nms-admin v3 privacy
[HUAWEI] snmp-agent trap enable
```

Les commandes d’authentification et de confidentialité demandent les secrets de manière interactive sur la version utilisée dans le Training Material. Ne placez pas ces secrets dans une note, un script ou un dépôt Git.

Le `source-interface` limite l’adresse locale utilisée pour recevoir et répondre au NMS. Le `target-host` désigne la destination des notifications ; `trap enable` active leur émission. Les deux sont nécessaires pour recevoir effectivement les traps.

## Pratique TianSemi : validation d’une supervision

1. Créez une vue de lecture limitée aux objets nécessaires plutôt que d’exposer tout l’arbre.
2. Configurez un groupe SNMPv3 en mode `privacy`.
3. Ajoutez un utilisateur avec authentification et chiffrement.
4. Fixez l’interface source de gestion.
5. Déclarez le NMS comme destination des traps.
6. Depuis le NMS, lisez un OID autorisé puis tentez un OID exclu.
7. Déclenchez un événement de lab et confirmez sa réception.

## Troubleshooting

| Symptôme | Vérification |
| --- | --- |
| Aucune réponse aux requêtes | Route, ACL, interface source, version et paramètres USM. |
| Authentification échouée | Utilisateur, protocole, clé et synchronisation des paramètres NMS/agent. |
| Lecture partielle | MIB chargée sur le NMS et vue VACM appliquée au groupe. |
| Requêtes OK, aucun trap | `target-host`, `trap enable`, joignabilité du NMS et vue de notification. |
| OID affiché sans nom | MIB correspondante absente de la plateforme. |

## À retenir pour l’examen HCIA-Datacom

- NMS et agent échangent des messages décrivant des objets de MIB identifiés par OID.
- Get, Get-Next, Set, Response et Trap existent dès SNMPv1.
- SNMPv2c ajoute notamment Get-Bulk et Inform.
- Inform exige un acquittement ; Trap n’en exige pas.
- SNMPv3 apporte USM, authentification, chiffrement et contrôle par vues.
- SNMPv3 est le choix recommandé lorsque la sécurité compte.

## Révision TianSemi

1. Quels rôles remplissent le NMS et l’agent SNMP ?
2. Qu’est-ce qu’une MIB et comment un OID identifie-t-il un objet ?
3. Quelle différence existe entre Get et Get-Next ?
4. Pourquoi Get-Bulk est-il utile ?
5. Quelle différence existe entre Trap et Inform ?
6. Qu’apportent USM et VACM dans SNMPv3 ?
7. Pourquoi faut-il configurer à la fois `target-host` et `trap enable` ?
8. Pourquoi éviter SNMPv1 et v2c sur un réseau non protégé ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 943 à 954.
