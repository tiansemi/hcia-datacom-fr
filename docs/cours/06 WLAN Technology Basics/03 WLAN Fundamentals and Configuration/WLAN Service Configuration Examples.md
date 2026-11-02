---
id: HUA-HCIA-DC-071
title: "WLAN Service Configuration Examples"
official_title: "WLAN Service Configuration Examples"
course_unit: "6.3.3"
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
source_pages: "Training PDF 918-931; Lab Guide PDF 172-186 (document pages 170-184)"
tags: [huawei, hcia, datacom, wlan, lab, wac, fit-ap, capwap, vap, tunnel-forwarding]
prerequisites:
  - "[[WLAN Service Configuration Procedure]]"
related:
  - "[[WLAN Fundamentals]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Exemple de configuration d’un service WLAN (WLAN Service Configuration Examples)

## Objectif du lab officiel

Construire un WLAN centralisé simple dans lequel :

- le WAC et le Fit AP utilisent une architecture Layer 2 in-path ;
- le WAC fournit DHCP aux AP et aux stations ;
- le VLAN 100 transporte la gestion et le VLAN 101 le service ;
- le trafic utilisateur est centralisé par tunnel CAPWAP ;
- le SSID `employee` est protégé par WPA/WPA2-PSK avec AES ;
- le service reste disponible pendant le roaming dans la zone couverte.

![Topologie et plan de données du lab WLAN WAC + Fit AP](<../../../assets/diagrammes/06 WLAN Technology Basics/wlan-wac-fit-ap-lab-fr.svg>)

## Plan de données

| Élément | Valeur du Training Material |
| --- | --- |
| Réseau AP | VLAN 100 — `10.1.100.0/24` |
| Passerelle et source CAPWAP | `Vlanif100` — `10.1.100.1/24` |
| Réseau stations | VLAN 101 — `10.1.101.0/24` |
| Passerelle stations | `Vlanif101` — `10.1.101.1/24` |
| Groupe AP | `ap-group1` |
| Profil réglementaire | `domain` |
| SSID / profils de service | `employee` |
| Transfert | Tunnel |

Le Lab Guide V2.0 présente la même logique avec des noms adaptés à son exercice. Les commandes ci-dessous suivent le scénario du Training Material tout en remplaçant les secrets par des valeurs de lab explicites à choisir localement.

## Étape 1 — VLAN et DHCP sur le WAC

```text
<WAC> system-view
[WAC] vlan batch 100 101
[WAC] dhcp enable

[WAC] interface Vlanif100
[WAC-Vlanif100] ip address 10.1.100.1 24
[WAC-Vlanif100] dhcp select interface
[WAC-Vlanif100] quit

[WAC] interface Vlanif101
[WAC-Vlanif101] ip address 10.1.101.1 24
[WAC-Vlanif101] dhcp select interface
[WAC-Vlanif101] quit
```

## Étape 2 — Transport du VLAN de gestion

Sur les liens switch–AP et switch–WAC, transportez le VLAN 100. Le port vers l’AP utilise le VLAN de gestion comme PVID.

```text
[SW] vlan batch 100
[SW] interface GigabitEthernet 0/0/1
[SW-GigabitEthernet0/0/1] port link-type trunk
[SW-GigabitEthernet0/0/1] port trunk pvid vlan 100
[SW-GigabitEthernet0/0/1] port trunk allow-pass vlan 100
[SW-GigabitEthernet0/0/1] quit

[SW] interface GigabitEthernet 0/0/2
[SW-GigabitEthernet0/0/2] port link-type trunk
[SW-GigabitEthernet0/0/2] port trunk allow-pass vlan 100
[SW-GigabitEthernet0/0/2] quit
```

Parce que le scénario utilise le transfert par tunnel, le VLAN de service 101 n’a pas à être transporté jusqu’à l’AP sur ces liens : les données utilisateur sont encapsulées vers le WAC.

## Étape 3 — Source CAPWAP et groupe AP

```text
[WAC] capwap source interface Vlanif100
[WAC] wlan
[WAC-wlan] ap-group name ap-group1
[WAC-wlan-ap-group-ap-group1] quit
[WAC-wlan] ap auth-mode mac-auth
```

Sur les versions présentées par le support, la première configuration de la source CAPWAP peut déclencher des invites de création de clés et d’identifiants pour les Fit AP. Utilisez des secrets propres au lab, ne les publiez pas et ne réutilisez pas les valeurs d’exemple en production.

## Étape 4 — Importer et rattacher l’AP

Récupérez la MAC réelle de l’AP, par exemple avec LLDP sur le switch, puis importez-le :

```text
[WAC-wlan] ap-id 0 ap-mac <mac-ap>
[WAC-wlan-ap-0] ap-name ap1
[WAC-wlan-ap-0] ap-group ap-group1
[WAC-wlan-ap-0] quit
```

Mettez l’AP sous tension et attendez l’état `nor` :

```text
<WAC> display ap all
```

Ne poursuivez pas vers les profils si l’AP n’est pas normal. À ce stade, corrigez l’adressage, la découverte, CAPWAP ou l’admission.

## Étape 5 — Construire les profils

```text
[WAC] wlan

[WAC-wlan] regulatory-domain-profile name domain
[WAC-wlan-regulate-domain-domain] country-code <code-pays-réel>
[WAC-wlan-regulate-domain-domain] quit

[WAC-wlan] ssid-profile name employee
[WAC-wlan-ssid-prof-employee] ssid employee
[WAC-wlan-ssid-prof-employee] quit

[WAC-wlan] security-profile name employee
[WAC-wlan-sec-prof-employee] security wpa-wpa2 psk pass-phrase <secret-lab> aes
[WAC-wlan-sec-prof-employee] quit

[WAC-wlan] vap-profile name employee
[WAC-wlan-vap-prof-employee] forward-mode tunnel
[WAC-wlan-vap-prof-employee] service-vlan vlan-id 101
[WAC-wlan-vap-prof-employee] security-profile employee
[WAC-wlan-vap-prof-employee] ssid-profile employee
[WAC-wlan-vap-prof-employee] quit
```

Le Training Material emploie `CN` pour son exemple. Dans une mise en œuvre réelle, le code pays doit correspondre à la réglementation du lieu de déploiement.

## Étape 6 — Appliquer le service au groupe

```text
[WAC-wlan] ap-group name ap-group1
[WAC-wlan-ap-group-ap-group1] vap-profile employee wlan 1 radio all
[WAC-wlan-ap-group-ap-group1] regulatory-domain-profile domain
[WAC-wlan-ap-group-ap-group1] quit
```

L’application du profil réglementaire peut réinitialiser les paramètres de canal et de puissance et provoquer un redémarrage radio. Lisez l’avertissement avant de confirmer sur un réseau réel.

## Étape 7 — Vérifier de bout en bout

### AP et VAP

```text
<WAC> display ap all
<WAC> display vap ssid employee
```

Résultats attendus :

- l’AP est en état `nor` ;
- un VAP est créé sur chaque radio ciblée ;
- le champ `Status` des VAP vaut `ON` ;
- le SSID et le mode d’authentification correspondent au plan.

### Station et adressage

Connectez une station au SSID avec le secret du lab, puis contrôlez :

```text
<WAC> display station all
<WAC> display ip pool interface Vlanif101 used
```

La station doit apparaître associée, obtenir une adresse `10.1.101.0/24` et joindre sa passerelle `10.1.101.1`.

## Variantes d’observation

| Modification | Effet attendu |
| --- | --- |
| `forward-mode direct-forward` | Le VLAN de service doit être acheminé entre AP et réseau supérieur. |
| Mauvaise MAC importée | L’AP attendu ne passe pas en état normal. |
| VAP non lié au groupe | AP normal, mais aucun service `employee` sur ses radios. |
| VLAN 101 sans DHCP | Association possible, mais absence d’adresse station. |
| Mauvais secret | Échec d’accès avant l’obtention d’une adresse. |

## Checklist de preuve

- [ ] capture de `display ap all` avec l’état normal ;
- [ ] capture de `display vap ssid employee` avec `Status ON` ;
- [ ] capture de `display station all` ;
- [ ] bail visible dans le pool de `Vlanif101` ;
- [ ] test de la passerelle ;
- [ ] explication du chemin des données en mode tunnel ;
- [ ] sauvegarde de la configuration après validation.

## À retenir pour l’examen HCIA-Datacom

- Le scénario officiel sépare gestion VLAN 100 et service VLAN 101.
- Le WAC fournit DHCP, sert de source CAPWAP et centralise les données tunnelées.
- L’AP doit être normal avant l’application du service.
- SSID et sécurité sont référencés par le VAP profile ; le VAP est lié au groupe AP.
- `display vap ssid ...` valide l’activation du service sur les radios.
- En tunnel forwarding, le VLAN de service n’est pas transporté vers l’AP comme en direct forwarding.

## Révision TianSemi

1. Pourquoi le lab utilise-t-il deux VLAN distincts ?
2. Quel rôle remplit `Vlanif100` ?
3. Pourquoi seul le VLAN de gestion est-il autorisé vers l’AP dans ce scénario tunnel ?
4. À quel moment faut-il vérifier `display ap all` ?
5. Comment SSID profile, security profile et VAP profile sont-ils liés ?
6. Que signifie l’état `ON` dans `display vap ssid employee` ?
7. Quelles commandes prouvent qu’une station est associée et adressée ?
8. Quelle adaptation réseau exige le passage au transfert direct ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 918 à 931.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 172 à 186, correspondant aux pages 170 à 184 du document, lab « WAC + Fit AP Networking Configuration ».
