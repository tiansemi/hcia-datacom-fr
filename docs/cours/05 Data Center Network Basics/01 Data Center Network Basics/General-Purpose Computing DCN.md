---
id: HUA-HCIA-DC-059
title: "General-Purpose Computing DCN"
official_title: "General-Purpose Computing DCN"
course_unit: "5.1.3"
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
source_pages: "PDF 753-766"
tags: [huawei, hcia, datacom, dcn, spine-leaf, m-lag, vxlan, evpn, sdn]
prerequisites:
  - "[[DC Development and Evolution]]"
related:
  - "[[Intelligent Computing DCN]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# DCN de calcul général (General-Purpose Computing DCN)

## Profil des charges

Le calcul général s'appuie principalement sur les CPU pour exécuter de nombreuses tâches variées et à logique complexe : serveurs web, bases de données et applications d'entreprise.

Une requête client déclenche souvent des dizaines d'appels entre services. Chaque échange est petit, mais leur agrégation produit un trafic **east-west** intense et sensible à la latence.

## De DCN 1.0 à DCN 3.0

![Évolution du DCN général vers spine-leaf, M-LAG et VXLAN](<../../../assets/diagrammes/05 Data Center Network Basics/general-dcn-evolution-fr.svg>)

| Version | Technologies | Limitation ou progrès |
| --- | --- | --- |
| DCN 1.0 | STP + VRRP | Liens bloqués, faible utilisation et échelle limitée. |
| DCN 2.0 | M-LAG | Agrégation multi-châssis et liens actifs, mais chemins L3 east-west non optimaux. |
| DCN 3.0 | Spine-leaf + VXLAN | Multipath, échelle accrue et chemins east-west optimisés. |

Huawei présente DCN 3.0 comme l'architecture actuelle du calcul général.

## Exigences et technologies

### Élasticité : spine-leaf

Chaque leaf se connecte à chaque spine, sans liaison east-west au même niveau. Ajouter des spines augmente la capacité ; une troisième couche permet une échelle supérieure.

### Bande passante et disponibilité : M-LAG

M-LAG agrège les liens d'un serveur ou d'un pare-feu vers deux leaf distincts. Il augmente la bande passante utilisable et étend la redondance du niveau carte au niveau équipement.

### Isolation et mobilité : VXLAN

Le VNI VXLAN utilise 24 bits, contre 12 bits pour le VLAN. Il permet environ 16 millions d'identifiants et construit un réseau L2 virtuel au-dessus d'un underlay IP routé, étendant le domaine de migration des VM.

### Automatisation : EVPN et SDN

EVPN fournit un plan de contrôle à VXLAN pour découvrir les VTEP et annoncer les hôtes sans dépendre uniquement du flooding. Un contrôleur SDN centralise la gestion et collabore avec la plateforme cloud pour automatiser le déploiement.

## Underlay et overlay

- l'**underlay** est le réseau IP routé qui assure la connectivité entre VTEP ;
- l'**overlay** contient les réseaux VXLAN des utilisateurs ;
- les utilisateurs voient leur réseau virtuel, pas la topologie physique sous-jacente.

## Pratique TianSemi : concevoir une fabric

Pour six racks de serveurs :

1. placez deux leaf par groupe de serveurs et au moins deux spines ;
2. dessinez les connexions full-mesh leaf-spine ;
3. ajoutez M-LAG pour un serveur critique ;
4. attribuez deux VNI à deux tenants ;
5. identifiez underlay, overlay, VTEP et plan de contrôle EVPN.

## À retenir pour l'examen HCIA-Datacom

- Le calcul général privilégie flexibilité, concurrence et logique complexe.
- Ses flux sont principalement east-west et sensibles à la latence.
- Spine-leaf apporte élasticité et multipath.
- M-LAG apporte bande passante et redondance multi-équipement.
- VXLAN utilise un VNI 24 bits pour l'isolation et la mobilité.
- EVPN automatise le plan de contrôle VXLAN ; SDN automatise le réseau.

## Révision TianSemi

1. Pourquoi une requête web produit-elle beaucoup de trafic east-west ?
2. Quelle limitation de STP DCN 2.0 corrige-t-il ?
3. Comment spine-leaf permet-il l'extension de capacité ?
4. Quel problème M-LAG résout-il au raccordement des serveurs ?
5. Pourquoi VXLAN dépasse-t-il l'échelle des VLAN ?
6. Quelle différence existe entre underlay et overlay ?
7. Quel rôle EVPN et le contrôleur SDN jouent-ils ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 753 à 766.
