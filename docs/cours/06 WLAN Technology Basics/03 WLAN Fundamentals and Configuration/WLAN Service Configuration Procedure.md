---
id: HUA-HCIA-DC-070
title: "WLAN Service Configuration Procedure"
official_title: "WLAN Service Configuration Procedure"
course_unit: "6.3.2"
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
source: "HCIA-Datacom V2.0 Training Material; HCIA-Datacom V2.0 Lab Guide"
source_pages: "Training PDF 905-917; Lab Guide PDF 172-186 (document pages 170-184)"
tags: [huawei, hcia, datacom, wlan, configuration, profile, ap-group, ssid-profile, vap-profile]
prerequisites:
  - "[[WLAN Fundamentals]]"
related:
  - "[[WLAN Service Configuration Examples]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Procédure de configuration d’un service WLAN (WLAN Service Configuration Procedure)

## Logique générale

La configuration Huawei repose sur des **profils** liés entre eux, puis appliqués à un AP ou, plus souvent, à un groupe d’AP. Il faut donc construire le service du bas vers le haut : connectivité, admission des AP, profils, liaisons, puis vérification.

![Relations entre profils et ordre de configuration WLAN](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-profile-workflow-fr.svg>)

## Étape 1 — Préparer la connectivité

Avant d’entrer dans la vue WLAN, vérifiez :

- le VLAN ou le routage du trafic de gestion entre WAC et AP ;
- le chemin du trafic de service en transfert direct ;
- la joignabilité du serveur DHCP par les stations ;
- le plan d’adresses des AP, des stations et de la source CAPWAP.

En transfert tunnel, le trafic de service est encapsulé vers le WAC : le réseau d’accès n’a pas à transporter le VLAN de service de la même manière qu’en transfert direct.

## Étape 2 — Créer le groupe et permettre la mise en ligne des AP

Un AP ne peut appartenir qu’à un groupe lorsqu’il est en ligne. Le groupe permet de livrer une configuration identique à plusieurs AP.

```text
<WAC> system-view
[WAC] wlan
[WAC-wlan] ap-group name <groupe-ap>
[WAC-wlan-ap-group-<groupe-ap>] quit
```

Configurez ensuite la source CAPWAP du WAC, sous forme d’interface ou d’adresse :

```text
[WAC] capwap source interface Vlanif <vlan-gestion>
# ou
[WAC] capwap source ip-address <adresse-wac>
```

La source doit être unique et joignable par les AP. Selon le scénario de déploiement, le WAC peut autoriser l’établissement initial de sessions DTLS sans authentification :

```text
[WAC] capwap dtls no-auth enable
```

Cette facilité de déploiement doit être limitée au contexte prévu et accompagnée d’un mode d’admission explicite.

## Étape 3 — Choisir le contrôle d’accès des AP

```text
[WAC-wlan] ap auth-mode { mac-auth | sn-auth | no-auth }
```

Avec une admission MAC ou SN, ajoutez l’AP hors ligne, donnez-lui un nom et rattachez-le au groupe :

```text
[WAC-wlan] ap-id <id> ap-mac <mac-ap>
[WAC-wlan-ap-<id>] ap-name <nom-ap>
[WAC-wlan-ap-<id>] ap-group <groupe-ap>
[WAC-wlan-ap-<id>] quit
```

Vérifiez la mise en ligne avant de construire le service :

```text
<WAC> display ap all
<WAC> display ap ap-group <groupe-ap>
```

## Étape 4 — Créer les profils de base

### Profil de domaine réglementaire

Il définit notamment le pays, les canaux et largeurs autorisés. Le code pays doit correspondre au lieu réel de déploiement.

```text
[WAC-wlan] regulatory-domain-profile name <profil-domaine>
[WAC-wlan-regulate-domain-<profil-domaine>] country-code <code-pays>
```

### Profil SSID

```text
[WAC-wlan] ssid-profile name <profil-ssid>
[WAC-wlan-ssid-prof-<profil-ssid>] ssid <nom-visible>
```

Le profil peut aussi limiter le nombre de stations du VAP ou masquer le SSID. Le masquage ne remplace pas une politique de sécurité robuste.

### Profil de sécurité

```text
[WAC-wlan] security-profile name <profil-securite>
[WAC-wlan-sec-prof-<profil-securite>] security <politique>
```

Le support présente les familles open, WPA, WPA-WPA2, WPA2, WPA3 et WPA2-WPA3. Choisissez la politique en fonction des capacités des terminaux et des exigences de sécurité ; évitez de retenir `open` comme configuration de production par simple commodité de lab.

### Profil VAP

Le VAP profile assemble le service : chemin des données, VLAN de service, SSID et sécurité.

```text
[WAC-wlan] vap-profile name <profil-vap>
[WAC-wlan-vap-prof-<profil-vap>] forward-mode { direct-forward | tunnel }
[WAC-wlan-vap-prof-<profil-vap>] service-vlan vlan-id <vlan-service>
[WAC-wlan-vap-prof-<profil-vap>] security-profile <profil-securite>
[WAC-wlan-vap-prof-<profil-vap>] ssid-profile <profil-ssid>
```

## Étape 5 — Appliquer les profils

Liez le VAP profile aux radios du groupe AP et le profil réglementaire au groupe :

```text
[WAC-wlan] ap-group name <groupe-ap>
[WAC-wlan-ap-group-<groupe-ap>] vap-profile <profil-vap> wlan <id-wlan> radio all
[WAC-wlan-ap-group-<groupe-ap>] regulatory-domain-profile <profil-domaine>
```

Une configuration appliquée directement à un AP prend priorité sur celle du groupe. Cette exception doit rester documentée, sinon deux AP d’un même groupe peuvent se comporter différemment.

## Étape 6 — Vérifier le service

```text
<WAC> display ap all
<WAC> display vap all
<WAC> display vap ssid <nom-visible>
<WAC> display station all
```

L’ordre de lecture est important :

1. AP en état normal ;
2. VAP en état `ON` sur les radios attendues ;
3. station associée ;
4. adresse IP attribuée ;
5. connectivité vers la passerelle et les ressources autorisées.

## Pratique obligatoire : fiche de préparation

Avant toute saisie, remplissez :

| Élément | Valeur à décider |
| --- | --- |
| VLAN et sous-réseau de gestion |  |
| Source CAPWAP |  |
| Mode de découverte du WAC |  |
| Mode d’authentification des AP |  |
| Groupe AP |  |
| Code pays |  |
| SSID |  |
| Politique de sécurité |  |
| Mode direct ou tunnel |  |
| VLAN de service |  |
| Source DHCP des stations |  |

Transformez ensuite chaque ligne en objet de configuration ou en test de validation.

## Troubleshooting orienté profils

| Résultat | Cause probable | Action |
| --- | --- | --- |
| AP absent de `display ap all` | Connectivité ou admission non terminée | Revenir à la source CAPWAP, à la découverte et à l’authentification AP. |
| AP normal, aucun VAP | Profil VAP non lié au groupe/radio | Vérifier l’association `vap-profile ... radio ...`. |
| VAP présent avec mauvais SSID | Mauvais SSID profile référencé | Lire la chaîne VAP → SSID profile. |
| VAP actif, station refusée | Security profile ou limite de stations | Vérifier la politique et les contrôles d’accès STA. |
| Station associée sans adresse | VLAN de service ou DHCP | Contrôler le mode de transfert et le chemin DHCP. |

## À retenir pour l’examen HCIA-Datacom

- La procédure suit : connectivité, AP group, source CAPWAP, admission AP, profils, liaisons, vérifications.
- Chaque AP en ligne appartient à un seul AP group.
- Regulatory domain, SSID, security et VAP sont les profils de base du service.
- Le VAP profile référence SSID et sécurité et porte le mode de transfert et le VLAN de service.
- Les profils peuvent s’appliquer au groupe ou à un AP ; la configuration spécifique à l’AP est prioritaire.
- `display ap all`, `display vap all` et `display station all` valident des étapes différentes.

## Révision TianSemi

1. Pourquoi la connectivité doit-elle être vérifiée avant les profils WLAN ?
2. Quel est l’intérêt d’un AP group ?
3. Pourquoi la source CAPWAP doit-elle être joignable et unique ?
4. Quels modes d’authentification des AP sont disponibles ?
5. Que configurent les profils réglementaire, SSID, sécurité et VAP ?
6. Où définit-on le mode de transfert et le VLAN de service ?
7. Quelle configuration est prioritaire entre AP group et AP individuel ?
8. Dans quel ordre faut-il vérifier AP, VAP, station et adressage ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 905 à 917.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 172 à 186, correspondant aux pages 170 à 184 du document.
