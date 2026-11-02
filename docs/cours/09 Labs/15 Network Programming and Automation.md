---
id: HUA-HCIA-LAB-015
title: "Network Programming and Automation (Optional)"
official_title: "Network Programming and Automation (Optional)"
category: "Huawei HCIA-Datacom Lab"
domain: "Datacom"
vendor: "Huawei"
certification: "HCIA-Datacom"
version: "V2.0"
difficulty: "advanced"
status: "draft"
author: "MOULO OHOLO Jean Noel"
reviewed: false
created: 2026-08-11
updated: 2026-08-11
module: "09 Labs"
type: "lab"
source: "HCIA-Datacom V2.0 Lab Guide"
source_pages: "PDF 188-197 (guide pages 186-195)"
lab_required: false
tags: [huawei, hcia, datacom, lab, python, paramiko, ssh, automation]
devices: [PC2, S4]
commands: [stelnet server enable, ssh user, local-user, invoke_shell]
publication_status: "not-started"
site_url:
---

# Lab 15 — Programmation réseau avec Paramiko (optionnel)

## Objectif et topologie

Configurer STelnet sur S4, puis utiliser Python/Paramiko depuis PC2 pour ouvrir une session SSH interactive, exécuter des commandes et récupérer la sortie.

![PC2 automatise S4 par SSH avec Paramiko](<../../assets/diagrammes/09 Labs/lab15-paramiko-topology-fr.svg>)

- PC2 : `192.168.1.253/24`.
- S4 `GE1/0/4` : `192.168.1.254/24`.

## 1 — Préparer STelnet sur S4

```text
[S4-GE1/0/4] undo portswitch
[S4-GE1/0/4] ip address 192.168.1.254 24
[S4] stelnet server enable
[S4] ssh user python
[S4] ssh user python authentication-type password
[S4] ssh user python service-type stelnet
[S4] user-interface vty 0 4
[S4-ui-vty0-4] authentication-mode aaa
[S4-ui-vty0-4] protocol inbound ssh
[S4] aaa
[S4-aaa] local-user python password irreversible-cipher <SECRET-LAB>
[S4-aaa] local-user python privilege level 3
[S4-aaa] local-user python service-type ssh
```

Testez d’abord une connexion SSH manuelle depuis PC2. En production, limitez `ssh server-source` à l’interface de gestion.

## 2 — Script Paramiko sûr et lisible

```python
import os
import time
import paramiko

host = "192.168.1.254"
username = "python"
password = os.environ["HUAWEI_LAB_PASSWORD"]

client = paramiko.SSHClient()
client.load_system_host_keys()
client.set_missing_host_key_policy(paramiko.RejectPolicy())

client.connect(host, username=username, password=password, timeout=10)
shell = client.invoke_shell()
time.sleep(1)

for command in ["screen-length 0 temporary", "display version", "display ip interface brief"]:
    shell.send(command + "\n")
    time.sleep(1)

output = shell.recv(65535).decode("utf-8", errors="replace")
print(output)
client.close()
```

Le guide utilise `AutoAddPolicy` pour simplifier le premier essai. La version TianSemi refuse une clé inconnue : récupérez et validez d’abord l’empreinte de S4 dans `known_hosts`. Le secret provient d’une variable d’environnement et ne doit pas être écrit dans le script ou Git.

## Vérifications attendues

1. Le ping PC2–S4 réussit.
2. La connexion SSH manuelle fonctionne.
3. Le script affiche l’invite et les sorties des deux commandes `display`.
4. `screen-length 0 temporary` évite une pagination bloquante.
5. La session est fermée même après la collecte.

Pour industrialiser, ajoutez `try/finally`, journalisation, timeout par commande et validation de l’invite plutôt que des délais fixes.

## Troubleshooting

- `AuthenticationException` : vérifier compte AAA, service-type SSH et secret injecté.
- `NoValidConnectionsError` : vérifier IP, reachability, STelnet et source serveur.
- `SSHException` sur la clé : valider l’empreinte et alimenter `known_hosts`.
- Sortie vide/incomplète : attendre l’invite, tester `recv_ready()` et lire en boucle.
- Script bloqué sur `---- More ----` : désactiver temporairement la pagination.

## Révision TianSemi

1. Pourquoi tester SSH manuellement avant le script ?
2. Quel rôle joue `invoke_shell()` ?
3. Pourquoi `AutoAddPolicy` est-il risqué ?
4. Comment éviter de stocker le mot de passe dans Git ?
5. Pourquoi les `sleep` fixes sont-ils fragiles ?
6. Comment envoyer une liste de commandes tout en vérifiant chaque résultat ?

## Références

- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 188 à 197 (pages 186 à 195 du guide).
