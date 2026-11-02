---
id: HUA-HCIA-LAB-014
title: "WAC + Fit AP Networking Configuration"
official_title: "WAC + Fit AP Networking Configuration"
category: "Huawei HCIA-Datacom Lab"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "advanced"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-06
updated: 2026-08-11
module: "09 Labs"
type: "lab"
source: "HCIA-Datacom V2.0 Lab Guide"
source_pages: "PDF 172-187 (guide pages 170-185)"
lab_required: true
tags: [huawei, hcia, datacom, lab, wlan, wac, fit-ap, capwap, vap]
devices: [WAC1, S3, AP1, STA]
commands: [capwap source interface, ap auth-mode, ap-id, security-profile, ssid-profile, vap-profile, display ap all]
publication_status: "not-started"
site_url:
---

# Lab 14 — Configuration WAC + Fit AP

## Objectif et topologie

Mettre AP1 en ligne sur WAC1, diffuser le SSID `HCIA-WLAN`, attribuer une adresse au STA et valider le trafic en transfert direct.

![WAC1 relié via S3 à AP1 et au STA](<../../assets/diagrammes/09 Labs/lab14-wlan-topology-fr.svg>)

| Fonction | Valeur officielle |
| --- | --- |
| VLAN de gestion AP | VLAN 100, passerelle/DHCP `192.168.100.254/24` |
| VLAN de service STA | VLAN 101, passerelle/DHCP `192.168.101.254/24` |
| CAPWAP source | `Vlanif100` |
| Groupe AP | `ap-group1` |
| SSID / profils | `HCIA-WLAN` |
| Transfert | Direct forwarding |

Adaptez le code pays à la réglementation locale ; ne recopiez pas automatiquement `CN`. Utilisez aussi une PSK unique et robuste au lieu de l’exemple du guide.

## 1 — Réseau filaire et DHCP

```text
[WAC1] vlan batch 100 101
[WAC1-GE1/0/13] port link-type trunk
[WAC1-GE1/0/13] port trunk allow-pass vlan 100 101
[S3-GE1/0/1] port link-type trunk
[S3-GE1/0/1] port trunk allow-pass vlan 100 101
[S3-GE1/0/4] port link-type trunk
[S3-GE1/0/4] port trunk allow-pass vlan 100 101
[S3-GE1/0/4] port trunk pvid vlan 100
[WAC1-Vlanif100] ip address 192.168.100.254 24
[WAC1-Vlanif101] ip address 192.168.101.254 24
[WAC1] dhcp enable
[WAC1-Vlanif100] dhcp select interface
[WAC1-Vlanif101] dhcp select interface
```

## 2 — Faire monter AP1

```text
[WAC1] wlan
[WAC1-wlan] ap-group name ap-group1
[WAC1-wlan] regulatory-domain-profile name default
[WAC1-wlan-regulate-domain-default] country-code <CODE-PAYS>
[WAC1-wlan-ap-group-ap-group1] regulatory-domain-profile default
[WAC1] capwap source interface Vlanif 100
[WAC1-wlan] ap auth-mode mac-auth
```

Relevez la MAC réelle avec LLDP, puis importez l’AP :

```text
[S3] display lldp neighbor interface GE 1/0/4
[WAC1-wlan] ap-id 1 ap-mac <MAC-AP1>
[WAC1-wlan-ap-1] ap-name AP1
[WAC1-wlan-ap-1] ap-group ap-group1
[WAC1] display ap all
```

Attendez l’état `nor`. Si l’AP reste dans un état intermédiaire, ne poursuivez pas les profils de service avant d’avoir corrigé la gestion/CAPWAP.

## 3 — Construire le service WLAN

```text
[WAC1-wlan] security-profile name HCIA-WLAN
[WAC1-wlan-sec-prof-HCIA-WLAN] security wpa-wpa2 psk pass-phrase <PSK-LAB> aes
[WAC1-wlan] ssid-profile name HCIA-WLAN
[WAC1-wlan-ssid-prof-HCIA-WLAN] ssid HCIA-WLAN
[WAC1-wlan] vap-profile name HCIA-WLAN
[WAC1-wlan-vap-prof-HCIA-WLAN] forward-mode direct-forward
[WAC1-wlan-vap-prof-HCIA-WLAN] service-vlan vlan-id 101
[WAC1-wlan-vap-prof-HCIA-WLAN] security-profile HCIA-WLAN
[WAC1-wlan-vap-prof-HCIA-WLAN] ssid-profile HCIA-WLAN
[WAC1-wlan-ap-group-ap-group1] vap-profile HCIA-WLAN wlan 1 radio all
```

## Vérifications attendues

```text
[WAC1] display ap all
[WAC1] display vap all
[WAC1] display station all
[WAC1] display ip pool interface Vlanif101 used
```

- AP1 est `nor` dans `ap-group1`.
- Le VAP est actif sur les radios prévues.
- Le STA voit `HCIA-WLAN`, s’authentifie et reçoit `192.168.101.0/24`.
- Le STA joint `192.168.101.254`.

## Troubleshooting

- AP offline : vérifier VLAN 100, PVID, DHCP, MAC importée et source CAPWAP.
- AP online sans SSID : contrôler l’association VAP → groupe AP → radios.
- STA authentifié sans IP : vérifier VLAN 101, transfert direct et pool DHCP.
- PSK refusée : aligner security profile et client, sans publier le secret.
- Radio non conforme : corriger le domaine réglementaire avant émission.

## Révision TianSemi

1. Pourquoi séparer VLAN de gestion AP et VLAN de service STA ?
2. Quel est le rôle de la source CAPWAP ?
3. Pourquoi relever la MAC réelle d’AP1 ?
4. Comment security, SSID et VAP profiles s’imbriquent-ils ?
5. Que signifie direct forwarding ?
6. Quelles preuves confirment successivement AP, VAP puis STA ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 172 à 187 (pages 170 à 185 du guide).
