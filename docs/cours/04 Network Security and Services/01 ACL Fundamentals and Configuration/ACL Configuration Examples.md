---
id: HUA-HCIA-DC-045
title: "ACL Configuration Examples"
official_title: "ACL Configuration Examples"
course_unit: "4.1.3"
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
source_pages: "PDF 583-589"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 136-142"
tags: [huawei, hcia, datacom, acl-configuration, advanced-acl, mqc, traffic-policy]
prerequisites:
  - "[[Fundamentals of ACL]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Exemples de configuration ACL (ACL Configuration Examples)

## Basic ACL

Une basic ACL convient lorsque seule la source IPv4 doit déterminer la correspondance.

```text
[R1] acl 2000
[R1-acl4-basic-2000] rule 5 deny source 192.168.1.0 0.0.0.255
[R1-acl4-basic-2000] rule 10 permit source any
```

## Advanced ACL

Une advanced ACL peut préciser source, destination, protocole et ports.

```text
[R1] acl 3000
[R1-acl4-advance-3000] rule 5 deny ip source 192.168.1.0 0.0.0.255 destination 192.168.254.0 0.0.0.255
[R1-acl4-advance-3000] rule 10 permit ip source 192.168.2.0 0.0.0.255 destination 192.168.254.0 0.0.0.255
```

Dans le scénario Huawei, R&D est bloqué vers le serveur intranet alors que le marketing est autorisé.

## Invoquer l'ACL avec MQC

```text
[R1] traffic classifier FILTER-SERVER
[R1-classifier-FILTER-SERVER] if-match acl 3000
[R1] traffic behavior APPLY-FILTER
[R1-behavior-APPLY-FILTER] permit
[R1] traffic policy SERVER-POLICY
[R1-trafficpolicy-SERVER-POLICY] classifier FILTER-SERVER behavior APPLY-FILTER
[R1] interface GE 0/0/1
[R1-GE0/0/1] traffic-policy SERVER-POLICY outbound
```

Le classifier sélectionne les paquets ayant obtenu une correspondance ACL. Le behavior et le module MQC interprètent ensuite permit/deny selon la logique décrite par la plateforme. Il faut donc tester le comportement effectif, pas déduire le résultat à partir du seul mot `permit` du behavior.

## Placement de la politique

Pour filtrer un flux R1 → R3 sur R2, deux emplacements peuvent voir le paquet :

- entrée de l'interface recevant depuis R1 ;
- sortie de l'interface envoyant vers R3.

Le lab applique la politique en sortie de GE0/0/2. Le meilleur emplacement limite l'impact et évite de traiter du trafic non concerné.

## Mini-lab TianSemi inspiré du Lab Guide

1. Configurez OSPF pour joindre une loopback source à une adresse distante.
2. Vérifiez deux pings sourcés distincts avant filtrage.
3. Créez une ACL qui correspond à une seule des deux sources.
4. Liez-la à classifier, behavior et policy.
5. Appliquez la policy en sortie vers la destination.
6. Vérifiez que le flux ciblé change de comportement et que l'autre reste intact.
7. Déplacez la policy en entrée du lien source et comparez.

## Vérifications

```text
<R1> display acl 3000
<R1> display traffic classifier user-defined
<R1> display traffic policy user-defined
<R1> display traffic-policy applied-record
```

Selon la version VRP, les commandes d'affichage disponibles peuvent varier. Complétez avec des pings sourcés et les compteurs de règles/policy.

## Troubleshooting

| Symptôme | Contrôle |
| --- | --- |
| Tous les flux passent | ACL non invoquée, policy non appliquée ou mauvaise direction |
| Tous les flux sont bloqués | Règle trop générale, ordre ou comportement par défaut |
| Exception ignorée | Règle générale de Rule ID plus faible |
| Aucun compteur | Trafic ne traverse pas l'interface/direction choisie |
| Ping de test trompeur | Adresse source non imposée |
| Syntaxe refusée | Type ACL ou capacités de la version VRP |

## À retenir pour l'examen HCIA-Datacom

- Basic ACL : 2000–2999 ; advanced ACL : 3000–3999.
- Une advanced ACL peut combiner source, destination, protocole et port.
- MQC suit classifier → behavior → policy → interface.
- Une traffic policy s'applique en `inbound` ou `outbound`.
- Toujours vérifier l'ordre des règles et le module qui invoque l'ACL.
- Les tests doivent inclure au moins un flux autorisé et un flux interdit.

## Révision TianSemi

1. Quelle différence de critères sépare basic et advanced ACL ?
2. Quels objets faut-il créer avant d'appliquer une policy MQC ?
3. Où appliquer une policy pour filtrer le trafic sortant vers un serveur ?
4. Pourquoi faut-il imposer la source d'un ping de validation ?
5. Quelles causes expliquent une ACL sans aucun compteur ?
6. Comment vérifier qu'une exception n'est pas masquée par une règle générale ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 583 à 589.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 136 à 142 pour le filtrage ACL invoqué par MQC.
