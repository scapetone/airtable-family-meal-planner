# Prompt de bootstrap Airtable

Ce prompt est destiné à une première installation avec une base Airtable vide déjà accessible par l'assistant.

Copiez-collez le texte ci-dessous dans votre conversation après avoir partagé ce dépôt.

---

Lis le README et les fichiers `docs/airtable-schema.md`, `config.example.json`, `airtable/week-types.example.json` et `prompts/assistant-instructions.md` de ce dépôt.

Tu as accès à une base Airtable que je viens de créer. Commence par inspecter sa structure actuelle.

Si elle est vide, construis le modèle décrit dans `docs/airtable-schema.md` :

- crée uniquement les quatre tables `Recettes`, `Types de semaine`, `Semaines` et `Repas` ;
- crée les champs avec les types et options documentés ;
- crée correctement les liens entre tables ;
- n'ajoute aucune automatisation, vue ou table supplémentaire ;
- ne copie aucune donnée personnelle depuis le dépôt : les exemples sont génériques ;
- relis ensuite le schéma Airtable créé et compare-le au schéma du dépôt.

Après vérification, présente-moi un résumé très court de ce qui a été créé et demande-moi uniquement les informations nécessaires pour personnaliser mes types de semaine : premier jour de la semaine culinaire, repas habituels et nombre de personnes.

N'enregistre pas encore de semaine réelle ni de faux historique.

Quand nous aurons défini mes types de semaine, enregistre-les dans Airtable et considère `prompts/assistant-instructions.md` comme les règles de fonctionnement du projet.

---

## Pourquoi inspecter d'abord la base ?

Le prompt fonctionne aussi si la base n'est plus totalement vide. L'assistant doit éviter de recréer une table ou un champ déjà présent et signaler les différences avant de les modifier.