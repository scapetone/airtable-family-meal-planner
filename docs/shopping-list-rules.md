# Règles de calcul des courses

La liste de courses est calculée uniquement depuis les repas :

- de type `À cuisiner` ;
- non annulés ;
- liés à une recette.

Les repas de type `Restes` ne génèrent pas un second achat.

## Formule

Pour chaque ingrédient :

```text
quantité nécessaire =
quantité dans la recette
× (portions à préparer si renseignées, sinon nombre de personnes)
÷ nombre de portions de référence de la recette
```

## Agrégation

- additionner les ingrédients portant le même nom normalisé ;
- convertir seulement les unités simples compatibles (`kg/g`, `l/cl/ml`) ;
- ne pas convertir arbitrairement un poids en nombre de pièces ;
- signaler toute estimation ;
- arrondir après agrégation ;
- distinguer quantité utilisée et conditionnement à acheter ;
- retirer les produits que l'utilisateur indique déjà avoir ;
- exclure l'eau du robinet des achats ;
- organiser le résultat par catégorie.

## Exemple

Une recette prévue pour 4 contient 300 g de pâtes. Si 6 portions doivent être préparées :

```text
300 × 6 ÷ 4 = 450 g
```

La quantité nécessaire est donc 450 g. La quantité à acheter peut être 500 g si c'est le conditionnement pratique retenu.
