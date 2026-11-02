---
id: HUA-HCIA-DC-038
title: "Typical OSPF Configuration"
official_title: "Typical OSPF Configuration"
course_unit: "3.3.3"
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
source_pages: "PDF 484-494"
source_lab:
  - "HCIA-Datacom V2.0 Lab Guide.pdf, pages PDF 108-124"
tags: [huawei, hcia, datacom, ospf-configuration, wildcard, ospf-cost, authentication, default-route]
prerequisites:
  - "[[Working Principles of OSPF]]"
lab_required: true
publication_status: "not-started"
site_url:
---

# Configuration OSPF typique (Typical OSPF Configuration)

## Planifier avant de configurer

Définissez pour chaque routeur : Router ID unique, process ID local, aire de chaque interface, préfixes à annoncer, coûts éventuels et méthode d'authentification.

![Topologie OSPF typique avec aire 0, aire 1 et routeur ABR](<../../../assets/diagrammes/03 IP Technology Basics/ospf-typical-config-fr.svg>)

Le process ID possède une portée locale : deux voisins peuvent utiliser des numéros de processus différents. En revanche, les interfaces d'un même lien doivent appartenir à la même aire et partager des paramètres compatibles.

## Configuration de base

```text
[R1] ospf 1 router-id 1.1.1.1
[R1-ospf-1] area 0
[R1-ospf-1-area-0.0.0.0] network 1.1.1.1 0.0.0.0
[R1-ospf-1-area-0.0.0.0] network 10.1.12.0 0.0.0.3
```

La commande `network` n'ajoute pas directement une route à annoncer : elle sélectionne les interfaces dont l'adresse principale correspond au couple adresse/wildcard et les active dans l'aire.

- wildcard `0.0.0.0` : correspond à une adresse d'interface précise ;
- wildcard `0.0.0.3` : correspond aux adresses d'un /30 ;
- wildcard `0.0.0.255` : correspond aux adresses d'un /24.

## Exemple multi-aire

R2 relie l'aire 0 à l'aire 1 :

```text
[R2] ospf 1 router-id 2.2.2.2
[R2-ospf-1] area 0
[R2-ospf-1-area-0.0.0.0] network 10.1.12.0 0.0.0.3
[R2-ospf-1-area-0.0.0.0] quit
[R2-ospf-1] area 1
[R2-ospf-1-area-0.0.0.1] network 10.1.23.0 0.0.0.3
```

R2 devient ABR parce qu'il possède des interfaces OSPF dans l'aire 0 et dans l'aire 1.

## Coût et bande passante de référence

```text
[R1-GE0/0/0] ospf cost 10
[R1-ospf-1] bandwidth-reference 10000
```

`ospf cost` fixe explicitement le coût d'une interface entre 1 et 65535. `bandwidth-reference` change la référence du processus, exprimée en Mbit/s. Pour des calculs cohérents, utilisez la même référence sur tous les routeurs du domaine.

## Priorité DR

```text
[R1-GE0/0/0] ospf dr-priority 100
```

La valeur va de 0 à 255. Une valeur élevée augmente les chances d'élection ; 0 rend l'interface inéligible. Une modification ne préempte pas une élection déjà achevée.

## Authentification du lab officiel

Le lab emploie HMAC-SHA256. Les paramètres doivent correspondre sur un même lien.

```text
[R1-GE0/0/1] ospf authentication-mode hmac-sha256 1 cipher <secret>
```

L'authentification peut aussi être définie au niveau de l'aire. Une incohérence fait tomber le voisinage ; vérifiez immédiatement `display ospf peer brief`. Ne documentez jamais un secret réel dans une note ou un dépôt.

## Annoncer une route par défaut

```text
[R1-ospf-1] default-route-advertise always
```

Sans `always`, la commande annonce normalement une route par défaut seulement si une route par défaut active appropriée existe localement. Avec `always`, l'annonce est forcée : utilisez-la uniquement si R1 fournit réellement une sortie fiable.

## Vérification ordonnée

```text
<R2> display ospf peer brief
<R2> display ospf lsdb
<R2> display ospf routing
<R2> display ip routing-table protocol ospf
<R1> ping -a 1.1.1.1 3.3.3.3
```

1. Interfaces IP Up/Up.
2. Voisins dans l'aire attendue et état stable.
3. LSDB alimentée.
4. Routes OSPF calculées puis actives dans la table IP.
5. Ping sourcé pour valider aussi le retour.

## Mini-lab TianSemi

1. Construisez un triangle R1–S1–S2 avec une loopback /32 par équipement.
2. Configurez OSPF aire 0 et des Router ID égaux aux loopbacks.
3. Vérifiez les trois voisinages et les routes apprises.
4. Activez HMAC-SHA256 sur un seul côté : observez la perte du voisin, puis corrigez l'autre côté.
5. Faites annoncer une route par défaut depuis R1 et vérifiez-la sur les deux autres équipements.
6. Modifiez le coût d'une interface pour forcer le trajet alternatif.
7. Validez le chemin par `tracert` et remettez le coût initial.

## Troubleshooting

| Symptôme | Contrôle prioritaire |
| --- | --- |
| Aucun processus utile | `ospf 1` et interfaces sélectionnées par `network` |
| Voisin absent | Aire, wildcard, type réseau, timers et authentification |
| Router ID inattendu | Configuration explicite et redémarrage du processus si nécessaire |
| Route OSPF inactive | Route directe/statique de meilleure préférence |
| Mauvais chemin | Somme des coûts des interfaces de sortie |
| Route par défaut absente | Existence locale de la défaut ou usage de `always` |
| Une aire isolée | ABR connecté à l'aire 0 et affectation correcte des liens |

## À retenir pour l'examen HCIA-Datacom

- `ospf`, `area` et `network` constituent la configuration minimale.
- La wildcard sélectionne les interfaces, elle ne décrit pas simplement un préfixe à publier.
- Le Router ID doit être unique et stable.
- Le coût se configure par interface ; la référence se configure dans le processus.
- Les voisins doivent atteindre Full lorsque l'adjacence est attendue.
- Une route OSPF calculée n'est pas forcément active dans la table IP.
- L'authentification doit correspondre sur les deux extrémités.

## Révision TianSemi

1. Quelle différence existe entre process ID et Router ID ?
2. Comment la commande `network` détermine-t-elle les interfaces OSPF ?
3. Quelle wildcard correspond à une adresse précise, un /30 et un /24 ?
4. Pourquoi la référence de bande passante doit-elle être cohérente dans le domaine ?
5. Quel effet produit une priorité DR égale à 0 ?
6. Pourquoi une authentification configurée sur un seul côté coupe-t-elle le voisinage ?
7. Que change le paramètre `always` de `default-route-advertise` ?
8. Dans quel ordre vérifier OSPF après configuration ?

## Références

- Huawei, *HCIA-Datacom V2.0 Training Material*, pages PDF 484 à 494.
- Huawei, *HCIA-Datacom V2.0 Lab Guide*, pages PDF 108 à 124 pour OSPF en aire unique, l'authentification, la route par défaut et le réglage du coût.
