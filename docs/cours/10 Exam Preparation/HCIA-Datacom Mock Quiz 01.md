---
id: HUA-HCIA-DC-EXAM-004
title: "HCIA-Datacom Mock Quiz 01"
category: "Huawei HCIA-Datacom Exam Preparation"
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
module: "10 Exam Preparation"
type: "mock-quiz"
source: "Original TianSemi questions derived from HCIA-Datacom V2.0 official materials"
source_pages: "Training PDF 1-1117; Lab Guide PDF 14-270"
tags: [huawei, hcia, datacom, quiz, exam-preparation]
publication_status: "not-started"
site_url:
---

# Quiz blanc TianSemi 01 — HCIA-Datacom V2.0

## Consignes

Ce quiz est une création TianSemi destinée au diagnostic. Il ne contient pas de questions officielles Huawei et ne reproduit pas le format ou la pondération de l’examen réel.

- Une seule réponse sauf mention **plusieurs réponses**.
- Répondre sans notes, puis justifier chaque choix.
- Un choix correct sans justification vaut « à revoir ».

## Questions

### Fondations

1. Lors de l’encapsulation, quelle action décrit le mieux le rôle de chaque couche ?
   A. Supprimer les informations de contrôle de la couche supérieure
   B. Ajouter les informations nécessaires à son propre traitement
   C. Convertir systématiquement les données en adresse IP
   D. Choisir toujours le chemin physique complet

2. Quel équipement prend principalement une décision de transfert à partir d’une table MAC ?
   A. Routeur
   B. Switch Ethernet
   C. Serveur DHCP
   D. WAC uniquement

3. **Plusieurs réponses.** Pourquoi les protocoles standardisés sont-ils nécessaires ?
   A. Pour définir des règles communes d’échange
   B. Pour favoriser l’interopérabilité
   C. Pour supprimer le besoin d’adressage
   D. Pour structurer l’encodage et l’interprétation des informations

4. Dans VRP, quelle commande fournit le contexte de configuration actif de la vue courante ?
   A. `display this`
   B. `save`
   C. `reset saved-configuration`
   D. `reboot`

### Ethernet

5. Un switch reçoit une trame unicast dont l’adresse MAC de destination est inconnue dans le VLAN. Que fait-il normalement ?
   A. Il la diffuse sur les autres ports autorisés du même VLAN
   B. Il l’envoie uniquement au routeur par défaut
   C. Il la supprime toujours
   D. Il la transmet dans tous les VLAN

6. Quel est le rôle du PVID lors de la réception d’une trame non taggée sur un port Ethernet ?
   A. Choisir le root bridge
   B. Associer la trame à un VLAN
   C. Déterminer l’adresse MAC source
   D. Calculer le coût OSPF

7. Quel critère est comparé en premier lors de l’élection du root bridge STP ?
   A. La vitesse du port
   B. Le bridge ID
   C. L’adresse IP de management
   D. Le nombre de VLAN

8. Quel avantage LACP apporte-t-il par rapport à une agrégation manuelle ?
   A. La négociation et la détection cohérente des liens membres
   B. Le routage dynamique inter-VLAN
   C. Le chiffrement des trames
   D. La suppression de la table MAC

### IP

9. Combien d’adresses hôtes utilisables possède normalement un sous-réseau IPv4 `/27` classique ?
   A. 14
   B. 30
   C. 32
   D. 62

10. Deux routes correspondent à une destination. Quel principe décide d’abord laquelle utiliser ?
    A. La plus ancienne
    B. Le longest prefix match
    C. La préférence la plus élevée numériquement
    D. Le next hop dont l’IP est la plus petite

11. Quel ordre correspond au processus DHCP IPv4 classique ?
    A. Discover, Offer, Request, Acknowledge
    B. Request, Discover, Acknowledge, Offer
    C. Offer, Discover, Request, Release
    D. Discover, Request, Offer, Acknowledge

12. Un voisin OSPF est Full, mais une Loopback distante n’apparaît pas dans la table de routage. Quelle hypothèse faut-il vérifier en priorité ?
    A. La Loopback n’est peut-être pas annoncée dans OSPF
    B. STP bloque nécessairement la Loopback
    C. La table MAC contient trop d’entrées
    D. Le serveur DHCP est arrêté

13. En IPv6, quel protocole assure notamment la découverte des voisins et des routeurs ?
    A. ARP
    B. ICMPv6/NDP
    C. FTP
    D. LACP

14. Quelle propriété décrit une adresse IPv6 link-local ?
    A. Elle est routée globalement par défaut
    B. Elle est limitée au lien local
    C. Elle remplace toutes les adresses globales
    D. Elle nécessite toujours DHCPv6 stateful

### Sécurité et services

15. Pourquoi l’ordre des règles d’une ACL est-il déterminant ?
    A. Le premier match applicable décide généralement de l’action
    B. Toutes les règles sont additionnées avant décision
    C. Seule la dernière règle est lue
    D. L’ordre ne compte que pour IPv6

16. Quel élément de AAA répond à la question « quelles actions cet utilisateur peut-il effectuer » ?
    A. Authentication
    B. Authorization
    C. Accounting
    D. Address translation

17. Quel protocole est le choix recommandé pour une administration distante chiffrée ?
    A. Telnet
    B. FTP
    C. SSH/STelnet
    D. TFTP

18. Easy IP traduit généralement l’adresse source privée vers :
    A. L’adresse publique de l’interface de sortie
    B. L’adresse MAC du serveur
    C. L’adresse du DNS
    D. Une adresse link-local

### Data center et WLAN

19. Quel besoin distingue particulièrement un DCN de calcul intelligent ?
    A. Des échanges à forte bande passante et faible latence, souvent sensibles aux pertes
    B. L’absence totale de redondance
    C. Uniquement du trafic web utilisateur
    D. La suppression des switches

20. Dans l’architecture dual-track présentée par Huawei, l’idée générale est de :
    A. faire coexister des infrastructures adaptées à des charges de calcul différentes
    B. remplacer IP par STP
    C. interdire les réseaux de stockage
    D. utiliser un seul lien physique sans secours

21. Un ESS correspond le mieux à :
    A. plusieurs BSS fournissant un même service WLAN étendu
    B. une table de routage OSPF
    C. une ACL avancée
    D. un agrégat Ethernet

22. Dans une architecture WAC + Fit AP, CAPWAP sert principalement à :
    A. transporter le contrôle entre AP et WAC, et selon le mode certaines données
    B. remplacer 802.11 sur la radio
    C. attribuer une adresse MAC au STA
    D. élire le root bridge STP

23. En transfert direct, où les données utilisateur quittent-elles normalement le chemin radio pour rejoindre le réseau filaire ?
    A. Directement au niveau de l’AP vers le réseau de service
    B. Uniquement après traversée du WAC dans un tunnel de données
    C. Sur le serveur DHCP
    D. Sur le routeur Internet uniquement

24. Pourquoi 802.11 utilise-t-il CSMA/CA plutôt que le mécanisme historique CSMA/CD d’Ethernet partagé ?
    A. Une station radio ne détecte pas de manière fiable toutes les collisions pendant l’émission
    B. Les WLAN n’utilisent aucune adresse
    C. CSMA/CA chiffre automatiquement les trames
    D. STP l’impose

### O&M, dépannage et campus

25. Quel protocole est conçu pour une configuration structurée à partir de modèles de données et d’opérations RPC ?
    A. NETCONF
    B. Telnet
    C. ARP
    D. LACP

26. RESTCONF expose généralement des données de configuration via :
    A. des méthodes HTTP et des ressources structurées
    B. des trames STP
    C. un signal radio CAPWAP
    D. la table MAC

27. Pourquoi `AutoAddPolicy` de Paramiko est-il déconseillé dans un usage sécurisé ?
    A. Il accepte automatiquement une clé hôte inconnue
    B. Il désactive Python
    C. Il empêche SSH d’utiliser un mot de passe
    D. Il chiffre deux fois le trafic

28. Quelle est la meilleure première action face à une panne signalée ?
    A. Modifier plusieurs paramètres probables
    B. Reproduire et délimiter le symptôme
    C. Redémarrer tous les équipements
    D. Effacer la configuration

29. Dans le campus du lab 17, quel équipement joue à la fois la passerelle de services et le serveur DHCP ?
    A. AP1
    B. Core1
    C. PC2
    D. ACC1

30. Pourquoi valider les liens directs avant d’activer OSPF ?
    A. Pour séparer un défaut d’adressage/lien d’un défaut de protocole de routage
    B. Pour éviter toute route directe
    C. Pour supprimer ICMP
    D. Pour créer automatiquement les VLAN

31. Un client DHCP ne reçoit rien. Le trunk transporte le bon VLAN, le pool est correct, mais la VLANIF passerelle est `Administratively DOWN`. Quelle correction est la plus ciblée ?
    A. `undo shutdown` sur la VLANIF
    B. Changer le router ID OSPF
    C. Effacer la table MAC
    D. Activer FTP

32. AP1 est `nor`, le VAP est `ON`, mais il diffuse un SSID inattendu. Quelle relation faut-il inspecter ?
    A. Le profil SSID référencé par le profil VAP
    B. La préférence d’une route statique sur R1
    C. Le bridge ID de Core1
    D. Le pool NAT

## Correction commentée

| Q | Réponse | Justification courte |
| ---: | --- | --- |
| 1 | B | Chaque couche ajoute les informations utiles à son traitement et au pair distant. |
| 2 | B | Le switch commute principalement selon la destination MAC apprise. |
| 3 | A, B, D | Les standards décrivent des règles communes d’encodage et d’échange. |
| 4 | A | `display this` montre la configuration de la vue courante. |
| 5 | A | L’unicast inconnu est floodé dans le même VLAN, hors port d’entrée. |
| 6 | B | Le PVID classe une trame entrante non taggée. |
| 7 | B | STP compare le bridge ID, composé notamment de priorité et MAC. |
| 8 | A | LACP négocie et surveille la cohérence des liens membres. |
| 9 | B | Un `/27` offre 32 adresses, généralement 30 hôtes utilisables. |
| 10 | B | La route au préfixe le plus spécifique est examinée avant préférence/coût. |
| 11 | A | DORA : Discover, Offer, Request, Acknowledge. |
| 12 | A | Un voisin Full ne garantit pas l’annonce de tous les préfixes. |
| 13 | B | NDP s’appuie sur ICMPv6 et remplit plusieurs fonctions d’ARP et de découverte. |
| 14 | B | Une link-local reste sur le lien et sert notamment à NDP. |
| 15 | A | La première règle correspondante détermine l’action dans le traitement courant. |
| 16 | B | L’autorisation définit les droits après identification. |
| 17 | C | SSH/STelnet protège l’administration distante. |
| 18 | A | Easy IP réutilise l’adresse de l’interface de sortie. |
| 19 | A | Les fabric de calcul intelligent visent débit, faible latence et maîtrise des pertes. |
| 20 | A | Les deux voies répondent à des profils de calcul et de réseau distincts. |
| 21 | A | Un ESS étend le service à travers plusieurs BSS. |
| 22 | A | CAPWAP porte le contrôle AP–WAC et peut aussi transporter les données en tunnel. |
| 23 | A | En direct forwarding, l’AP remet les données au réseau de service local. |
| 24 | A | Le médium radio rend la détection de collision pendant émission peu fiable. |
| 25 | A | NETCONF utilise RPC et modèles structurés, souvent YANG. |
| 26 | A | RESTCONF emploie HTTP sur des ressources de données structurées. |
| 27 | A | Accepter une clé inconnue sans validation ouvre la voie à l’interception. |
| 28 | B | Il faut disposer d’un symptôme reproductible avant de formuler et tester une hypothèse. |
| 29 | B | Core1 assure ces deux fonctions dans le scénario officiel. |
| 30 | A | Le test des dépendances réduit le périmètre de recherche. |
| 31 | A | La cause démontrée est l’arrêt administratif de la VLANIF. |
| 32 | A | Le VAP doit référencer le bon profil SSID. |

## Exploiter le résultat

Regroupez vos erreurs par mécanisme, pas seulement par numéro. Pour chaque mauvaise réponse, ajoutez une ligne au [[HCIA-Datacom Weak Points Tracker]] et une preuve à produire.

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1 à 1117.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 14 à 270.
