---
id: HUA-HCIA-DC-044
title: "Fundamentals of ACL"
official_title: "Fundamentals of ACL"
course_unit: "4.1.2"
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
source_pages: "PDF 569-582"
tags: [huawei, hcia, datacom, acl-rule, wildcard, basic-acl, advanced-acl, mqc]
prerequisites:
  - "[[ACL Overview]]"
related:
  - "[[ACL Configuration Examples]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# Fondamentaux des ACL (Fundamentals of ACL)

## Anatomie d'une ACL

```text
acl number 2000
 rule 5 permit source 10.1.1.0 0.0.0.255
 rule 10 deny source 10.2.2.0 0.0.0.255
```

- le numéro identifie l'ACL et son type ;
- le Rule ID ordonne les règles ;
- `permit` ou `deny` donne le résultat de la correspondance ;
- les options décrivent le trafic à reconnaître.

Le pas automatique par défaut est 5 : 5, 10, 15, etc. Cet espace permet d'insérer ultérieurement une règle intermédiaire, par exemple 11, sans renuméroter immédiatement toutes les règles.

## Wildcard

![Lire une wildcard : 0 exige une égalité, 1 ignore le bit](<../../../assets/diagrammes/04 Network Security and Services/acl-wildcard-fr.svg>)

Une wildcard indique, bit par bit :

- `0` : le bit doit correspondre exactement ;
- `1` : le bit n'est pas contrôlé.

| Besoin | Adresse + wildcard |
| --- | --- |
| Hôte exact `192.168.1.1` | `192.168.1.1 0` |
| Sous-réseau `192.168.1.0/24` | `192.168.1.0 0.0.0.255` |
| Toutes les adresses | `any` ou `0.0.0.0 255.255.255.255` |
| Adresses impaires du /24 | `192.168.1.1 0.0.0.254` |

Contrairement à un masque de sous-réseau, les bits d'une wildcard n'ont pas besoin d'être contigus.

## Principaux types

| Type | Numéros | Critères majeurs |
| --- | ---: | --- |
| Basic ACL | 2000–2999 | Source IPv4, fragment, plage horaire |
| Advanced ACL | 3000–3999 | Source/destination, protocole, ports TCP/UDP |
| Layer 2 ACL | 4000–4999 | MAC, VLAN, type de protocole L2 |
| User-defined ACL | 5000–5999 | Octets à un offset défini |
| User ACL | 6000–9999 | Adresses, groupes UCL, protocoles et ports |
| ARP-based ACL | 23000–23999 | Adresses IPv4 et MAC des paquets ARP |

Une ACL peut être numérotée ou nommée. Pour un simple critère de source, une basic ACL suffit ; dès que destination, protocole ou port est nécessaire, utilisez une advanced ACL.

## Ordre et résultat de correspondance

![Première règle correspondante : le traitement ACL s'arrête](<../../../assets/diagrammes/04 Network Security and Services/acl-first-match-fr.svg>)

En ordre `config`, utilisé par défaut, les règles sont examinées par Rule ID croissant. En ordre `auto`, le système les organise par précision décroissante.

Résultats :

- règle `permit` trouvée : correspondance positive permit ;
- règle `deny` trouvée : correspondance positive deny ;
- aucune règle trouvée, ACL vide ou inexistante : correspondance négative.

Le traitement final dépend ensuite du module appelant. Il faut connaître son comportement lorsque l'ACL ne correspond à aucune règle.

## MQC

MQC associe trois objets :

1. **traffic classifier** : sélectionne les paquets, souvent via une ACL ;
2. **traffic behavior** : définit l'action ;
3. **traffic policy** : lie classifier et behavior, puis s'applique à une interface.

Le filtre peut être placé en entrée ou en sortie. Choisissez l'endroit qui voit le trafic voulu sans perturber inutilement d'autres flux.

## Pratique TianSemi : prédire les correspondances

Pour les règles 5 deny `192.168.1.0/24`, 10 permit `192.168.1.10` et 15 permit any :

1. prédisez le résultat pour `192.168.1.10` ;
2. expliquez pourquoi la règle 10 ne sera jamais atteinte ;
3. proposez un nouvel ordre qui autorise l'exception ;
4. testez aussi une adresse extérieure au /24.

## À retenir pour l'examen HCIA-Datacom

- Les règles sont ordonnées par Rule ID ; le pas par défaut est 5.
- Wildcard 0 = exact, 1 = indifférent.
- Basic ACL examine surtout la source ; advanced ACL ajoute destination, protocole et ports.
- La première règle correspondante arrête la recherche.
- `config` suit l'ID croissant ; `auto` privilégie la précision.
- ACL, classifier, behavior et policy ont des rôles séparés.

## Révision TianSemi

1. Quel est le rôle du Rule ID et du pas ?
2. Comment interpréter les bits 0 et 1 d'une wildcard ?
3. Quelle wildcard correspond à un /24, un hôte exact et toutes les adresses ?
4. Quand faut-il préférer une advanced ACL à une basic ACL ?
5. Que signifie une correspondance positive deny ?
6. Pourquoi une règle générale placée trop tôt masque-t-elle les suivantes ?
7. Quels sont les trois objets MQC ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 569 à 582.
