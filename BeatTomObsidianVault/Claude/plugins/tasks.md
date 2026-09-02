---
type: reference
tags: [claude, internal, plugins]
plugin-id: obsidian-tasks-plugin
status: actif
---

# Tasks

**id** `obsidian-tasks-plugin` · **Chemin** `.obsidian/plugins/obsidian-tasks-plugin/` · **Données** cases à cocher markdown dans n'importe quelle note, configuration dans `data.json` · indexe les `- [ ]` du vault et les interroge via des blocs de requête `tasks`.

Périmètre dans Beat Tom : les **sous-tâches à l'intérieur d'une carte**. Le suivi de haut niveau appartient au board ([[kanban-bases-view]]), pas à Tasks. Une carte = un fichier de `Board/`, ses étapes = des cases à cocher dans le corps de ce fichier.

## Axes / Config

La donnée vit dans le markdown, pas dans `data.json`. Ce dernier ne contient que des préférences d'affichage et de format d'émoji.

| Élément | Syntaxe |
|---|---|
| Tâche ouverte | `- [ ] libellé` |
| Tâche faite | `- [x] libellé` |
| Échéance | `📅 YYYY-MM-DD` |
| Priorité haute | `⏫` |
| Date de fin | `✅ YYYY-MM-DD` (ajoutée automatiquement à la complétion si l'option est active) |

Bloc de requête :

````
```tasks
not done
path includes Board
sort by due
```
````

## Frontmatter attendu

Aucun. Tasks lit les lignes de case à cocher, pas le frontmatter. Le frontmatter d'une carte reste celui du board.

## Opérations

| Action | Comment |
|---|---|
| Ajouter une sous-tâche | écrire une ligne `- [ ] ...` dans le corps du `.md` de la carte |
| Cocher une sous-tâche | remplacer `- [ ]` par `- [x]` dans le fichier. Ne pas ajouter `✅` à la main sauf si la date de complétion importe. |
| Lister les tâches ouvertes | insérer un bloc ```` ```tasks ```` avec les filtres voulus |
| Reconstruire l'index | l'index est en mémoire et se reconstruit au reload du vault. Aucun fichier à toucher. |

Les `.md` sont éditables à tout moment, y compris Obsidian ouvert. Seul `data.json` relève de la règle de course.

Voir aussi [[kanban-bases-view]], [[core-templates]]
