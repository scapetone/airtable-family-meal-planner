# Family Meal Planner — Airtable + assistant IA

Un système simple pour **préparer les menus de la semaine, capitaliser ses recettes et générer les courses**, sans construire une application complète.

L'idée est de faire d'Airtable la mémoire du foyer : les recettes, les semaines et les repas y sont stockés dans une structure claire. Un assistant IA peut ensuite lire cette base, proposer un menu cohérent, y ajouter de nouvelles recettes au fil du temps et calculer les quantités nécessaires pour les courses.

Le projet est volontairement pensé pour rester léger : **Airtable peut être utilisé comme base sur son offre gratuite**, sans automatisations complexes ni multiplication des tables. La configuration de référence conserve un historique limité et prévoit une purge manuelle et sécurisée quand la base grossit.

> Ce dépôt ne contient aucune donnée familiale réelle. Les effectifs, rythmes de semaine, recettes et paramètres fournis sont des exemples à adapter.

## Le concept en une minute

Au lieu de recommencer chaque semaine avec une liste de recettes dans un document ou dans sa tête, on construit progressivement une petite base de connaissances culinaire.

1. **Les recettes vivent dans Airtable.** Chaque recette contient ses portions, sa préparation et une liste d'ingrédients structurée.
2. **Les types de semaine décrivent le rythme du foyer.** Par exemple : semaine classique, garde alternée, vacances, semaine très chargée, etc.
3. **Une semaine réelle est créée à partir d'un type de semaine.** Les repas à prévoir et le nombre de personnes sont connus à l'avance, puis ajustés si nécessaire.
4. **L'assistant propose le menu.** Il peut tenir compte des recettes actives, de la saison, du temps disponible et des repas récents.
5. **Le menu validé est enregistré dans Airtable.** L'historique devient alors utile pour éviter de tourner en rond.
6. **Les courses sont calculées à partir des recettes prévues.** Les quantités sont adaptées au nombre de portions à préparer et regroupées par ingrédient.
7. **La base s'enrichit naturellement.** Quand vous partagez une nouvelle recette, l'assistant peut l'analyser, la structurer et l'ajouter à Airtable pour les prochaines semaines.

En pratique, plus vous utilisez le système, plus il devient pertinent : votre catalogue de recettes grandit et l'historique aide à varier les menus.

## Pourquoi Airtable ?

Airtable joue ici le rôle d'une petite base de données compréhensible par tout le monde. On peut consulter et modifier les recettes ou les menus directement dans une interface proche d'un tableur, tout en conservant des liens entre les données.

Le système de référence utilise seulement **quatre tables** :

- **Recettes** : ce que vous savez cuisiner.
- **Types de semaine** : les rythmes habituels du foyer.
- **Semaines** : les semaines réellement planifiées.
- **Repas** : chaque créneau de repas prévu, avec son nombre de personnes et éventuellement sa recette.

Cela suffit pour relier une recette à un repas, un repas à une semaine, calculer les courses et reconstituer l'historique.

## Une base de recettes qui s'enrichit avec le temps

La table `Recettes` n'est pas un catalogue figé à remplir intégralement avant de commencer.

Vous pouvez démarrer avec seulement quelques recettes familières. Ensuite, à chaque fois que vous découvrez une recette intéressante — site web, livre, recette familiale ou recette improvisée — vous pouvez demander à l'assistant de l'ajouter.

L'assistant peut notamment :

- vérifier si la recette existe déjà ;
- extraire le nombre de portions ;
- transformer les ingrédients en données structurées ;
- conserver les quantités de la recette originale ;
- estimer prudemment une quantité quand la source reste vague ;
- classer les ingrédients par rayon pour les courses ;
- enregistrer la préparation ;
- proposer une saison et un usage, par exemple `Rapide`, `Quotidien` ou `Week-end`.

Le but est que **l'ajout d'une recette soit presque aussi simple que de la partager dans une conversation**.

## Comment naît un menu de la semaine ?

Un `Type de semaine` contient les créneaux qui nécessitent réellement un repas et leurs effectifs habituels.

Par exemple :

```json
{
  "name": "Semaine classique",
  "meals": [
    { "day": 0, "moment": "Soir", "people": 4 },
    { "day": 1, "moment": "Midi", "people": 4 },
    { "day": 1, "moment": "Soir", "people": 4 },
    { "day": 2, "moment": "Midi", "people": 4 }
  ]
}
```

`day: 0` représente le premier jour de la semaine configurée. Le système n'impose donc pas que votre semaine culinaire commence le lundi.

Quand vous préparez une nouvelle semaine, l'assistant peut :

- partir du type de semaine correspondant ;
- appliquer les exceptions de cette semaine ;
- consulter les recettes disponibles ;
- regarder ce qui a été mangé récemment ;
- proposer un menu varié ;
- modifier les propositions avec vous ;
- enregistrer le résultat **uniquement après validation**.

Cette dernière règle est importante : l'IA propose, mais la semaine réellement enregistrée reste sous votre contrôle.

## Et les restes ?

Le modèle distingue le nombre de personnes présentes du nombre de portions réellement cuisinées.

Vous pouvez par exemple cuisiner 6 portions pour 4 personnes et prévoir les 2 portions restantes pour un autre repas. Le repas `Restes` n'achète pas une deuxième fois les ingrédients.

C'est ce qui permet de planifier volontairement du surplus au lieu de traiter les restes comme un accident.

## Générer la liste de courses

Pour chaque repas à cuisiner, la quantité nécessaire est calculée ainsi :

```text
quantité de la recette
× portions à préparer
÷ nombre de portions de la recette d'origine
```

Les ingrédients identiques sont ensuite regroupés. Les unités simples peuvent être converties (`kg` / `g`, `l` / `cl` / `ml`) et la liste est organisée par catégories : fruits et légumes, boucherie, crèmerie, épicerie, etc.

Le système distingue autant que possible :

- la quantité réellement nécessaire pour cuisiner ;
- la quantité pratique à acheter au magasin.

On peut ensuite retirer ce qui est déjà présent dans les placards.

## Pensé pour Airtable gratuit

Le projet évite volontairement une architecture qui consommerait beaucoup de lignes ou nécessiterait des fonctions avancées d'Airtable.

Quelques principes :

- seulement quatre tables principales ;
- pas de table supplémentaire pour chaque liste de courses ;
- pas d'automatisation Airtable obligatoire ;
- pas de duplication des ingrédients dans une table dédiée ;
- les compositions de recettes et certains modèles sont stockés en JSON dans des champs texte ;
- l'historique ancien peut être exporté puis purgé manuellement ;
- un seuil d'alerte configurable peut signaler que la base commence à devenir trop volumineuse.

La configuration de référence prévoit environ **six mois d'historique actif** et un seuil d'avertissement à **850 lignes**. Ce nombre est un garde-fou du projet, pas une promesse sur les limites commerciales actuelles d'Airtable : vérifiez les limites de votre forfait et ajustez le seuil si nécessaire.

Avant toute purge, le système doit toujours : exporter les anciens repas, vérifier que l'export est récupérable, demander une confirmation explicite, puis seulement supprimer les anciennes données concernées.

## Ce qu'il faut pour l'utiliser

La version la plus simple ne demande pas de développer une application.

Il vous faut :

1. un compte Airtable ;
2. une base contenant les quatre tables décrites dans [`docs/airtable-schema.md`](docs/airtable-schema.md) ;
3. quelques recettes de départ ;
4. au moins un type de semaine ;
5. un assistant IA capable de lire et d'écrire dans Airtable, ou votre propre intégration via l'API Airtable.

Le prompt générique fourni dans [`prompts/assistant-instructions.md`](prompts/assistant-instructions.md) contient les règles métier principales.

## Démarrage rapide

### 1. Créer la base Airtable

Créez les quatre tables décrites dans [`docs/airtable-schema.md`](docs/airtable-schema.md).

### 2. Créer vos types de semaine

Inspirez-vous de [`airtable/week-types.example.json`](airtable/week-types.example.json). Les effectifs et le premier jour de la semaine sont propres à chaque foyer.

### 3. Ajouter quelques recettes

Le format recommandé pour les ingrédients se trouve dans [`examples/recipe.example.json`](examples/recipe.example.json).

### 4. Configurer l'assistant

Adaptez [`prompts/assistant-instructions.md`](prompts/assistant-instructions.md) avec le nom ou l'identifiant de votre base et votre organisation de semaine.

Ne versionnez jamais vos clés API ou identifiants privés. Le fichier [`.env.example`](.env.example) montre les variables qui peuvent rester locales.

### 5. Préparer votre première semaine

Demandez par exemple :

> Prépare le menu de ma prochaine semaine classique. Regarde les recettes disponibles et évite si possible celles mangées récemment. Ne l'enregistre pas avant que je valide.

Puis ajustez le menu en conversation et validez-le quand il vous convient.

## Structure du dépôt

```text
.
├── README.md
├── LICENSE
├── .env.example
├── .gitignore
├── airtable/
│   ├── schema.example.json
│   └── week-types.example.json
├── docs/
│   ├── airtable-schema.md
│   ├── concepts.md
│   ├── meal-planning-rules.md
│   └── shopping-list-rules.md
├── examples/
│   ├── recipe.example.json
│   └── week.example.json
└── prompts/
    └── assistant-instructions.md
```

## Ce dépôt est une spécification, pas une application imposée

Le projet décrit surtout un **modèle de données et des règles métier**. Vous pouvez l'utiliser avec ChatGPT, un autre assistant, un script maison ou une petite application.

Cette séparation est volontaire : Airtable reste lisible et utilisable directement, même si vous changez ensuite d'outil d'IA.

## Principes du projet

- L'utilisateur garde le dernier mot avant l'enregistrement d'un menu.
- Une recette ne doit pas être dupliquée si elle existe déjà.
- Les anciennes semaines ne doivent pas changer quand un modèle de semaine est modifié.
- Les quantités de recettes restent rattachées à leur nombre de portions d'origine.
- Les restes ne génèrent pas un deuxième achat.
- Les estimations doivent être signalées comme telles.
- Une purge d'historique n'est jamais automatique.
- Les données privées du foyer ne doivent pas être publiées dans ce dépôt.

## Licence

MIT. Vous pouvez reprendre, adapter et redistribuer le modèle.
