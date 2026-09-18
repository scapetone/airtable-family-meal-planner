# Family Meal Planner — Airtable + assistant IA

Un système pour **préparer les menus de la semaine, enrichir progressivement un catalogue de recettes et générer les courses**, avec Airtable comme mémoire et un assistant IA comme interface.

Le scénario de référence est volontairement simple :

**une base Airtable vide + ChatGPT connecté à Airtable + ce dépôt GitHub.**

Vous créez une base Airtable vide, vous donnez à ChatGPT l'accès à cette base, puis vous lui partagez ce dépôt. ChatGPT lit le schéma documenté ici et peut construire les tables, ajouter les premiers modèles de semaine, puis gérer les menus au fil des conversations.

> Ce dépôt ne contient aucune donnée familiale réelle. Les effectifs, recettes et rythmes fournis sont uniquement des exemples.

## Ce que ce projet est — et n'est pas

Ce dépôt est avant tout **une spécification réutilisable** : un modèle Airtable, des règles métier et des instructions pour un assistant IA.

Il ne contient pas une application web ni un programme à lancer.

Dans l'usage de référence, **ChatGPT est l'application** : il discute avec l'utilisateur et lit/écrit dans Airtable via une connexion Airtable compatible.

Le projet n'est pas fondamentalement lié à ChatGPT. Un autre assistant peut reprendre le même modèle s'il est capable de lire cette documentation et d'accéder à Airtable via un connecteur, MCP ou une intégration équivalente.

## L'expérience utilisateur

Une fois le système installé, l'usage ressemble à ceci :

> « Prépare le menu de la semaine prochaine. »

L'assistant consulte les types de semaine, les recettes actives et l'historique récent. Il propose les repas correspondant réellement aux créneaux du foyer.

Vous pouvez répondre :

> « Remplace le poisson du samedi. Dimanche midi on mange dehors. Et lundi fais quelque chose de rapide. »

L'assistant ajuste le menu. Rien n'est enregistré tant que vous ne validez pas.

> « OK, valide. »

Les repas sont enregistrés dans Airtable.

Vous pouvez ensuite demander :

> « Fais-moi les courses. »

Les ingrédients sont recalculés selon les portions à préparer, regroupés et classés par catégorie.

Et lorsque vous trouvez une nouvelle recette :

> « Ajoute cette recette pour une prochaine fois. »

L'assistant extrait les portions, ingrédients et instructions, vérifie les doublons, puis enrichit la table `Recettes`. Cette nouvelle recette pourra être proposée les semaines suivantes.

**Plus le système est utilisé, plus sa mémoire culinaire devient utile.**

## Le modèle en quatre tables

Le système de référence utilise seulement quatre tables Airtable :

- **Recettes** — le catalogue de plats et leurs ingrédients ;
- **Types de semaine** — les rythmes habituels du foyer ;
- **Semaines** — les semaines réellement planifiées ;
- **Repas** — chaque créneau concret, avec son effectif et sa recette éventuelle.

Cette structure suffit pour construire les menus, gérer les restes, retrouver l'historique et calculer les courses.

Le schéma complet est décrit dans [`docs/airtable-schema.md`](docs/airtable-schema.md).

## Installation recommandée avec ChatGPT

### 1. Créer une base Airtable vide

Créez simplement une nouvelle base Airtable. Vous n'avez pas besoin de créer les tables vous-même.

### 2. Connecter Airtable à ChatGPT

Donnez à ChatGPT un accès en lecture et écriture à cette base via votre connexion Airtable.

L'assistant doit pouvoir créer les tables et champs, puis lire et modifier leurs enregistrements.

### 3. Partager ce dépôt à ChatGPT

Donnez à ChatGPT l'URL de ce dépôt ou rendez ses fichiers accessibles dans votre projet/conversation.

Demandez-lui ensuite de lire en priorité :

- [`docs/airtable-schema.md`](docs/airtable-schema.md) ;
- [`config.example.json`](config.example.json) ;
- [`airtable/week-types.example.json`](airtable/week-types.example.json) ;
- [`prompts/bootstrap-airtable.md`](prompts/bootstrap-airtable.md).

### 4. Lancer le bootstrap

Vous pouvez utiliser directement le prompt fourni dans [`prompts/bootstrap-airtable.md`](prompts/bootstrap-airtable.md).

En résumé, vous demandez à l'assistant :

> Lis la documentation de ce dépôt. Inspecte ma base Airtable vide, construis les quatre tables et leurs champs conformément au schéma, puis vérifie la structure créée. Ne crée pas d'autres tables ou automatisations.

L'assistant construit alors la base au lieu de vous demander de reproduire le schéma manuellement.

### 5. Personnaliser le foyer

Adaptez le premier jour de votre semaine culinaire, vos types de semaine, les créneaux nécessaires, les effectifs habituels et éventuellement la durée d'historique souhaitée.

Les exemples fournis ne sont pas des règles imposées.

### 6. Ajouter quelques recettes

Vous pouvez saisir quelques recettes manuellement, importer vos recettes existantes, ou simplement les donner à l'assistant une par une. Vous n'avez pas besoin de remplir un catalogue complet avant de commencer.

### 7. Préparer la première semaine

> Prépare ma prochaine semaine. Consulte les recettes disponibles et évite si possible celles mangées récemment. Ne l'enregistre pas avant que je valide.

Un parcours détaillé est disponible dans [`docs/getting-started.md`](docs/getting-started.md).

## Comment fonctionne une semaine ?

Un `Type de semaine` décrit seulement les repas qui doivent réellement être prévus. `day: 0` représente le premier jour de la semaine culinaire configurée : le système n'impose donc pas le lundi.

Quand une vraie semaine est créée, les effectifs du modèle sont copiés dans les repas. Les exceptions de cette semaine peuvent ensuite être appliquées sans modifier le modèle original.

## Les recettes enrichissent la base au fil du temps

Lorsqu'une recette est fournie à l'assistant, celui-ci peut vérifier les doublons, conserver sa source réelle, enregistrer ses portions, structurer ses ingrédients, conserver les quantités d'origine, enregistrer sa préparation et son temps total, signaler les estimations, proposer une saison et un usage, puis l'activer pour de futurs menus.

Le but est que **l'ajout d'une recette soit presque aussi simple que de partager la recette dans la conversation**.

## Restes et portions à préparer

Le système distingue **Nombre de personnes** et **Portions à préparer**. On peut donc cuisiner 6 portions pour 4 personnes afin de prévoir 2 portions pour plus tard. Un repas de type `Restes` ne génère pas un deuxième achat.

## Liste de courses

Pour chaque repas à cuisiner :

```text
quantité nécessaire =
quantité de la recette
× (portions à préparer si renseignées, sinon nombre de personnes)
÷ nombre de portions de référence de la recette
```

Les ingrédients communs sont regroupés, les unités simples compatibles sont converties et les produits déjà présents peuvent être retirés.

## Pensé pour rester léger sur Airtable

Le projet évite volontairement de multiplier les tables et les lignes : quatre tables principales, pas de table permanente pour les courses, pas d'automatisation obligatoire et des ingrédients stockés en JSON.

La configuration de référence propose **six mois d'historique actif** et un avertissement à **850 lignes**. Ce sont des choix prudents du projet, pas une description des limites commerciales actuelles d'Airtable.

Aucune purge ne doit être automatique : export, vérification, confirmation, puis suppression.

## Configuration

Les paramètres génériques sont illustrés dans [`config.example.json`](config.example.json). Ils servent de référence à l'assistant ; aucun programme de ce dépôt ne les exécute automatiquement.

## Documentation

- [`docs/getting-started.md`](docs/getting-started.md) — installation de bout en bout ;
- [`docs/airtable-schema.md`](docs/airtable-schema.md) — structure précise des tables ;
- [`docs/concepts.md`](docs/concepts.md) — concepts métier ;
- [`docs/meal-planning-rules.md`](docs/meal-planning-rules.md) — règles de planification ;
- [`docs/shopping-list-rules.md`](docs/shopping-list-rules.md) — calcul des courses ;
- [`examples/example-session.md`](examples/example-session.md) — exemple d'utilisation ;
- [`prompts/bootstrap-airtable.md`](prompts/bootstrap-airtable.md) — prompt pour construire une base vide ;
- [`prompts/assistant-instructions.md`](prompts/assistant-instructions.md) — règles permanentes pour l'assistant.

## Principes du projet

- L'utilisateur garde le dernier mot avant l'enregistrement d'un menu.
- Une recette ne doit pas être dupliquée si elle existe déjà.
- Les anciennes semaines ne changent pas quand un type de semaine est modifié.
- Les quantités restent rattachées aux portions de la recette d'origine.
- Les restes ne génèrent pas un deuxième achat.
- Les estimations sont explicitement signalées.
- Une purge d'historique n'est jamais automatique.
- Les données privées d'un foyer ne doivent pas être publiées dans ce dépôt.

## Licence

MIT. Vous pouvez reprendre, adapter et redistribuer le modèle.