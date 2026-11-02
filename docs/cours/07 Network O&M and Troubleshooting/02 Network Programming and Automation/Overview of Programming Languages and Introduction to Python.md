---
id: HUA-HCIA-DC-078
title: "Overview of Programming Languages and Introduction to Python"
official_title: "Overview of Programming Languages and Introduction to Python"
course_unit: "7.2.2"
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
source_pages: "PDF 989-1002"
tags: [huawei, hcia, datacom, python, programming-language, bytecode, indentation, function, class]
prerequisites:
  - "[[Introduction to Network Programming and Automation]]"
related:
  - "[[Introduction to Paramiko APIs]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Langages de programmation et introduction à Python

## Du code source à l’exécution

Un langage de programmation décrit le comportement attendu d’un ordinateur. Les langages peuvent être classés par niveau d’abstraction — machine, assembleur, haut niveau — et par mode d’exécution.

![Exécution Python et éléments de structure d’un programme](<../../../assets/diagrammes/07 Network O&M and Troubleshooting/python-execution-structure-fr.svg>)

### Langage compilé

Le compilateur traduit le code source en instructions machine, puis l’éditeur de liens assemble les bibliothèques pour produire un exécutable. Cet exécutable dépend généralement de la plateforme et de l’architecture cible. C, C++ et Go sont cités comme exemples.

### Langage interprété

Le programme est traduit vers une forme intermédiaire puis exécuté par une machine virtuelle. Le support présente Java et Python dans cette catégorie. Pour Python :

1. l’interpréteur lit le fichier `.py` ;
2. il produit du bytecode, éventuellement enregistré en `.pyc` ;
3. la Python Virtual Machine exécute ce bytecode.

Cette couche favorise la portabilité, au prix d’une exécution souvent moins rapide qu’un binaire natif optimisé.

## Pourquoi Python

Python est un langage open source, de haut niveau et à typage dynamique. Ses points forts sont :

- syntaxe lisible ;
- styles procédural et orienté objet ;
- vaste bibliothèque standard et écosystème tiers ;
- interopérabilité avec d’autres langages ;
- portabilité entre systèmes.

Ces qualités expliquent son usage en automatisation O&M, traitement de données et intelligence artificielle.

## Mode interactif et mode script

- Le **mode interactif** exécute immédiatement une instruction et convient aux essais courts.
- Le **mode script** conserve le programme dans un fichier `.py`, le rend réexécutable, testable et versionnable.

```text
python
>>> print("hello world")
hello world
```

```text
python demo.py
```

Une automatisation réseau durable doit être écrite en script, avec paramètres, gestion des erreurs et journalisation.

## Conventions essentielles

### Identifiants

Un nom peut contenir lettres, chiffres et `_`, mais ne peut pas commencer par un chiffre. Les identifiants sont sensibles à la casse : `User_ID` et `user_id` sont différents.

### Indentation

L’indentation définit les blocs. Huawei recommande quatre espaces par niveau. Une incohérence entraîne `IndentationError`.

```python
if device_reachable:
    print("Équipement joignable")
else:
    print("Équipement indisponible")
```

### Lisibilité

- une instruction par ligne ;
- pas de point-virgule inutile ;
- lignes vides entre blocs logiques ;
- espaces cohérents ;
- commentaires `#` pour expliquer le pourquoi ;
- docstring pour décrire module, fonction ou classe.

## Fonctions, modules, classes et méthodes

| Élément | Définition | Exemple d’usage réseau |
| --- | --- | --- |
| Fonction | Bloc réutilisable défini avec `def` | Tester la joignabilité d’une cible. |
| Module | Fichier Python importable | Regrouper les fonctions SSH. |
| Classe | Modèle d’objets et de comportements | Représenter un équipement. |
| Méthode | Fonction définie dans une classe | `device.backup_config()`. |

Le paramètre `self` représente l’instance dans une méthode. Un module importé exécute ses instructions de niveau supérieur ; protégez le point d’entrée avec `if __name__ == "__main__":` lorsque nécessaire.

## Pratique TianSemi : inventaire minimal

Écrivez un script qui :

1. stocke trois équipements sous forme de dictionnaires ;
2. définit une fonction `validate_device()` contrôlant nom, adresse et rôle ;
3. parcourt l’inventaire avec une boucle ;
4. affiche uniquement les entrées valides ;
5. place l’exécution dans une fonction `main()` ;
6. ajoute une docstring et des noms explicites ;
7. provoque volontairement une erreur d’indentation puis corrigez-la.

## À retenir pour l’examen HCIA-Datacom

- Un langage compilé produit un exécutable ; un langage interprété s’appuie sur une machine virtuelle à l’exécution.
- Python traduit le code en bytecode exécuté par la PVM.
- Python est de haut niveau, open source, dynamique, lisible et extensible.
- L’indentation fait partie de la syntaxe ; quatre espaces sont recommandés.
- Un identifiant ne commence pas par un chiffre et reste sensible à la casse.
- Une fonction est réutilisable ; un module est un fichier importable ; une méthode est une fonction de classe.

## Révision TianSemi

1. Quelle différence existe entre langage compilé et interprété ?
2. Quel rôle remplissent bytecode et PVM ?
3. Quels avantages et limite principale de Python le support présente-t-il ?
4. Quand utiliser le mode interactif ou le mode script ?
5. Quelles règles s’appliquent aux identifiants ?
6. Pourquoi l’indentation est-elle obligatoire en Python ?
7. Quelle différence existe entre fonction, module, classe et méthode ?
8. Pourquoi éviter du code actif non protégé lors de l’import d’un module ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 989 à 1002.
