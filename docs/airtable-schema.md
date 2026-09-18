# Schéma Airtable

Le modèle minimal utilise quatre tables principales. Dans le scénario de référence, l'assistant IA peut créer directement ces tables dans une base Airtable vide.

Les noms ci-dessous constituent le **schéma canonique de référence**. Une implémentation peut les traduire, mais l'assistant doit alors conserver une correspondance explicite.

## 1. Recettes

| Champ | Type conseillé | Rôle |
|---|---|---|
| Nom | Texte | Nom de la recette |
| Photo | Pièce jointe / URL | Illustration facultative |
| Préparation | Texte long | Étapes de préparation |
| Source | URL | Source originale si disponible |
| Saison | Sélection | Hiver, Printemps, Été, Automne, Toute l'année |
| Usage | Sélection | Quotidien, Rapide, Week-end, Festif |
| Recette pour | Nombre | Portions correspondant aux quantités stockées |
| Temps total | Nombre | Durée totale indicative en minutes |
| Actif | Case à cocher | Permet d'exclure temporairement une recette |
| Composition JSON | Texte long | Tableau JSON des ingrédients |
| Repas | Lien inverse | Repas utilisant cette recette |

Exemple de `Composition JSON` :

```json
[
  {
    "ingredient": "Carotte",
    "quantite": 3,
    "unite": "pièce",
    "categorie": "Fruits & légumes",
    "note": ""
  }
]
```

Unités recommandées : `g`, `kg`, `ml`, `cl`, `l`, `pièce`, `boîte`, `pot`, `cuillère à soupe`, `cuillère à café`.

Catégories recommandées : `Fruits & légumes`, `Boucherie`, `Poissonnerie`, `Crèmerie`, `Épicerie`, `Surgelés`, `Boulangerie`, `Autre`.

## 2. Types de semaine

| Champ | Type conseillé | Rôle |
|---|---|---|
| Nom | Texte | Nom du modèle |
| Effectifs habituels JSON | Texte long | Créneaux et effectifs habituels |
| Notes | Texte long | Particularités du modèle |
| Semaines | Lien inverse | Semaines utilisant ce modèle |

Format de référence :

```json
[
  { "day": 0, "moment": "Soir", "people": 4 },
  { "day": 1, "moment": "Midi", "people": 4 }
]
```

`day` est un décalage depuis le premier jour de la semaine culinaire : `0` = premier jour, `1` = lendemain, etc.

Les créneaux absents du JSON ne doivent pas être créés automatiquement.

## 3. Semaines

| Champ | Type conseillé | Rôle |
|---|---|---|
| Date de début | Date | Premier jour de la semaine culinaire |
| Type de semaine | Lien vers Types de semaine | Modèle utilisé |
| Contexte | Sélection | Ex. École, Vacances, Autre |
| Statut | Sélection | À préparer, Validée |
| Notes | Texte long | Exceptions et contexte |
| Repas | Lien inverse | Repas de la semaine |

Une semaine ne doit utiliser qu'un seul type de semaine.

## 4. Repas

| Champ | Type conseillé | Rôle |
|---|---|---|
| Créneau | Texte | Ex. `2026-09-18 — Soir` |
| Date | Date | Date réelle du repas |
| Moment | Sélection | Midi ou Soir |
| Semaine | Lien vers Semaines | Une seule semaine |
| Nombre de personnes | Nombre | Effectif réel |
| Portions à préparer | Nombre | Facultatif : permet de cuisiner du surplus |
| Type de repas | Sélection | À cuisiner, Restes, Extérieur, Autre |
| Recette | Lien vers Recettes | Une seule recette |
| Annulé | Case à cocher | Ignore le repas |
| Notes | Texte long | Ajustements éventuels |

Avant de créer un repas, rechercher une ligne existante avec la même `Date + Moment`.

## Historique : version minimale

Aucun compteur n'est nécessaire dans `Recettes`.

Pour connaître la dernière consommation ou le nombre de consommations, l'assistant peut lire directement les repas passés liés à une recette, en tenant compte de la date, du type de repas (`À cuisiner` ou `Restes`) et du champ `Annulé`.

Cela évite de dupliquer une information calculable.

## Champs d'historique optionnels

Une implémentation peut ajouter des champs calculés comme `Consommation présumée` ou `Date de consommation présumée` si cela améliore les recherches dans Airtable.

Ils ne font pas partie du schéma minimal et ne sont pas nécessaires au fonctionnement du projet.

## Création par l'assistant

Lors du bootstrap d'une base vide :

1. inspecter la base existante avant toute création ;
2. créer uniquement les quatre tables décrites ici ;
3. créer d'abord les champs simples, puis les liens entre tables ;
4. vérifier les types et options de sélection ;
5. relire le schéma créé ;
6. signaler toute limitation du connecteur Airtable au lieu d'inventer un champ de remplacement ;
7. ne créer ni vues, ni automatisations, ni tables supplémentaires sans demande explicite.