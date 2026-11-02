---
id: HUA-HCIA-DC-060
title: "Intelligent Computing DCN"
official_title: "Intelligent Computing DCN"
course_unit: "5.1.4"
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
source_pages: "PDF 767-776"
tags: [huawei, hcia, datacom, ai, gpu, lossless-network, pfc, ecn, rdma, roce]
prerequisites:
  - "[[General-Purpose Computing DCN]]"
related:
  - "[[Dual-Track Architecture of DCs]]"
lab_required: false
publication_status: "not-started"
site_url:
---

# DCN de calcul intelligent (Intelligent Computing DCN)

## Calcul parallèle et synchronisation

Le calcul intelligent utilise principalement des GPU pour diviser un très grand travail en nombreuses opérations simples exécutées en parallèle. Les exemples sont l'entraînement et l'inférence IA ainsi que le calcul scientifique.

À la fin d'une itération d'entraînement, les GPU synchronisent leurs données selon un motif **all-to-all**. Un GPU ayant terminé attend les autres à une barrière de synchronisation. La latence réseau devient donc directement du temps de calcul inutilisé.

## Risques du trafic IA

Des centaines ou milliers de serveurs produisent des rafales et des incast many-to-one. Congestion, déséquilibre et perte prolongent certains flux ; l'ensemble du cluster attend alors les retardataires.

![Les exigences réseau du calcul intelligent et les mécanismes associés](<../../../assets/diagrammes/05 Data Center Network Basics/intelligent-dcn-requirements-fr.svg>)

| Exigence | Ordre de grandeur ou mécanisme du support |
| --- | --- |
| Très haute bande passante | 400 ou 800 Gbit/s par port selon les scénarios. |
| Absence de perte | PFC et ECN évitent la saturation et les retransmissions. |
| Très faible latence | RDMA contourne le noyau et réduit l'usage CPU. |
| Grande échelle | Spine-leaf ou dragonfly selon la taille du cluster. |

## PFC : contrôle lien par lien

**Priority-based Flow Control** surveille le tampon d'une file de priorité. Au-dessus d'un seuil, le récepteur envoie une trame PFC pour interrompre temporairement l'émetteur. Quand le tampon descend sous le seuil de reprise, une nouvelle trame autorise la transmission.

PFC agit entre deux voisins et par priorité. Il évite que l'émetteur ne déborde le tampon du récepteur.

## ECN : notification de bout en bout

**Explicit Congestion Notification** marque les paquets lorsque le tampon dépasse un seuil. Le serveur recevant ces paquets envoie des Congestion Notification Packets à la source, qui réduit son débit.

PFC freine localement ; ECN signale la congestion jusqu'à l'émetteur. Ils sont complémentaires.

## RDMA et RoCE

**Remote Direct Memory Access** permet à une carte réseau de lire ou écrire directement la mémoire distante sans copies répétées via la mémoire du noyau. Le CPU et la pile classique sont moins sollicités.

**RoCE** transporte RDMA sur Ethernet ; RoCEv2 est la variante la plus utilisée dans le support. La carte réseau doit le prendre en charge.

## Pratique TianSemi : analyser une congestion

Un cluster entraîne un modèle sur huit GPU. Tous envoient simultanément vers un nœud d'agrégation :

1. identifiez le motif incast ;
2. expliquez l'effet d'une perte sur la barrière de synchronisation ;
3. placez l'action de PFC sur le lien congestionné ;
4. décrivez la boucle de rétroaction ECN/CNP ;
5. expliquez ce que RDMA réduit côté serveur.

## À retenir pour l'examen HCIA-Datacom

- Le calcul intelligent privilégie le débit de tâches massivement parallèles.
- Les échanges GPU sont all-to-all et synchronisés.
- La perte d'un paquet peut ralentir toute une itération.
- PFC contrôle localement une file par priorité.
- ECN marque la congestion et provoque un ralentissement de bout en bout.
- RDMA réduit copies mémoire, intervention du noyau et latence.
- RoCE permet RDMA sur Ethernet.

## Révision TianSemi

1. Pourquoi la latence réseau réduit-elle l'utilisation des GPU ?
2. Qu'est-ce qu'un motif all-to-all ?
3. Pourquoi l'incast peut-il saturer un port ?
4. Quand un équipement envoie-t-il une trame PFC ?
5. Comment ECN fait-il réduire le débit de la source ?
6. Quelle différence de portée existe entre PFC et ECN ?
7. Comment RDMA réduit-il la latence serveur ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 767 à 776.
