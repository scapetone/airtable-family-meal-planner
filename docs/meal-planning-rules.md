# Règles de planification des menus

## Préparer une semaine

1. Lire le contexte de la semaine, son type et les éventuelles exceptions.
2. Vérifier si des repas sont déjà enregistrés.
3. Copier les effectifs du type de semaine dans la nouvelle semaine.
4. Appliquer les exceptions sans modifier le modèle d'origine.
5. Consulter les recettes actives.
6. Tenir compte, si disponible, de la saison, de l'usage, du temps disponible et de l'historique récent.
7. Proposer un menu.
8. Ajuster le menu avec l'utilisateur.
9. N'enregistrer les repas qu'après validation explicite.
10. Relire les données enregistrées avant de marquer la semaine comme validée.

## Prévenir les doublons

Avant de créer :

- rechercher la semaine par sa date de début ;
- rechercher chaque repas par `Date + Moment`.

Si un repas existe déjà, le modifier plutôt que d'en créer un nouveau.

## Remplacer une recette

Un remplacement modifie le repas existant et son lien vers la recette. Il ne crée pas un deuxième repas pour le même créneau.

## Ajouter une recette

Lorsqu'une nouvelle recette est fournie :

1. vérifier qu'elle n'existe pas déjà ;
2. extraire titre, portions, ingrédients, préparation et temps quand ils sont disponibles ;
3. conserver le lien source réel, sans le deviner ;
4. conserver les quantités explicitement données ;
5. estimer uniquement les quantités vagues quand une estimation culinaire raisonnable est possible ;
6. signaler les estimations dans la note de l'ingrédient ;
7. utiliser des noms d'ingrédients cohérents pour permettre leur agrégation ;
8. valider le JSON avant l'enregistrement ;
9. rendre la recette active par défaut.
