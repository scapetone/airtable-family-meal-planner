# Démarrage de bout en bout

Ce guide décrit le scénario de référence : **ChatGPT + Airtable connecté + ce dépôt GitHub**.

Il n'y a aucun programme à installer localement.

## 1. Créer une base Airtable vide

Dans Airtable, créez une nouvelle base. Son nom est libre.

Vous pouvez laisser la base vide : l'objectif est que l'assistant construise lui-même le modèle.

## 2. Donner accès à Airtable à ChatGPT

Connectez Airtable à ChatGPT et autorisez l'accès à la base que vous venez de créer.

L'accès doit permettre à l'assistant de lire la structure de la base, créer les tables et champs, puis lire et modifier les enregistrements.

Si votre connexion ne permet que de lire les données, le bootstrap automatique ne pourra pas créer le schéma.

## 3. Donner ce dépôt à l'assistant

Partagez l'URL GitHub du projet ou rendez les fichiers du dépôt accessibles dans la conversation/projet.

L'assistant a surtout besoin de :

- `README.md` ;
- `docs/airtable-schema.md` ;
- `config.example.json` ;
- `airtable/week-types.example.json` ;
- `prompts/bootstrap-airtable.md` ;
- `prompts/assistant-instructions.md`.

## 4. Construire Airtable automatiquement

Utilisez le prompt de `prompts/bootstrap-airtable.md`.

L'assistant doit d'abord inspecter la base, puis créer les quatre tables et vérifier le résultat.

À ce stade, la base est structurée mais ne contient encore ni faux repas ni faux historique.

## 5. Définir votre rythme réel

L'assistant vous demandera les informations qui changent réellement le modèle, par exemple :

- quel jour commence votre semaine culinaire ;
- quels midis et soirs doivent être planifiés ;
- combien de personnes mangent habituellement à chaque créneau ;
- s'il existe plusieurs rythmes récurrents.

Un foyer peut par exemple avoir un type `Avec enfants` et un type `Sans enfants`, ou `Semaine école` et `Vacances`.

Les types de semaine sont des modèles. Lorsqu'une semaine réelle est créée, leurs effectifs sont copiés dans les repas. Modifier ensuite le modèle ne doit jamais modifier rétroactivement les anciennes semaines.

## 6. Ajouter les premières recettes

Vous pouvez commencer avec très peu de recettes.

Donnez simplement une recette à l'assistant et demandez :

> Ajoute cette recette dans Airtable pour une prochaine fois.

L'assistant doit vérifier les doublons, conserver la source réelle, enregistrer le nombre de portions, structurer les ingrédients, la préparation et le temps total lorsqu'il est connu.

Les ingrédients restent dans `Composition JSON`, ce qui évite une table Airtable supplémentaire.

## 7. Préparer une semaine

Demandez par exemple :

> Prépare la semaine prochaine avec mon type de semaine habituel. Consulte les recettes actives et l'historique récent. Ne l'enregistre pas avant que je valide.

L'assistant propose le menu en conversation.

Vous pouvez ensuite modifier librement certaines propositions.

Ce n'est qu'après un message explicite tel que :

> OK, valide.

que l'assistant crée ou met à jour la semaine et ses repas dans Airtable.

## 8. Générer les courses

Une fois le menu validé :

> Fais-moi la liste de courses pour cette semaine.

L'assistant adapte les quantités aux portions réellement préparées, regroupe les ingrédients communs et organise la liste par catégorie.

Vous pouvez ensuite répondre :

> J'ai déjà les pâtes, les œufs et les oignons.

pour les retirer de la liste.

## 9. Continuer à enrichir le système

Au fil des semaines :

- de nouvelles recettes rejoignent le catalogue ;
- l'historique des repas aide à varier les propositions ;
- les types de semaine évitent de redécrire le rythme du foyer à chaque fois ;
- les restes peuvent être planifiés volontairement.

Le système devient donc plus utile sans nécessiter de maintenance complexe.

## Utiliser un autre assistant

Le modèle n'impose pas ChatGPT au niveau des données.

Un autre assistant peut être utilisé s'il peut :

1. lire la documentation de ce dépôt ;
2. inspecter et modifier le schéma Airtable ;
3. lire et écrire les enregistrements ;
4. respecter les règles décrites dans `prompts/assistant-instructions.md`.

La manière exacte de connecter Airtable dépend alors de l'assistant et de son environnement.