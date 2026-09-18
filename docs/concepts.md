# Concepts

## Recette

Une recette est un plat réutilisable. Elle contient notamment un nom, une préparation, un nombre de portions de référence et une composition structurée en JSON.

Les quantités enregistrées restent celles de la recette de référence. Elles sont adaptées seulement au moment de planifier les courses.

## Type de semaine

Un type de semaine représente un rythme habituel : quels repas doivent être prévus et pour combien de personnes.

Il s'agit d'un modèle. Modifier ce modèle ne doit jamais modifier rétroactivement les semaines déjà créées.

## Semaine

Une semaine représente une période réellement planifiée. Elle possède une date de début, un type de semaine facultatif, un contexte et un statut.

Le premier jour de la semaine culinaire peut être différent du lundi.

## Repas

Un repas représente un créneau précis : une date + `Midi` ou `Soir`.

Il peut être :

- `À cuisiner` ;
- `Restes` ;
- `Extérieur` ;
- `Autre`.

Un repas peut être lié à une recette et contient l'effectif réel ainsi que, si nécessaire, le nombre de portions à préparer.

## Historique

L'historique est reconstruit depuis les repas passés plutôt que dupliqué dans la table des recettes. Il sert notamment à connaître la dernière consommation d'une recette et à éviter une répétition excessive.
