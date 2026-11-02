---
id: HUA-HCIA-DC-075
title: "RESTCONF"
official_title: "RESTCONF"
course_unit: "7.1.4"
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
source_pages: "PDF 967-971"
tags: [huawei, hcia, datacom, restconf, http, restful, api, json, xml, yang]
prerequisites:
  - "[[NETCONF]]"
related:
  - "[[Introduction to iMaster NCE]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# RESTCONF

## Les données réseau exposées comme ressources web

**Representational State Transfer Configuration Protocol** reprend les concepts de configuration structurée de NETCONF et les expose au moyen de méthodes HTTP et d’interfaces RESTful courantes dans le monde applicatif.

![Correspondance entre méthodes RESTCONF et opérations NETCONF](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/restconf-http-methods-fr.svg>)

Le client est généralement une plateforme ou une application web. Le serveur RESTCONF s’exécute sur l’équipement, interprète la requête, agit sur les données puis répond en XML ou JSON.

## NETCONF et RESTCONF

| Aspect | NETCONF | RESTCONF |
| --- | --- | --- |
| Transport privilégié | SSH | HTTP/HTTPS |
| Format du support | XML | XML ou JSON |
| Style | RPC | Ressources et méthodes RESTful |
| Datastores | Plusieurs, selon capacités | Uniquement running dans le cours |
| Application | Peut préparer puis commiter | Effet immédiat |
| Fonctions avancées | Verrouillage, rollback, transaction | Opérations CRUD plus simples |

RESTCONF simplifie l’intégration applicative, mais ne remplace pas les mécanismes transactionnels avancés de NETCONF.

## Méthodes HTTP

| Méthode | Fonction RESTCONF | Équivalent conceptuel NETCONF |
| --- | --- | --- |
| OPTIONS | Interroge les méthodes prises en charge | Aucun équivalent direct. |
| HEAD | Vérifie l’existence d’une donnée | `<get-config>` ou `<get>` sans corps retourné. |
| GET | Lit configuration ou état | `<get-config>` ou `<get>`. |
| POST | Crée une donnée ou invoque un RPC | `<edit-config operation="create">` ou RPC. |
| PUT | Remplace une configuration | `<edit-config operation="replace">`. |
| PATCH | Fusionne une modification | `<edit-config operation="merge">`. |
| DELETE | Supprime une donnée | `<edit-config operation="delete">`. |

Les changements agissent directement sur running. Le client doit donc vérifier précisément la ressource et le corps envoyé avant une opération d’écriture.

## Activation sur Huawei

Le Training Material illustre l’activation HTTP :

```text
[HUAWEI] http
[HUAWEI-http] service restconf
[HUAWEI-http-service-restconf] server enable
[HUAWEI-http-service-restconf] server-source Vlanif20
```

La connectivité et l’utilisateur AAA doivent être préparés séparément. Le support renvoie à la documentation produit pour HTTPS. Sur un réseau réel, privilégiez le transport sécurisé et une interface source de gestion contrôlée.

## Pratique TianSemi : tester sans modifier

1. Activez RESTCONF dans un environnement de lab et limitez sa source à l’interface de gestion.
2. Configurez l’utilisateur AAA nécessaire.
3. Depuis un client HTTP, utilisez `OPTIONS` pour relever les méthodes autorisées.
4. Testez `HEAD` sur une ressource connue puis inexistante.
5. Utilisez `GET` et comparez la réponse JSON et XML si les deux sont disponibles.
6. Préparez un `PATCH`, mais relisez URI, modèle YANG et corps avant de l’exécuter.
7. Vérifiez l’état sur l’équipement après toute écriture.

## Troubleshooting

| Symptôme | Vérification |
| --- | --- |
| Session impossible | Service, source HTTP, route, ACL et AAA. |
| `404` | URI de ressource ou chemin du modèle incorrect. |
| `405` | Méthode non permise sur la ressource. |
| `401/403` | Authentification ou autorisation insuffisante. |
| Corps rejeté | Format JSON/XML, espace de noms, types et contraintes YANG. |
| Changement immédiat inattendu | RESTCONF agit sur running sans phase candidate/commit. |

## À retenir pour l’examen HCIA-Datacom

- RESTCONF combine concepts NETCONF, modèles structurés et HTTP RESTful.
- Les messages utilisent XML ou JSON.
- GET lit, POST crée ou invoque, PUT remplace, PATCH fusionne et DELETE supprime.
- RESTCONF utilise running et les modifications prennent effet immédiatement.
- NETCONF reste plus riche pour datastores multiples, verrouillage et rollback.
- L’interface source et un transport sécurisé sont essentiels en exploitation.

## Révision TianSemi

1. Pourquoi RESTCONF est-il familier aux développeurs d’applications web ?
2. Quelles différences existent entre RESTCONF et NETCONF pour le transport et l’encodage ?
3. Que font GET, POST, PUT, PATCH et DELETE ?
4. Pourquoi une modification RESTCONF exige-t-elle une validation prudente ?
5. Quel datastore RESTCONF utilise-t-il dans le cours ?
6. Quel rôle remplit `server-source` ?
7. Dans quel cas NETCONF reste-t-il préférable à RESTCONF ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 967 à 971.
