---
id: HUA-HCIA-DC-009
title: "Logging In to Network Devices"
official_title: "Logging In to Network Devices"
course_unit: "1.3.2"
category: "Huawei HCIA-Datacom Topic"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "beginner"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-07-13
updated: 2026-08-10
source: "HCIA-Datacom V2.0 Training Material"
source_pages: "PDF 96-105"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 14-15"
tags: [huawei, hcia, datacom, console, telnet, stelnet, web-ui]
prerequisites:
  - "[[Operating System Overview]]"
related:
  - "[[Configuring Network Devices]]"
lab_required: true
labs:
  - "[[01 Basic Operations on Huawei Network Devices]]"
publication_status: "not-started"
site_url:
---

# Connexion aux équipements réseau (Logging In to Network Devices)

## Gestion d'un équipement réseau

Un équipement peut généralement être administré par :

- la **CLI**, qui offre une gestion fine mais exige la maîtrise des commandes ;
- le **système web**, qui offre une interface graphique intuitive mais ne couvre qu'une partie des fonctions.

Par défaut dans l'exemple du cours :

- la CLI est accessible par le port console ;
- le système web est accessible par HTTPS.

HTTPS associe HTTP à SSL ou TLS afin de chiffrer la communication et d'authentifier le serveur web.

![Comparaison des accès console, Telnet, STelnet et web à un équipement Huawei](<../../../assets/diagrammes/01 Data Communication Network Basics/network-device-login-methods-fr.svg>)

## Connexion à la CLI

Huawei présente trois modes : console, Telnet et STelnet.

### Éléments nécessaires à une connexion

| Élément | Rôle |
| --- | --- |
| Interface utilisateur | fournit le point d'entrée et gère la session |
| Informations de gestion utilisateur | authentifient, autorisent et définissent les droits |
| Service terminal | fournit le protocole de session, notamment Telnet ou STelnet |

Le système prend en charge :

- l'interface **console**, pour l'utilisateur connecté localement ;
- les interfaces **VTY**, créées pour les connexions Telnet ou STelnet.

Le support indique qu'un maximum de 21 utilisateurs peut utiliser simultanément les interfaces VTY dans l'exemple, avec une valeur susceptible de varier selon le modèle et la version.

### Console

Le port console est un port série présent sur l'équipement. La connexion locale est utilisée notamment pour la première mise en service.

```text
PC administrateur -- port COM / adaptateur USB-série -- câble console -- équipement
```

Huawei décrit la connexion physique suivante :

- connecter le côté terminal au port COM du PC ;
- connecter l'autre extrémité au port console de l'équipement ;
- utiliser un adaptateur série vers USB si le PC ne dispose pas de port série.

### Telnet et STelnet

Telnet et STelnet permettent une connexion distante par terminal virtuel. Un client demande une session à un serveur Telnet ou STelnet. Un équipement réseau peut jouer le rôle de client ou de serveur.

STelnet établit une connexion plus sûre que Telnet. Les mécanismes détaillés sont étudiés plus tard dans le cours « Network Services and Applications ».

## Première connexion par console

Huawei utilise PuTTY comme exemple de logiciel de terminal.

### Paramètres série

| Paramètre | Valeur par défaut présentée |
| --- | --- |
| Débit | 9600 bit/s |
| Bits de données | 8 |
| Bits d'arrêt | 1 |
| Parité | aucune |
| Contrôle de flux | aucun |

Procédure :

1. ouvrir PuTTY et créer une session ;
2. sélectionner le type de connexion série ;
3. choisir le port COM réellement utilisé ;
4. appliquer les paramètres série ci-dessus ;
5. ouvrir la session.

Lors de la première connexion, l'équipement demande de définir un mot de passe de 8 à 16 caractères. Lors d'une connexion ultérieure, l'utilisateur saisit le mot de passe ou, si AAA est utilisé, un nom d'utilisateur et un mot de passe.

Après authentification, le prompt utilisateur apparaît :

```text
<HUAWEI>
```

## Configuration initiale après la première connexion

Un équipement neuf doit recevoir des paramètres système de base avant la configuration des services. Huawei cite :

- l'heure système ;
- le nom de l'équipement ;
- l'adresse IP de management.

Les commandes correspondantes sont présentées dans l'unité « Configuring Network Devices ».

## Connexion au système web

L'équipement contient un serveur web qui permet l'administration par HTTPS.

Deux situations sont distinguées :

- **première connexion** : utilisation de l'adresse de management par défaut si le modèle le permet ;
- **connexions suivantes** : utilisation d'une adresse de management, d'un port HTTPS et d'un compte administrateur configurés.

Dans l'exemple CloudEngine S5755-H du support :

- adresse par défaut : `192.168.1.253/24` ;
- port HTTPS par défaut : `8443`.

Ces valeurs dépendent du modèle et de la version. La documentation du produit reste la référence.

### Première connexion web dans l'exemple

1. Relier le PC au port de management, ou au premier port Ethernet si le modèle n'a pas de port dédié.
2. Maintenir le bouton MODE pendant 6 secondes jusqu'à l'état indiqué par le voyant MST.
3. Configurer le PC dans le même sous-réseau que `192.168.1.253/24`.
4. Ouvrir `https://192.168.1.253:8443`.
5. Créer le compte administrateur demandé.
6. Se connecter avec ce compte.
7. Modifier le mot de passe initial lorsque le système le demande.
8. Se reconnecter avec le nouveau mot de passe.

Le comportement du bouton MODE est spécifique au produit présenté. Il ne faut pas transposer cette procédure à un autre équipement sans consulter sa documentation.

## Organisation de l'interface web

Huawei présente cinq onglets :

| Onglet | Fonctions principales |
| --- | --- |
| Monitor | état du système, panneau, interfaces, journaux, alarmes et modules optiques |
| Object | objets réutilisables comme adresses et services |
| Configure | VLAN, MAC, interfaces, ARP, DNS, routes, stack et WLAN |
| Diagnose | journaux, collecte d'informations et diagnostic |
| System | administrateurs, horloge, SNMP et mise à niveau |

L'interface comprend également un arbre de navigation, une zone d'opération, des boutons courants comme logout ou save, et un accès à la console CLI.

## Pratique officielle associée

Le lab « Basic Operations on Huawei Network Devices » reprend la connexion par console :

1. connecter le PC au routeur par le câble console ;
2. ouvrir la session CLI ;
3. définir le mot de passe initial ;
4. vérifier l'apparition du prompt `<HUAWEI>` ;
5. poursuivre avec les commandes de base décrites dans la note suivante.

## Résumé fidèle du cours

- La CLI permet une administration fine ; le web simplifie une partie des opérations.
- Console est un accès local ; Telnet et STelnet sont des accès distants par VTY.
- STelnet est plus sûr que Telnet.
- La connexion console par défaut utilise 9600-8-N-1 sans contrôle de flux.
- Le système web utilise HTTPS et nécessite une adresse de management et un compte administrateur.
- Les paramètres par défaut varient selon le modèle et la version.

## Diagnostic des problèmes de connexion

| Symptôme | Vérification prioritaire |
| --- | --- |
| aucun affichage par console | câble, port COM, débit 9600 et paramètres 8-N-1 sans contrôle de flux |
| caractères illisibles | vitesse ou paramètres série incorrects |
| invite présente mais authentification refusée | mot de passe, compte AAA et niveau d'autorisation |
| Telnet ou STelnet inaccessible | connectivité IP, service activé, interfaces VTY et méthode d'authentification |
| interface web inaccessible | adresse de management, port HTTPS, même sous-réseau et état du service web |
| procédure par défaut inefficace | vérifier le modèle et la version dans la documentation produit |

Le diagnostic doit commencer par le mode de connexion utilisé. Une console locale ne dépend pas de la connectivité IP, contrairement à Telnet, STelnet et HTTPS.

## À retenir pour l'examen HCIA-Datacom

- Console correspond à un accès local ; Telnet et STelnet utilisent les interfaces VTY.
- STelnet protège mieux la session que Telnet.
- Les paramètres console présentés sont 9600-8-N-1 sans contrôle de flux.
- Le système web utilise HTTPS et une adresse de management.
- Un seul utilisateur peut utiliser l'interface console ; la capacité VTY dépend de la plateforme.
- Les valeurs par défaut du système web sont spécifiques au modèle présenté.

## Révision TianSemi

1. Quelle différence faut-il établir entre une connexion console et une connexion VTY ?
2. Pourquoi la console reste-t-elle utile lors d'une première mise en service ou d'une perte de connectivité IP ?
3. Quels paramètres série Huawei présente-t-il pour la connexion console par défaut ?
4. Pourquoi STelnet doit-il être préféré à Telnet lorsque les deux sont possibles ?
5. Quels éléments sont nécessaires avant une connexion au système web ?
6. Pourquoi ne faut-il pas appliquer aveuglément l'adresse et le port HTTPS de l'exemple à un autre modèle ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 96-105.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 14-15.
- Huawei Talent, HCIA-Datacom V2.0 : https://e.huawei.com/en/talent/#/cert/product-details?certifiedProductId=1316&authenticationLevel=CTYPE_CARE_HCIA&technicalField=IIC&version=2.0
