# Exemple d'une session

Cet exemple illustre l'expérience utilisateur une fois Airtable installé.

## 1. Ajouter une recette

**Utilisateur**

> J'ai trouvé cette recette de poulet aux champignons. Ajoute-la pour une prochaine semaine.

**Assistant**

> La recette n'existe pas encore. Je l'ajoute pour 4 portions avec ses ingrédients, sa préparation, son temps total et sa source. Je garde les quantités d'origine.

L'assistant crée alors une ligne dans `Recettes`.

## 2. Préparer le menu

**Utilisateur**

> Prépare ma semaine prochaine.

**Assistant**

> J'ai trouvé votre type de semaine habituel : 8 repas sont à prévoir. Voici une proposition :

| Jour | Moment | Proposition |
|---|---|---|
| Vendredi | Soir | Pizza maison |
| Samedi | Midi | Poulet aux champignons + riz |
| Samedi | Soir | Soupe + tartines |
| Dimanche | Midi | Lasagnes |
| Dimanche | Soir | Restes de lasagnes |
| Lundi | Soir | Poisson pané + légumes |
| Mardi | Soir | Boulettes tomate + semoule |
| Mercredi | Soir | Pâtes pesto |

> Je n'ai rien enregistré pour le moment.

## 3. Ajuster

**Utilisateur**

> Dimanche midi on mange dehors. Et mets quelque chose de rapide mardi.

**Assistant**

> D'accord. Je passe dimanche midi en `Extérieur` et je remplace mardi par une recette rapide. Le reste ne change pas.

## 4. Valider

**Utilisateur**

> OK, valide.

L'assistant vérifie d'abord qu'aucune semaine ni aucun repas identique n'existe déjà, puis enregistre ou met à jour les lignes dans `Semaines` et `Repas`.

## 5. Faire les courses

**Utilisateur**

> Fais les courses.

**Assistant**

> J'agrège uniquement les repas `À cuisiner`, en tenant compte des portions à préparer. Le repas `Extérieur` et les `Restes` ne génèrent pas de deuxième achat.

Exemple de sortie :

### Fruits & légumes
- 500 g de champignons
- 4 oignons
- 1 salade

### Boucherie
- 900 g de poulet
- 600 g de viande hachée

### Crèmerie
- 40 cl de crème liquide
- 1 mozzarella

### Épicerie
- 1 kg de riz à acheter pour environ 700 g utilisés
- 500 g de semoule

**Utilisateur**

> J'ai déjà le riz et les oignons.

L'assistant renvoie alors la liste sans ces deux produits.

## Ce que le système a mémorisé

Après cette session, Airtable contient :

- la nouvelle recette ajoutée ;
- la semaine réellement validée ;
- les repas et leurs effectifs ;
- l'information nécessaire pour que ces recettes apparaissent dans l'historique futur.

La liste de courses elle-même n'a pas besoin d'être conservée dans une table.