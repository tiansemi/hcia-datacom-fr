---
id: HUA-HCIA-DC-079
title: "Introduction to Paramiko APIs"
official_title: "Introduction to Paramiko APIs"
course_unit: "7.2.3"
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
source_pages: "PDF 1004-1010"
tags: [huawei, hcia, datacom, python, paramiko, ssh, sshclient, exec-command, invoke-shell]
prerequisites:
  - "[[Overview of Programming Languages and Introduction to Python]]"
related:
  - "[[Example for Network Programming Using Paramiko]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Introduction aux API Paramiko (Introduction to Paramiko APIs)

## Paramiko et SSHv2

Paramiko est un module Python implémentant SSHv2. Il permet l’authentification par mot de passe ou clé publique, l’exécution distante de commandes et le transfert de fichiers.

![Cycle SSHClient et choix entre exec_command et invoke_shell](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/paramiko-sshclient-fr.svg>)

Une API fournit des classes et méthodes prêtes à l’emploi. L’ingénieur utilise leur contrat sans réimplémenter le protocole SSH. `SSHClient` constitue l’interface haut niveau principale et encapsule notamment Transport, Channel et SFTPClient.

## Cycle de vie d’un SSHClient

```python
import paramiko

client = paramiko.SSHClient()
client.load_system_host_keys()
client.set_missing_host_key_policy(paramiko.RejectPolicy())
client.connect(
    hostname="192.0.2.10",
    port=22,
    username="automation",
    key_filename="id_rsa",
)

stdin, stdout, stderr = client.exec_command("display version")
output = stdout.read().decode("utf-8")
errors = stderr.read().decode("utf-8")
client.close()
```

Le Training Material illustre aussi `AutoAddPolicy()`. Cette politique accepte automatiquement une clé d’hôte inconnue. Elle simplifie un lab, mais retire la vérification d’identité du serveur. Préenregistrez la clé attendue et conservez `RejectPolicy`, valeur par défaut, pour une exploitation sûre.

## Méthodes principales

| Méthode | Rôle |
| --- | --- |
| `connect()` | Établit la connexion et authentifie le client. |
| `set_missing_host_key_policy()` | Définit le traitement d’une clé d’hôte inconnue. |
| `exec_command()` | Exécute une commande dans un canal dédié. |
| `invoke_shell()` | Ouvre une session interactive et persistante. |
| `close()` | Ferme la connexion et libère les ressources. |

`connect()` accepte notamment `hostname`, `port`, `username`, `password`, `key_filename` ou un objet `pkey`. Seul `hostname` est obligatoire dans la signature présentée, mais l’authentification doit évidemment disposer des informations nécessaires.

## Politique de clé d’hôte

| Politique | Comportement | Usage |
| --- | --- | --- |
| `RejectPolicy` | Refuse une clé inconnue | Choix sûr par défaut. |
| `WarningPolicy` | Accepte et émet un avertissement | Diagnostic contrôlé, pas une validation d’identité. |
| `AutoAddPolicy` | Accepte et enregistre automatiquement | Lab isolé uniquement. |

La clé d’hôte identifie l’équipement SSH ; elle ne doit pas être confondue avec la clé privée du compte client.

## `exec_command()` ou `invoke_shell()`

### Commande isolée

`exec_command()` renvoie trois flux :

- `stdin` pour fournir une entrée ;
- `stdout` pour la sortie normale ;
- `stderr` pour les erreurs.

Il convient aux serveurs acceptant une commande non interactive par canal. Lisez les deux sorties avant de conclure au succès.

### CLI réseau interactive

`invoke_shell()` ouvre une session persistante et permet plusieurs commandes :

```python
channel = client.invoke_shell()
channel.send("screen-length 0 temporary\n")
channel.send("display current-configuration\n")
raw_output = channel.recv(65535)
text_output = raw_output.decode("utf-8")
```

Sur une CLI réseau, il faut gérer le prompt, les délais, la pagination, les confirmations et la réception partielle. Un unique `sleep()` et un seul `recv()` peuvent tronquer la sortie ; un script robuste lit jusqu’au prompt attendu ou jusqu’à un timeout maîtrisé.

## Pratique TianSemi : comparer les canaux

1. Ouvrez une connexion avec clé d’hôte connue.
2. Testez `exec_command("display version")` et lisez stdout/stderr.
3. Ouvrez ensuite `invoke_shell()`.
4. désactivez la pagination et envoyez deux commandes ;
5. implémentez une boucle de réception bornée par timeout ;
6. journalisez la cible, la commande et le résultat, jamais les secrets ;
7. fermez la connexion dans un bloc `finally`.

## Troubleshooting

| Symptôme | Vérification |
| --- | --- |
| `AuthenticationException` | Compte, méthode, clé, passphrase et droits SSH. |
| `BadHostKeyException` | Clé d’hôte modifiée : vérifier l’équipement avant toute mise à jour. |
| Timeout de connexion | Adresse, port, route, ACL et service STelnet. |
| Sortie vide | Méthode de canal, délai, prompt et lecture stdout/stderr. |
| Sortie tronquée | Boucle `recv`, taille du tampon et détection du prompt. |
| Script bloqué | Pagination ou confirmation interactive non traitée. |

## À retenir pour l’examen HCIA-Datacom

- Paramiko implémente SSHv2 et prend en charge mot de passe et clé publique.
- `SSHClient` fournit l’interface de haut niveau.
- `connect()` établit et authentifie ; `close()` termine la session.
- RejectPolicy, WarningPolicy et AutoAddPolicy traitent les clés d’hôte inconnues.
- `exec_command()` exécute une commande et renvoie stdin/stdout/stderr.
- `invoke_shell()` ouvre une session interactive capable d’envoyer plusieurs commandes.
- Les octets reçus doivent être décodés, par exemple en UTF-8.

## Révision TianSemi

1. Quelles fonctions SSH Paramiko fournit-il ?
2. Quel rôle remplit `SSHClient` ?
3. Quels paramètres de `connect()` servent à l’authentification ?
4. Pourquoi RejectPolicy est-il préférable à AutoAddPolicy ?
5. Que renvoie `exec_command()` ?
6. Quand utiliser `invoke_shell()` ?
7. Pourquoi un seul `sleep()` suivi d’un seul `recv()` est-il fragile ?
8. Quelles erreurs faut-il distinguer lors d’un échec SSH ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1004 à 1010.
