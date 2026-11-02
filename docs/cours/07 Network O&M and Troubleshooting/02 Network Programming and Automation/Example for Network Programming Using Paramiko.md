---
id: HUA-HCIA-DC-080
title: "Example for Network Programming Using Paramiko"
official_title: "Example for Network Programming Using Paramiko"
course_unit: "7.2.4"
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
source_pages: "Training PDF 1012-1019; Lab Guide PDF 188-197 (document pages 186-195)"
tags: [huawei, hcia, datacom, python, paramiko, lab, stelnet, rsa, automation]
prerequisites:
  - "[[Introduction to Paramiko APIs]]"
related:
  - "[[Overview of Network Troubleshooting]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Exemple de programmation réseau avec Paramiko

## Objectif

Configurer un switch Huawei comme serveur STelnet, authentifier un client Python par clé RSA, ouvrir une session avec Paramiko et récupérer la configuration courante.

![Chaîne du lab Paramiko avec authentification RSA](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/paramiko-rsa-lab-fr.svg>)

Le Training Material utilise `192.168.56.100/24` sur le serveur et `192.168.56.1/24` sur le client. Le Lab Guide propose un exercice analogue avec authentification par mot de passe. La variante RSA ci-dessous suit le Training Material et évite de stocker un mot de passe dans le script.

## Étape 1 — Préparer l’interface de gestion

```text
<HUAWEI> system-view immediately
[HUAWEI] sysname SSH-Server
[SSH-Server] interface GE 1/0/0
[SSH-Server-GE1/0/0] ip address 192.168.56.100 24
[SSH-Server-GE1/0/0] quit
```

Depuis le client, vérifiez d’abord la joignabilité de `192.168.56.100`.

## Étape 2 — Activer STelnet et les VTY

```text
[SSH-Server] stelnet server enable
[SSH-Server] ssh server-source all-interface
[SSH-Server] user-interface vty 0 4
[SSH-Server-ui-vty0-4] authentication-mode aaa
[SSH-Server-ui-vty0-4] protocol inbound ssh
[SSH-Server-ui-vty0-4] quit
```

`all-interface` facilite le lab. En production, limitez la source aux interfaces de gestion nécessaires.

## Étape 3 — Créer le compte SSH

```text
[SSH-Server] aaa
[SSH-Server-aaa] local-user client password irreversible-cipher <secret-de-secours>
[SSH-Server-aaa] local-user client privilege level 3
[SSH-Server-aaa] local-user client service-type ssh
[SSH-Server-aaa] quit

[SSH-Server] ssh user client
[SSH-Server] ssh user client authentication-type rsa
[SSH-Server] ssh user client service-type stelnet
```

Le mot de passe local existe dans l’exemple officiel, mais le compte SSH est configuré ici pour l’authentification RSA.

## Étape 4 — Générer la paire de clés sur le client

```text
ssh-keygen -t rsa -b 4096
```

Choisissez un chemin protégé et une passphrase. Le fichier privé ne quitte jamais le client ; seul le contenu de `id_rsa.pub` est importé sur le switch.

## Étape 5 — Importer la clé publique

```text
[SSH-Server] rsa peer-public-key rsa01 encoding-type openssh
[SSH-Server-rsa-public-key] public-key-code begin
[SSH-Server-rsa-public-key-rsa-key-code] ssh-rsa AAAA... <clé-publique-client>
[SSH-Server-rsa-public-key-rsa-key-code] peer-public-key end
[SSH-Server] ssh user client assign rsa-key rsa01
```

Recopiez la clé publique sans la tronquer. Le contenu réel est propre à votre client ; ne réutilisez jamais la chaîne abrégée du support.

## Étape 6 — Enregistrer la clé d’hôte

Avant d’exécuter Python, établissez une première connexion SSH contrôlée et vérifiez l’empreinte du switch :

```text
ssh client@192.168.56.100
```

Cette étape alimente le fichier `known_hosts`. Paramiko pourra ensuite refuser une identité inconnue ou modifiée.

## Étape 7 — Script Python

```python
from pathlib import Path
import time

import paramiko


HOST = "192.168.56.100"
USERNAME = "client"
PRIVATE_KEY = Path.home() / ".ssh" / "id_rsa"


def read_until_idle(channel, timeout=10):
    """Lit la sortie jusqu’à inactivité ou expiration du délai."""
    channel.settimeout(1.0)
    chunks = []
    deadline = time.monotonic() + timeout
    while time.monotonic() < deadline:
        try:
            data = channel.recv(65535)
        except TimeoutError:
            if chunks:
                break
            continue
        if not data:
            break
        chunks.append(data)
    return b"".join(chunks).decode("utf-8", errors="replace")


def main():
    client = paramiko.SSHClient()
    client.load_system_host_keys()
    client.set_missing_host_key_policy(paramiko.RejectPolicy())
    try:
        client.connect(
            hostname=HOST,
            port=22,
            username=USERNAME,
            key_filename=str(PRIVATE_KEY),
            timeout=10,
        )
        channel = client.invoke_shell()
        read_until_idle(channel)
        channel.send("screen-length 0 temporary\n")
        channel.send("display current-configuration\n")
        print(read_until_idle(channel))
    finally:
        client.close()


if __name__ == "__main__":
    main()
```

Le support officiel utilise `AutoAddPolicy()`, attend trois secondes puis appelle une seule fois `recv()`. La version TianSemi conserve les mêmes étapes fonctionnelles, mais vérifie la clé d’hôte, borne la connexion et lit dans une boucle.

## Variante du Lab Guide : mot de passe

Le Lab Guide configure l’utilisateur `python`, puis utilise :

```python
client.connect(
    "192.168.1.254",
    username="python",
    password=<secret-fourni-au-runtime>,
)
```

Ne placez pas le secret littéralement dans le fichier. Utilisez une saisie `getpass`, une variable d’environnement protégée ou un gestionnaire de secrets selon l’environnement.

## Vérification

Le résultat doit montrer :

- une session VTY SSH ouverte ;
- la commande de désactivation de pagination acceptée ;
- la sortie de `display current-configuration` ;
- aucune erreur d’authentification ou de clé d’hôte ;
- la fermeture de la session après lecture.

Sur le switch, vérifiez au besoin les utilisateurs en ligne et les paramètres SSH. En cas d’échec, testez d’abord le client OpenSSH avant Paramiko afin de séparer un problème serveur d’un problème Python.

## Troubleshooting du lab

| Symptôme | Cause probable |
| --- | --- |
| Ping impossible | Adresse, interface, route ou pare-feu local. |
| OpenSSH fonctionne, Paramiko échoue | Chemin de clé, passphrase, known_hosts ou version d’algorithme. |
| `Server not found in known_hosts` | Clé d’hôte non enregistrée ou mauvais fichier. |
| Authentification refusée | Clé publique mal importée, utilisateur non associé ou type RSA absent. |
| Prompt visible mais sortie incomplète | Pagination, temps de lecture ou boucle de réception. |
| Le script reste bloqué | Prompt interactif ou timeout non géré. |

## Checklist de preuve

- [ ] connectivité IP client–serveur ;
- [ ] service STelnet actif et VTY limité à SSH ;
- [ ] compte AAA et utilisateur SSH cohérents ;
- [ ] empreinte d’hôte vérifiée ;
- [ ] clé publique associée au bon utilisateur ;
- [ ] sortie complète de la configuration ;
- [ ] aucun secret ou clé privée ajouté à Git ;
- [ ] session fermée même en cas d’exception.

## À retenir pour l’examen HCIA-Datacom

- Le serveur doit disposer d’une adresse de gestion, de STelnet, de VTY AAA et d’un utilisateur SSH.
- La clé publique du client est importée et affectée à l’utilisateur ; la clé privée reste côté client.
- `invoke_shell()` convient à une CLI interactive et à plusieurs commandes.
- `screen-length 0 temporary` évite la pagination de la sortie.
- Le code doit fermer la connexion après l’opération.
- AutoAddPolicy facilite le lab, mais RejectPolicy et known_hosts protègent l’identité du serveur.

## Révision TianSemi

1. Quelles configurations serveur précèdent l’exécution de Paramiko ?
2. Pourquoi `protocol inbound ssh` est-il configuré sur les VTY ?
3. Quelle différence existe entre clé d’hôte et clé cliente RSA ?
4. Quelle partie de la paire RSA est importée sur le switch ?
5. Pourquoi désactiver temporairement la pagination ?
6. Pourquoi le script utilise-t-il `try/finally` ?
7. Comment diagnostiquer séparément le serveur SSH et le code Paramiko ?
8. Pourquoi ne faut-il pas stocker un mot de passe dans le script ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 1012 à 1019.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 188 à 197, correspondant aux pages 186 à 195 du document, lab « Network Programming and Automation (Optional) ».
