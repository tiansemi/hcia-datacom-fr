---
id: HUA-HCIA-DC-074
title: "NETCONF"
official_title: "NETCONF"
course_unit: "7.1.3"
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
source_pages: "PDF 955-965"
tags: [huawei, hcia, datacom, netconf, ssh, rpc, xml, yang, datastore, call-home]
prerequisites:
  - "[[SNMP]]"
related:
  - "[[RESTCONF]]"
  - "[[Introduction to iMaster NCE]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# NETCONF

## Pourquoi NETCONF complète SNMP

SNMP est largement utilisé pour la supervision, mais il manque de mécanismes adaptés aux changements de configuration complexes : verrouillage lors d’accès concurrents, lecture structurée d’une configuration entière et transaction contrôlée. **Network Configuration Protocol** répond à ces besoins.

![Couches, session et datastores NETCONF](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/netconf-layers-datastores-fr.svg>)

NETCONF permet d’ajouter, modifier ou supprimer des configurations et de lire les données de configuration ou d’état.

## Architecture client–serveur

- le client NETCONF s’exécute sur la plateforme de gestion ;
- le serveur NETCONF s’exécute sur l’équipement ;
- les échanges sont des requêtes et réponses RPC structurées.

Une session suit : établissement SSH, échange des capacités dans les messages `hello`, requêtes et réponses, demande de fermeture, puis confirmation.

## Les quatre couches

| Couche | Rôle | Exemples |
| --- | --- | --- |
| Secure Transport | Protège et transporte la session | SSH sur les équipements Huawei présentés. |
| Messages | Encapsule la mécanique RPC | `<rpc>`, `<rpc-reply>`, `<notification>`. |
| Operations | Définit les actions standard | `<get>`, `<edit-config>`, `<lock>`. |
| Content | Décrit les données configurées et observées | XML, schémas et modèles YANG. |

YANG définit des structures hiérarchiques et les données de configuration, d’état, les RPC et notifications. Le modèle sépare la structure des données du transport et de l’opération.

## Datastores

| Datastore | Contenu | Effet des modifications |
| --- | --- | --- |
| `<running/>` | Configuration actuellement effective | Modification individuelle immédiatement active. Seul datastore standard obligatoire. |
| `<candidate/>` | Configuration préparée | Doit être commitée vers running pour prendre effet. |
| `<startup/>` | Configuration chargée au prochain démarrage | Remplace running au démarrage ; remplacement global via copy-config. |

Cette séparation permet de préparer puis valider un ensemble de changements avant application, lorsque les capacités du serveur le permettent.

## Opérations essentielles

| Opération | Fonction |
| --- | --- |
| `<get-config>` | Lit une partie ou la totalité d’un datastore de configuration. |
| `<get>` | Lit configuration effective et données d’état. |
| `<edit-config>` | Ajoute, modifie ou supprime dans running ou candidate. |
| `<copy-config>` | Remplace un datastore par un autre. |
| `<delete-config>` | Supprime un datastore autre que running. |
| `<lock>` / `<unlock>` | Réserve puis libère l’accès exclusif à un datastore. |
| `<close-session>` | Ferme proprement la session. |
| `<kill-session>` | Ferme de force une session, avec privilège administrateur. |

Les capacités annoncées dans `hello` indiquent notamment si l’équipement accepte writable-running, candidate, rollback-on-error, startup distinct, notifications ou interleave.

## Activer NETCONF sur Huawei

```text
[HUAWEI] snetconf server enable
[HUAWEI] protocol inbound ssh port 830
```

Le support indique que les deux commandes activent NETCONF : la première permet l’usage sur le port SSH 22, la seconde sur le port standard 830. Si les deux sont présentes, les deux ports sont disponibles.

Pour que l’équipement initie lui-même la connexion vers un NMS :

```text
[HUAWEI] netconf
[HUAWEI-netconf] callhome <nom-callhome>
[HUAWEI-Netconf-callhome-<nom>] endpoint <nom-endpoint>
[HUAWEI-Netconf-callhome-<nom>-endpoint-<endpoint>] peer-ip <ip-nms> port <port>
```

Le **call home** convient lorsque le NMS ne découvre pas automatiquement l’équipement. Une autre approche utilise DHCP Option 148 : un équipement à l’état usine reçoit son adresse, les paramètres d’activation NETCONF et l’adresse/port d’iMaster NCE-Campus, puis s’enregistre auprès du contrôleur.

## Pratique TianSemi : transaction contrôlée

Sur un équipement ou simulateur compatible :

1. activez NETCONF sur SSH ;
2. établissez la session depuis un client ;
3. relevez les capacités annoncées dans `hello` ;
4. lisez `<running/>` avec un filtre ;
5. si `<candidate/>` est supporté, verrouillez-le, appliquez une modification, contrôlez le diff, commitez puis déverrouillez ;
6. refermez proprement la session ;
7. expliquez ce qui aurait été différent avec une modification immédiate de running.

## Troubleshooting

| Symptôme | Vérification |
| --- | --- |
| Connexion refusée | Service NETCONF, SSH, port 22/830, AAA et interface source. |
| RPC non reconnu | Capacité absente du `hello` ou espace de noms incorrect. |
| Échec de modification | Droits, datastore cible, verrou existant ou modèle YANG. |
| Configuration candidate sans effet | Opération de commit non réalisée. |
| Call home absent | Adresse/port NMS, route, ACL et paramètres de l’endpoint. |

## À retenir pour l’examen HCIA-Datacom

- NETCONF apporte données structurées, verrouillage, filtrage et meilleure extensibilité que SNMP pour la configuration.
- Huawei utilise SSH comme transport dans le contexte du cours.
- La session échange d’abord les capacités avec `hello`.
- Running est obligatoire ; candidate et startup sont optionnels selon les capacités.
- `<edit-config>` modifie, `<get>` lit état et configuration, `<lock>` évite les conflits.
- YANG modélise les données utilisées par NETCONF et RESTCONF.
- DHCP Option 148 peut fournir les paramètres d’enregistrement à iMaster NCE-Campus.

## Révision TianSemi

1. Quelles limites de SNMP motivent l’usage de NETCONF ?
2. Quels rôles remplissent client et serveur NETCONF ?
3. Quelles sont les quatre couches conceptuelles de NETCONF ?
4. Que se passe-t-il lors de l’échange `hello` ?
5. Quelle différence existe entre running, candidate et startup ?
6. À quoi servent lock et unlock ?
7. Quel est le rôle de YANG ?
8. Comment call home et DHCP Option 148 facilitent-ils l’onboarding ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 955 à 965.
