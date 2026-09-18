# Schéma Airtable

Le modèle utilise quatre tables principales.

## 1. Recettes

Champs recommandés :

| Champ | Type conseillé | Rôle |
|---|---|---|
| Nom | Texte | Nom de la recette |
| Photo | Pièce jointe / URL | Illustration facultative |
| Préparation | Texte long | Étapes de préparation |
| Source | URL | Source originale si disponible |
| Saison | Sélection | Hiver, Printemps, Été, Automne, Toute l'année |
| Usage | Sélection | Quotidien, Rapide, Week-end, Festif |
| Recette pour | Nombre | Nombre de portions correspondant aux quantités stockées |
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

Le champ JSON peut utiliser un décalage en jours depuis le premier jour de la semaine :

```json
[
  { "day": 0, "moment": "Soir", "people": 4 },
  { "day": 1, "moment": "Midi", "people": 4 }
]
```

Les créneaux non présents ne doivent pas être créés automatiquement.

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
| Semaine | Lien vers Semaines | Semaine concernée |
| Nombre de personnes | Nombre | Effectif réel |
| Portions à préparer | Nombre | Facultatif : permet de cuisiner du surplus |
| Type de repas | Sélection | À cuisiner, Restes, Extérieur, Autre |
| Recette | Lien vers Recettes | Une seule recette |
| Annulé | Case à cocher | Ignore le repas |
| Notes | Texte long | Ajustements éventuels |
| Consommation présumée | Formule / case | Facultatif, pour l'historique |
| Date de consommation présumée | Formule / date | Facultatif, pour l'historique |

Avant de créer un repas, chercher une ligne existante avec la même `Date + Moment`. Cela évite les doublons.
